---
title: "GSSoC - First PR: Implement Popup Design"
date: 2022-04-03T11:28:35+05:30
draft: false
tags: ["gssoc", "flutter", "open-source"]
keywords: ["gssoc", "flutter", "open-source"]
description: "This article walks through the open source contribution done in **GirlScript Summer of Code** against [Smart-Home-App](https://github.com/Lakhankumawat/smart-home-app) repository for the very first PR. The coming challenges and their possible solutions are also discussed along the way."
readingTime: true
---

Through this PR, I got the opportunity to implement the UI design for the popups in flutter that would then be used in various other parts of the application.

<!-- Popup UI Design Image -->
{{< figure src="/img/gssoc-first-pr/popup-ui-design.png" alt="Popups UI Design" caption="Popups UI Design" style="border-radius: 2px;">}}

As the work progressed, in order to accommodate changes due to upcoming needs, refactoring was done and it finally ended up in the work tree hierarchy shown below.

<!-- Work Tree Hierarchy Image -->
{{< figure src="/img/gssoc-first-pr/work-tree-hierarchy.png" alt="Work Tree Hierarchy" position="center" caption="Work Tree Hierarchy" style="border-radius: 2px;" >}}

---

## Challenges Encountered

While building the PopupState widget, we made use of the ListTile widget, and came across the fact that a subtitle may not be preferred with every title, and hence ended up making that property nullable, and used an empty string as a fallback, if no argument was passed against the popupSubtitle parameter.

```java
ListTile(
    title: Padding(
        padding: const EdgeInsets.only(bottom: 8),
        child: Text(
            popupTitle,
            style: const TextStyle(
                fontFamily: 'Lexend',
                fontSize: 20,
            ),
            textAlign: TextAlign.center,
        ),
    ),
    subtitle: Text(
        popupSubtitle ?? '',
        textAlign: TextAlign.center,
    ),
);
```

But as a result, if the popupSubtitle did turn out to be null, in the UI, the ListTile would reserve some space for the subtitle, which would appear as a prominent gap, as in the below snip.

{{< figure src="/img/gssoc-first-pr/reserved-subtitle-space.png" alt="Reserved Subtitle Space" position="center" caption="Reserved Subtitle Space" >}}

---

## Proposed Solution

The solution to this was to introduce a function that, based on the value of subtitle, would decide whether to return null or the subtitle wrapped inside the Text widget. The following snippet illustrates the solution.

```java
Widget? _judgeSubtitle() {
    return popupSubtitle == null
        ? null
        : Text(
            popupSubtitle!,
            textAlign: TextAlign.center,
          );
}
```

And, the complete code would then look like,

```java
ListTile(
    title: Padding(
        padding: const EdgeInsets.only(bottom: 8),
        child: Text(
            popupTitle,
            style: const TextStyle(
                fontFamily: 'Lexend',
                fontSize: 20,
            ),
            textAlign: TextAlign.center,
        ),
    ),
    subtitle: _judgeSubtitle(),
);
```

Ultimately, the ListTile no longer reserves space for the subtitle and the output looks as desired.

{{< figure src="/img/gssoc-first-pr/subtitle-space-vanished.png" alt="Subtitle Space Vanished" position="center" caption="Subtitle Space Vanished" >}}

That is all for this article. If you found it helpful and wish to go over the source code, you can hop over to [Source Code](https://github.com/Lakhankumawat/smart-home-app/tree/master/lib/popups).
