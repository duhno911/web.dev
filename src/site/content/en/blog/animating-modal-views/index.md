---
layout: post
title: Animating Modal Views
subhead: Modal views block the user interface to display important messages. Learn how to animate modal views in your apps.
authors:
  - paullewis
date: 2014-08-08
updated: 2019-08-07
description: Learn how to animate modal views in your apps.
tags:
  - blog # blog is a required tag for the article to show up in the blog.
---

  <figure>
    {% Img src="image/T4FyVKpzu4WKF1kBNvXepbi08t52/IysQk9ksWLJvaV3KhS83.gif", alt="Animating a modal view.", width="320", height="405" %}
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/animations/modal-view-animation.html" target="_blank" class="external">Try it</a>
    </figcaption>
  </figure>

Modal views are for important messages, and for which you have very good reasons to block the user interface. Use them carefully, because they're disruptive and can easily ruin the user’s experience if overused. But, in some circumstances, they’re the right views to use, and adding some animation will bring them to life.

### TL;DR
* Use modal views sparingly; users get frustrated if you interrupt their experience unnecessarily.
* Adding scale to the animation gives a nice "drop on" effect.
* Get rid of the modal view quickly when the user dismisses it. However, bring the modal view onto the screen a little more slowly so that it doesn't surprise the user.


The modal overlay should be aligned to the viewport, so set its `position` to `fixed`:

```css
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

  pointer-events: none;
  opacity: 0;

  will-change: transform, opacity;
}
```

It has an initial `opacity` of 0, so it's hidden from view, but then it also needs `pointer-events` set to `none` so that clicks and touches pass through. Without that, it blocks all interactions, rendering the whole page unresponsive. Finally, because it animates its `opacity` and `transform`, those need to be marked as changing with `will-change` (see also [Using the will-change property](https://developers.google.com/web/fundamentals/design-and-ux/animations/animations-and-performance#using-the-will-change-property)).

When the view is visible, it needs to accept interactions and have an `opacity` of 1:

```css
.modal.visible {
  pointer-events: auto;
  opacity: 1;
}
```

Now whenever the modal view is required, you can use JavaScript to toggle the "visible" class:

```js
modal.classList.add('visible');
```

At this point, the modal view appears without any animation, so you can now add that in
(see also [Custom Easing](https://developers.google.com/web/fundamentals/design-and-ux/animations/custom-easing)):

```css
.modal {
  transform: scale(1.15);

  transition:
    transform 0.1s cubic-bezier(0.465, 0.183, 0.153, 0.946),
    opacity 0.1s cubic-bezier(0.465, 0.183, 0.153, 0.946);
}
```

Adding `scale` to the transform makes the view appear to drop onto the screen slightly, which is a nice effect. The default transition applies to both transform and opacity properties with a custom curve and a duration of 0.1 seconds.

The duration is pretty short, though, but it's ideal for when the user dismisses the view and wants to get back to your app. The downside is that it’s probably too aggressive for when the modal view appears. To fix this, override the transition values for the `visible` class:

```css
.modal.visible {
  transform: scale(1);

  transition:
    transform 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946),
    opacity 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946);
}
```

Now the modal view takes 0.3 seconds to come onto the screen, which is a bit less aggressive, but it is dismissed quickly, which the user will appreciate.

