---
layout: page
title: Visual Servoing Robot Navigation
description: An evaluation of a featureless, appearance-based visual teach and repeat system for autonomous robot navigation in agricultural crop rows, using direct whole-image correlation to estimate and correct heading deviations along a taught route.
img: assets/img/visual_servo/voyager_preview.jpeg
# redirect: https://unsplash.com
importance: 3
category: robotics
---

## Overview

This project investigates a minimal, featureless appearance-based visual teach and repeat framework for autonomous robot navigation in agricultural crop row environments. Unlike conventional navigation systems that rely on detecting and matching visual landmarks or geometric features, this approach performs localization entirely through direct whole-image comparison between camera frames recorded during an initial teach traversal and those observed during subsequent autonomous repeat traversals. The appeal of such a system lies in its simplicity: by avoiding feature extraction pipelines entirely, it eliminates the associated computational overhead and parameter tuning, while remaining potentially robust to the kinds of appearance changes, like illumination shifts and seasonal variations, that challenge feature-based methods. The work evaluates whether this minimal system can exploit the geometric regularity of agricultural crop rows to achieve reliable autonomous route following.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/visual_servo/voyager_2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/visual_servo/voyager_demo.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Experimental Platform used, equiped with an embedded PC paired with an Nvidia Jetson AGX Orin running ROS2 Humble and Stereolabs ZED X and ZED X One cameras, used for VIO and wide andle images. 
</div>

The system operates in two distinct phases. During the teach phase, a robot is manually teleoperated along a desired route while a monocular wide field of view camera and visual-inertial odometry (VIO) system record paired image and pose measurements. These are stored as vertices in a topometric map, with new vertices appended whenever the robot's displacement from the last stored position exceeds a distance threshold of 0.3m or an angular threshold of 15°.

Before storage, all images undergo a pre-processing pipeline designed to reduce computational load and improve matching robustness. Images are first converted to grayscale and downscaled by a factor of 0.5, reducing resolution from 480×300 to 240×150 pixels. A one-dimensional patch normalisation is then applied, which subtracts the local mean pixel intensity and divides by the local standard deviation across each pixel neighbourhood. This contrast enhancement provides a degree of invariance to illumination changes, an important property in outdoor agricultural deployments where lighting conditions vary throughout the day.

During the repeat phase, the robot initialises its position by computing image correlations between its current camera view and all stored map images, selecting the best match as its starting position. It then autonomously follows the sequence of stored goal poses using a drive-to-pose controller, while two image-based correction modules continuously refine its trajectory in real time.

## Image Correlation and Offset Estimation

The core of the system is a Normalised Cross-Correlation (NCC) computation that slides the current processed camera image horizontally across a stored reference image, producing a correlation profile that scores the similarity between the two images at each pixel offset. The offset corresponding to the peak NCC score is taken as the robot's estimated rotational deviation from the taught path, which is then converted to an angular value using the camera's known horizontal field of view. A high, sharp peak in the correlation profile indicates a confident, reliable match, while a flat profile, which can arise from repetitive scene structure or aggressive image downscaling, indicates ambiguity and a higher likelihood of an incorrect estimate.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/visual_servo/correlation_profile.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of the Correlation Profile obtained from two rotationally offset images Image (a) shows the reference image that is being compared to, where the red vertical line shows the centre of the image. Image (b) shows the query image where the green vertical line indicates the position of the maximum NCC score. The plot shown in (c) shows the NCC calculated at varying horizontal pixel offsets between the two images.
</div>

To estimate the robot's rotational deviation from the taught path, the system interpolates between the NCC offsets computed for the preceding and following goal frames. As the robot moves forward, scene content naturally shifts outward in the image; interpolating between frames cancels this expected motion, isolating only the component of image shift attributable to heading error. Similarly, an along-path correction module computes correlation peaks across a local window of nearby map vertices and uses a weighted average of their peak scores to estimate whether the robot is running ahead of or behind its expected position along the route, adjusting the goal pose accordingly.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/visual_servo/orientation_correction.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/visual_servo/translation_correction.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of corrective operations during repeat traversals. Goal poses are rotated (left) and translated (right) to correct for orientation and translational offsets observed. 
</div>

## Some Results

The framework was evaluated on a commercial blueberry farm in Paarl, Western Cape. The test route consisted of blueberry bushes planted in pots along either side of the path formed a semi-structured visual corridor beneath a mesh netting canopy. The relatively flat terrain was considered well-suited to the system, which assumes a flat world and accounts only for horizontal image displacements. The corridor structure was expected to provide strong and consistent visual cues for appearance-based navigation.

Initially the system performed well, correctly localizing along the taught path and successfully matching all previously stored map frames without suffering from perceptual aliasing, a notable result given the highly repetitive visual appearance of the crop rows. However, a gradual lateral drift developed over the course of the traversal.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/visual_servo/zoomed_in_bio_vtr_updated.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of the gradual lateral offset developing over a crop row traversal. 
</div>

The system is able to mostly correct for orientation offsets, but struggles to deal with rich texture environments like that of farm environments, unlike indoor and structured environments, the floor and foliage have very similar textures resulting in an aliasing between the two and hence the gradual lateral drift.

Looking at the robot's estimated versus measured rotational offset confirmed that the correlation-based heading estimation was functioning correctly throughout, with the two signals tracking each other closely. Small oscillatory corrections in heading were also observed, arising from slight over-corrections by the orientation module being compensated for in subsequent frames, a characteristic sinusoidal motion about the intended path. These oscillations, while not significantly altering the overall direction of travel, reduced forward velocity and smoothness of traversal.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/visual_servo/teach_norm.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/visual_servo/est_exp_fig.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of a processed image from a crop row traversal (left), note the similar textures on the floor and folliage. And an illustration of the system accurately estimating the rotational offset experienced throughout a crop row traversal (right).
</div>
