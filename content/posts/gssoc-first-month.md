---
title: "GSSoC: First Month Summary"
date: 2022-04-03T11:28:35+05:30
draft: false
tags: ["gssoc", "flutter", "open-source"]
keywords: ["gssoc", "flutter", "open-source"]
description: "This post will walk you through the open source contribution done in **GirlScript Summer of Code 2022** in [Smart-Home-App](https://github.com/Lakhankumawat/smart-home-app) repository for the very first month."
readingTime: true
---

The first month mainly comprised of understanding the code base and alongside developing the front-end of the mobile application using Dart and Flutter framework.

Following is a breif description of the designs I got to implement during this first month, the challenges that came along and solutions to them. 

---

## Popups Design

This design section mainly consisted of icons to alert or inform the user about an event. These popups had the following forms as visible in the attached reference.

<!-- Popups Design -->
{{< figure src="/img/gssoc-first-month/popup-ui-design.png" alt="Popups Design" caption="Popups Design" style="border-radius: 2px;">}}

The following hybrid inheritance relationship was established based on the common features there were in between these different forms, and is depicted using the image below.

<!-- Inheritance Relationship -->
{{< figure src="/img/gssoc-first-month/work-tree-hierarchy.png" alt="Inheritance Relationship" caption="Inheritance Relationship" style="border-radius: 2px;" >}}

---

## Set Events Screen

Now was the time for the development of a screen that would be part of the mobile application, and involved the usage of popup design that was implemented before. The design can be viewed below.

<!-- Set Events Screen -->
{{< figure src="/img/gssoc-first-month/set-event-screen.png" alt="Set Event Screen Design" caption="Set Event Screen Design" position="center" style="border-radius: 2px;">}}

This was my first interaction with packages in Flutter. Here, I got to use **table_calender** package which offered a great degree of customisability in style.

Moreover, the role of popups came into picture while handling edge cases in utilisation. For instance, if an new event was to be added, but a day wasn't selected, or, if everything was executed correctly and the event was succesfully added to the selected day.

<!-- Squished TextFormField -->
{{< figure src="/img/gssoc-first-month/form-field-squished.png" alt="Squished TextFormField" caption="Squished TextFormField" position="center" style="border-radius: 2px;">}}

A problem, however, was encountered during the development phase, where the TextFormField would get squished when the keyboard would appear. This made it impossible to edit and view what was being written.

This was solved by setting the **resizeToAvoidBottomInset** property to false, allowing the keyboard to slide over the alert dialog without squeezing it.

<!-- Stretched TextFormField -->
{{< figure src="/img/gssoc-first-month/form-field-stretched.png" alt="Stretched TextFormField" caption="Stretched TextFormField" position="center" style="border-radius: 2px;">}}

---

## Winding Up

That is all for this post. If you found the work interesting and wish to take a glance at the source code, hop over to:

- [Popup Design](https://github.com/Lakhankumawat/smart-home-app/tree/master/lib/popups)
- [Set Events Screen](https://github.com/Lakhankumawat/smart-home-app/tree/master/lib/src/screens/set_event_screen)