---
title: "GSSoC: Second Month Summary"
date: 2022-05-01
draft: false
tags: ["gssoc", "flutter", "open-source"]
keywords: ["gssoc", "flutter", "open-source"]
description: "In this post, I'll be briefing about the work involved in the second month of open source contribution in **GirlScript Summer of Code 2022**."
readingTime: true
---

This month comprised of the development of the **Stats** Screen, which would show the user, the statistics of the electricity usage by appliances, daily, weekly or monthly. The design can be viewed below.

<!-- Stats Screen -->
{{< figure src="/img/gssoc-second-month/stats-screen.png" alt="Stats Screen" caption="Stats Screen" position="center" style="border-radius: 2px;">}}

---

## Implementation

The implementation of the design was achieved with the help of a flutter package **syncfusion_flutter_charts** which is a data visualisation library that plots the graph based on the provided data. Moreover, it also allows flexibility with the chart selection, be it, cartesian, line, bar, spline, etc.

However, in this case, I chose the Spline Chart for their ease of data representation. Below is visual representation of a sample data using spline curve.

<!-- Spline Curve -->
{{< figure src="/img/gssoc-second-month/spline-curve.png" alt="Spline Curve" caption="Spline Curve" style="border-radius: 2px;">}}

---

## Winding Up

Following is a snippet of the final implementation of the design as viewed from the android emulator.

<!-- Stats Usage -->
{{< figure src="/img/gssoc-second-month/stats-usage.png" alt="Stats Usage" caption="Stats Usage" position="center" style="border-radius: 2px;">}}

And, that will be all for this post. If you found the work interesting and wish to take a glance at the source code, hop over to [Source Code](https://github.com/Lakhankumawat/smart-home-app/tree/master/lib/src/screens/stats_screen).