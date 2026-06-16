---
layout: page
title: Automated Bird Scale
description: This project introduces an proof of concept design for an automated, field-deployable scale designed to track the weight of Red-winged starlings to assist in climate change and urbanization research. The embedded system combines RFID technology for individual bird identification with a precise load cell processing pipeline, automatically logging and syncing data to the cloud with minimal human intervention.
img: assets/img/bird_scale/square_preview.jpeg
importance: 7
category: software & automation
giscus_comments: false
---

## Overview

This project was developed for the Fitzpatrick Institute of African Ornithology at the University of Cape Town (UCT) to assist in ongoing research regarding how urbanization and climate change impact Red-winged starlings. Tracking weight fluctuations is pivotal to understanding these birds' environmental adaptability, but the legacy methodology relied on manual digital scales, field tracking, and human logging, a highly inefficient and time-consuming process. To address this challenge, our group of engineering students designed a proof of concept prototype of an automated, portable embedded system capable of identifying individual birds, accurately capturing their weight, and logging data with minimal human intervention.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bird_scale/fitz_bird.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bird_scale/internals.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bird_scale/preview.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    System evolution from the legacy manual weighing process for Red-winged starlings (left) to the internal electronic prototyping phase (middle) and the final, fully assembled, field-ready automated scale (right).
</div>

## Hardware & Structural Design

The mechanical and sensor hardware consists of a specialized weighing platform integrated with Radio Frequency Identification (RFID) and a custom load cell system. Utilizing a 3kg parallel beam strain gauge load cell coupled with an HX711 24-bit Analog-to-Digital Converter (ADC) amplifier, the system achieves highly precise small-mass measurements. To identify specific birds, low-cost passive integrated transponder (PIT) RFID tags are fitted to the birds, which are scanned by an RFID antenna securely integrated just beneath the weighing platform. The entire device is enclosed in a compact, robust 3D-printed housing engineered to secure the electronics and withstand outdoor environmental factors during campus deployments.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bird_scale/rfid_distance.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bird_scale/rfid_antenna.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bird_scale/3d_print.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Hardware integration overview highlighting the positioning of the RFID antenna and the load cell configuration.
</div>

## Embedded Systems, Data Processing, & Automation

At the core of the system’s data intelligence is a Raspberry Pi Zero W executing a Python-based data processing pipeline. When an RFID tag is detected, the system records the date and time, activates the load cell, and samples the weight data. To counter erratic bird movements, the software captures a series of weight readings over a set period, filters out outliers, and averages the remaining data to establish a precise weight profile. Finally, the integrated dataset (Identity, Weight, Gender, and Timestamp) is compiled into structured Excel logs and automatically synchronized to a cloud-based Google Drive repository via scheduled daily Cron tasks, providing researchers with remote, real-time access to automated summaries.

<div class="row">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bird_scale/larger_diagram.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bird_scale/data_output.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    High-level system architecture and power management block diagram (left) alongside a real-time data stream of filtered load cell weight readings processed by the Raspberry Pi (right).
</div>