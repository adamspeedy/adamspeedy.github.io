---
layout: page
title: Localizing using Neural Radiance Fields (NeRF)
description: An investigation into the the use of Neural Radiance Fields (NeRFs) for visual localisation. NeRF can create photo-realistic views from previously unseen viewpoints, the goal was to investigate whether these rendered views could be used as a map for appearance-based localisation.
img: assets/img//NeRF/NeRF_presentation.jpeg
importance: 4
category: robotics
---

## Overview

This project investigated whether Neural Radiance Fields (NeRFs) could be used as a map representation for visual localisation. Rather than localising against a traditional image database, the aim was to determine whether a robot could localise itself using images rendered from a NeRF model of an environment.

NeRFs are neural networks capable of reconstructing a 3D scene from a collection of 2D images and synthesising highly realistic views from previously unseen viewpoints. Their ability to generate photo-realistic images makes them an attractive candidate for robotic navigation, where visual appearance plays a key role in localisation.

Using captured classroom image and pose data from a University of Cape Town, various NeRF models were trained using the nerfstudio framework. Novel views were then rendered throughout the environment and used as a reference map for appearance-based localisation.

<div class="row">
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/NeRF/Un-Cropped-Image.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/NeRF/Nerfacto-Render-1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example images of a captured image of the UCT classroom (left) and a NeRF redured image of the robot in a similar position of the classroom (right). 
</div>

## How Neural Radiance Fields Work

A Neural Radiance Field represents a scene as a continuous neural network rather than an explicit geometric model. The network learns a mapping between a 3D position and viewing direction and the colour and density of light observed from that location.

Once trained on a collection of images captured from different viewpoints, the model can render realistic images from arbitrary camera poses. This process, known as novel view synthesis, enables the generation of viewpoints that were never directly observed during training.

The ability to render realistic images from any position makes NeRFs particularly interesting for robotics, as they could potentially serve as compact, view-generating maps for localisation and navigation tasks.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/NeRF/NeRF_Algorithm.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of the process of synthesising an image by feeding a 5D coordinate (spatial location and viewing direction) into the neural network to produce the colour and volume density. This is then projected into an image using volume rendering techniques and further optimised to create the specific viewpoint (Image credit Mildenhall et al.).
</div>

## Methodology

To evaluate whether rendered NeRF images could support localisation, a Visual Bag of Words (BoW) pipeline was developed.

The process began by extracting ORB visual features from a set of rendered NeRF images. These features were clustered to create a vocabulary of visual words representing common visual patterns within the environment.

Both the real camera images and the rendered NeRF images were then converted into frequency histograms describing the occurrence of these visual words. By comparing these histograms, the system could determine which rendered NeRF view most closely matched a query image and estimate the camera's location within the environment.

The pipeline therefore enabled direct localisation of real-world camera images against views generated entirely from a Neural Radiance Field.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/NeRF/BOW_Pipiline.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Results

The project demonstrated that traditional feature-based localisation techniques can operate on rendered NeRF imagery. ORB features extracted from rendered views showed sufficient consistency with features extracted from real images to allow meaningful place recognition.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid path="assets/img/NeRF/more_training_poses.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid path="assets/img/NeRF/ngp-ORb.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Illustration of training images used for training a NeRF model using nerfstudio(left). Example of extracted features from a rendured NeRF image, using the nerfacto mdoel (right).
</div>

The localisation system successfully identified the correct location within 10 frames of ground truth in 63.6% of test cases, demonstrating that NeRF-generated imagery can provide useful information for appearance-based localisation.

The results also highlighted several limitations. Localisation performance was strongly dependent on the quality of the NeRF reconstruction, which in turn relied heavily on the coverage and quality of the training images. Additional image preprocessing and dataset refinement were required to maximise reconstruction accuracy and improve localisation performance.
