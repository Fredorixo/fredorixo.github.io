---
title: "Blog Automation"
date: 2023-05-17T13:32:36+05:30
draft: false
tags: ["git", "github"]
keywords: ["git", "github"]
description: "Hey folks, in this post, I'll be walking you, through the steps, I took to automate the blog that you're reading right now. So what are your waiting for, hop in!"
readingTime: true
---

There could be various work-arounds for automating the blog posting process, but the one I'll be discussing is using **Github Actions**.

So throughtout this post, I'll be taking reference of my workflow and explaining what each line is responsible for performing.

---

## Workflow

Following is the code for the workflow written in YAML format. 

```yaml
name: Generate Build and deploy to Github Pages

on:
  push:
    branches:
      - master

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.108.0
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb

      - name: Install Node Dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"

      - name: Build Webpage
        env:
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: hugo --minify

      - name: Store Build in Artifact
        uses: actions/upload-pages-artifact@v1
        with:
          name: github-pages-artifacts
          path: ./public
  
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    permissions:
      pages: write
      id-token: write
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
        with:
          artifact_name: github-pages-artifacts
```

> NOTE: Indentation in YAML file can make a big difference. Hence, it is important to follow the same indentation as in the above code segment.

---

## Explanation

I'll be explaining the workflow in a step-wise manner, as below:

1. At the very beginning, a **name** is given to every workflow, "Generate Build and deploy to Github Pages" in this case.

    ```yaml
    name: Generate Build and deploy to Github Pages
    ```

2. The **on** keyword lets you define events that would trigger the workflow. Here it is a **push** event on that master branch.

    ```yaml
    on:
      push:
        branches:
          - master
    ```

3. By default, we have configured the workflow to exectue bash commands.

    ```yaml
    defaults:
      run:
        shell: bash
    ```

4. In the following code, we divide our workflow into two jobs: **build** and **deploy**, and both run on ubuntu-latest.

5. In the build job, we define 5 steps, in the following sequence:

    1. Checkout: In this step, we clone the contents of our repository onto the Virtual Machine, which is running ubuntu, as per our previous specification. This is achieved using a pre-defined Github Action named **checkout**.

        ```yaml
        - name: Checkout
          uses: actions/checkout@v3
        ```
    
    2. Install Hugo CLI: Since the VM doesn't already have Hugo installed on it, we've to perform it.

        ```yaml
        - name: Install Hugo CLI
          run: |
            wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
            && sudo dpkg -i ${{ runner.temp }}/hugo.deb
        ```

    3. Install Node Dependencies: This is crucial since the theme being used by the blog is making use of Node packages.

        ```yaml
        - name: Install Node Dependencies
          run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
        ```

    4. Build Wepage: In this step, we generate the static files with the help of Hugo CLI, installed in the previous steps.

        ```yaml
        - name: Build Webpage
          env:
            HUGO_ENVIRONMENT: production
            HUGO_ENV: production
          run: hugo --minify
        ```

    5. Store Build in Artifact: At the final step, we need to store these generated static files in a reserve, to be able to access them at the time of deploying. This too is achieved using a pre-defined Github Action, and the reserve is given the name of **github-pages-artifacts**.

        ```yaml
        - name: Store Build in Artifact
          uses: actions/upload-pages-artifact@v1
          with:
            name: github-pages-artifacts
            path: ./public
        ```

6. Now it is time to execute the deploy job. Inorder to ensure that that **deploy** job executes only after the build job, since it requires the static files for deployment, we add in the "needs" property.

    ```yaml
    needs: build
    ```

7. Moreover, to be able to deploy the static files, we also need to provide write permissions.

    ```yaml
    permissions:
      pages: write
      id-token: write
    ```

8. Finally, using the pre-defined **deploy-pages** Github Action, and providing the static files stored inside github-pages-artifacts, we preform the deployment.

    ```yaml
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
        with:
          artifact_name: github-pages-artifacts
    ```

9. And, then, it is published to the desired URL which then becomes visible publicly.

    ```yaml
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    ```

---

## Winding Up

That is all for this post. If you found it helpful, do share it among your peers. Also, if you wish to deep dive into Github Actions check out Github's official documentation on the following [link](https://docs.github.com/en/actions).