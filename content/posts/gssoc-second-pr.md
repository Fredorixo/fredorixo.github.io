---
title: "GSSoC - Second PR: Develop Set Event Screen"
date: 2022-04-04T20:01:43+05:30
draft: true
tags: ["gssoc", "flutter", "open-source"]
keywords: ["gssoc", "flutter", "open-source"]
description: "This article walks through the open source contribution done in **GirlScript Summer of Code** against [Smart-Home-App](https://github.com/Lakhankumawat/smart-home-app) repository for the very first PR. The coming challenges and their possible solutions are also discussed along the way."
readingTime: true
# lastModified: 2022-04-06
---

During the first month of contribution to [Smart-Home-App](https://github.com/Lakhankumawat/smart-home-app) repository, the designing phase was ongoing wherein contributors had to implement designs, and I took this opportunity to implement the Set Event Screen of the application. A snippet of the design is presented below.

<!-- Set Event Screen UI Design -->
{{< figure src="/img/gssoc-second-pr/set-event-screen.png" alt="Set Event Screen UI Design" caption="Set Event Screen UI Design" position="center" style="border-radius: 2px;">}}

This was my very first thorough experience with flutter packages, and in this pull request I got to work with [table_calendar](https://pub.dev/packages/table_calendar) package, for building calendars wherein one can save events to look up to later on.

## Table Calendar

While working with the table calendar, I initially set both the focusedDay and the selectedDay to `DateTime.now()`. But due to this, when the user would tap on `Add New Event` and add new ones, those wouldn't get registered within the map that was being maintainted, but would work perfectly fine after a new day was selected. The gif below illustrates the issue.

<!-- GIF of the issue -->

The **hack** that I found to this was to make the selectedDay property nullable, and leaving it unassigned initially until a day was tapped on. To ensure that the day was selected first and events were added to it afterwards, a check was maintained over the value of selectedDay.

If it was null, an alert dialog would popup informing the user that a day had to be selected. Otherwise, it would let the user set an event.

<!-- GIF of resolved -->

## Alert Dialog

Another challenge that I came across was the alert dialog being squished by the