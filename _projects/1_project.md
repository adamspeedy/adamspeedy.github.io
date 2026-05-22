---
layout: page
title: Long Term Navigation of Ground Robots
description:  An investigation into methods of visual autonomous navigatino for use in agricultural environments. With a further focus on integrating multiple experiences of the same place at differnt times to improve robustness against appearance variation. 
img: assets/img/long_term_nav_preview.jpg
importance: 1
category: robotics
related_publications: true
---

This work, as part of my masters thesis, investigated the viability of achieving meaningful autonomy in agricultural settings using solely visual methods. Agricultural environments experience significant visual changes both as a result of changing lighting over a single day and larger appearance changes resulting from changing season or normal farming activities.

To address these challenges, this work evaluates a landmark-based teach and repeat paradigm which uses feature triangulation for localization. With a further focus on the integration of multiple visual experiences of the same route, captured at different times, to improve robustness against substantial appearance variation.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/long_term_nav/another_view.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/long_term_nav/charging_a300.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/long_term_nav/rain_a300.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="images/long_term_nav/a300.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

The proposed methods are deployed and evaluated on Clearpath Husky robotic platforms in real-world crop row environments. Experiments assess path tracking performance, and failure modes under varying illumination and weather conditions. Additionally, testing whether the
use of multiple visual experiences result in long-term gains to navigation performance

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/long_term_nav/single_exp_inliers.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/multi_exp_inliers.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The results demonstrate that the such an approach successfully navigates under most conditions, with failures primarily occurring under extreme lighting and substantial appearance changes in the order of months. The incorporation of multiple visual experiences is shown to enhance navigation reliability, enabling successful traversal in scenarios where using a single taught experience failed, effectively extending the window of reliable operation.


The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
