---
layout: page
title: Adhesion Testing Machine Modernisation
description: This project involved redesigning an industrial adhesion testing machine to provide precise motor control, automated force measurement, and an intuitive operator interface. Built around a multi-core ESP32 and custom PCB hardware, the system enabled smooth real-time control and data acquisition within a compact embedded platform.
img: assets/img/nutec/instron_preview.jpeg
importance: 5
category: software & automation
---

During an industrial placement at NUTEC Digital Ink, I was tasked with modernising an adhesion testing machine used to measure laminate and label adhesion strength. The existing system provided only basic force measurements, requiring operators to manually record peak values. The goal was to improve usability, automate data collection, and provide researchers with greater control over testing procedures.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nutec/inside_instron.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nutec/demo_video.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nutec/display_view.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The modernised testing machine, featuring a stepper motor-driven linear actuator, load cell, and a custom touchscreen user interface for control and real-time data visualisation.
</div>


## Human-Machine Interface Design

A significant aspect of the project involved designing a user interface that could be operated intuitively by researchers and technicians with varying levels of technical expertise. The interface was developed around a clear workflow that allowed users to configure tests, control actuator movement, monitor force measurements, and export collected data.

To minimise processing load on the control hardware, the interface was implemented using a state-based architecture, where the display handled the majority of user interaction and visualisation tasks independently. This enabled responsive navigation while providing real-time graphical feedback of force measurements and machine status. The resulting interface simplified operation of the machine and significantly improved the accessibility of experimental data.

## Real-Time Control Using a Multi-Core ESP32

The machine utilised a stepper-motor-driven linear actuator to apply controlled tensile forces while simultaneously measuring load-cell data. Achieving smooth actuator motion while recording force measurements presented a real-time processing challenge, as both tasks had to execute concurrently without affecting one another.

To address this, the system was built around a multi-core ESP32 microcontroller. Motion control and force acquisition were separated across processor cores, allowing the actuator to maintain smooth movement while force data was sampled and logged in real time. This architecture enabled precise speed and position control while continuously recording force measurements throughout each test. Logged data could then be stored, visualised on the interface, and exported via microSD card or email for further analysis.

## Custom PCB Development

To integrate the various electronic components into a reliable and maintainable system, a custom breakout PCB was designed. The board consolidated connections between the ESP32, motor control hardware, sensors, and user interface while fitting within the existing machine enclosure.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/nutec/pcb_new.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/nutec/pcb_design.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Breakout PCB for the ESP32 microcontroller.
</div>

Particular attention was given to serviceability and future development, ensuring that the microcontroller remained easily accessible for firmware updates and system modifications. 

