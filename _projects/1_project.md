---
layout: page
title: Long Term Navigation of Ground Robots
description:  An investigation into methods of visual autonomous navigatino for use in agricultural environments. With a further focus on integrating multiple experiences of the same place at differnt times to improve robustness against appearance variation. 
img: assets/img/long_term_nav/preview_a300.jpeg
importance: 1
category: robotics
related_publications: false
---

## Overview
Achieving reliable autonomous navigation in unstructured outdoor environments remains a central challenge in field robotics. Agricultural settings are particularly demanding: crop rows provide only a narrow operational corridor, while the environment undergoes continuous visual change, from shifting shadows throughout the day to large appearance variations caused by seasonal changes, crop growth, and routine farming activities.

This project investigates whether a vision-based navigation system can provide long-term autonomy in these conditions and explores how leveraging multiple prior traversals of the same route can extend the period of reliable operation.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/long_term_nav/rain_traversal.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/long_term_nav/sunny_traversal.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/long_term_nav/charging_a300.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A Clearpath Husky A300 operating on a blueberry farm in Paarl, Western Cape, South Africa, under active rainfall (left) and bright midday sunlight (centre). The robot autonomously navigates between crop rows using only stereo camera input.
</div>

## Backround
Most commercial autonomous vehicles rely heavily on GPS or dedicated infrastructure. In dense crop rows, however, GPS signals can be unreliable, while the installation and maintenance of infrastructure is often impractical for farming operations.

A vision-only approach is therefore both attractive and technically challenging. The key difficulty is not simply following a known route, but recognising that route when its appearance has changed significantly since it was first taught. Illumination changes are particularly problematic: the same crop row captured at dawn, midday, and dusk can appear dramatically different to a feature-based localisation system.


## Teach & Repeat
The navigation framework used in this work follows a Visual Teach & Repeat (VT&R) paradigm. During the teach phase, the robot is manually driven along a desired route while building a map of visual landmarks. During the repeat phase, the robot autonomously re-traverses the route by localising itself against this stored map in real time.

This work builds upon the VT&R3 framework developed by the Autonomous Space Robotics Lab (ASRL) at the University of Toronto Institute for Aerospace Studies (UTIAS).

Localisation is achieved through feature matching and triangulation. By tracking visual landmarks across successive stereo images, the system estimates its deviation from the taught path and generates corrective steering commands. The approach relies solely on stereo vision, making it lightweight and deployable on affordable hardware.

<div class="row">
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/long_term_nav/pipeline_diagram.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/long_term_nav/landmark_migration.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of the VT&R pipeline (left). Visual features are extracted and triangulated from stereo images before being matched against a stored submap within the relative pose graph. The pose graph structure is illustrated on the right with landmark locations being migrated to the current goal frame.
</div>



## Multi-Experience Integration
A single taught experience captures the environment at one moment in time. When conditions differ substantially from that snapshot, due to seasonal changes, varying canopy cover, or significant lighting differences, the number of trackable landmarks decreases, causing localisation performance to degrade or fail.

To address this limitation, this work investigates the integration of multiple visual experiences of the same route, each recorded under different environmental conditions. Rather than replacing the original map, these experiences supplement it. During navigation, the system can localise against whichever experience provides the strongest visual correspondence to current conditions.

This multi-experience approach acts as a form of environmental memory, allowing the robot to maintain localisation across a much wider range of appearance changes than would be possible with a single experience alone.


<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/long_term_nav/single_exp_inliers.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/long_term_nav/multi_exp_inliers.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">    
    Feature-tracking visualisation using a single taught experience (left) and multiple experiences (right). line correspondences indicate matched landmarks between the live camera image and stored landmarks in map. Access to multiple experiences increases the likelihood of finding reliable matches and maintaining localisation.
</div>


## System Development and Deployment
A major component of this project involved developing and deploying the complete navigation stack on a Clearpath Husky platform. This included migrating the VT&R3 codebase to ROS 2 Jazzy, integrating a new sensor suite, and incorporating a visual-inertial odometry (VIO) prior to improve mapping performance in uneven terrains.

Field deployment on a working blueberry farm in Paarl, Western Cape, introduced challenges beyond the software itself. Testing had to be coordinated around active farming operations while maintaining reliable system performance in dusty, wet, and highly variable environmental conditions.

Evaluation was conducted across multiple field visits spanning different seasons and times of day. Each deployment provided valuable insight into real-world failure modes and informed iterative improvements to both the software and hardware systems.


## Results
The proposed approach demonstrated reliable autonomous navigation across the majority of tested conditions, with path-tracking performance remaining within practical limits for agricultural operation.

Experiments showed that the single-experience landmark-based navigation framework could successfully repeat routes under most conditions. Failures primarily occurred during periods of extreme illumination change or significant appearance variation caused by seasonal transitions.

The incorporation of multiple visual experiences substantially improved robustness by extending the period of successful operation from hours to days. In several cases, the robot successfully completed routes that could not be navigated using a single taught experience alone.

These results highlight the importance of experience diversity for long-term visual navigation and demonstrate that multi-experience mapping can significantly improve navigation reliability in changing agricultural environments.

