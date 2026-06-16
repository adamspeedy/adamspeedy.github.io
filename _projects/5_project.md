---
layout: page
title: Line-Following Maze-Solving Robot
description: An autonomous line-following robot designed and built to navigate and solve a black-line maze, using custom-designed sensor circuitry and an Arduino Nano 33 IoT microcontroller. The project spanned hardware design, circuit fabrication, motion control simulation, and software algorithm development.
img: assets/img/line_following/lil_john_preview.jpeg
importance: 6
category: robotics
---

As part of an engineering course, I worked in a group of three to design and build an autonomous line-following robot capable of navigating and solving a black-line maze. The robot was built around an Arduino Nano 33 IoT microcontroller and consisted of a metal chassis, two motorised wheels with a dedicated motor driver, four infrared line sensors, and custom-designed Veroboard circuits for light sensing and communication.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/line_following/lil_john.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/line_following/demo_2.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/line_following/group_photo.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>

A key hardware challenge was designing a light colour sensor circuit capable of reliably distinguishing between red and green light signals, which were used to communicate information to the robot. After several design iterations — beginning with a simple LDR comparator circuit that suffered from false triggers due to ambient light fluctuations — the final design paired each colour LDR with its own ambient reference LDR in a voltage divider configuration. A potentiometer allowed dynamic calibration, and a single LM358 op-amp IC provided stable comparisons at the 3.3V logic level required by the Arduino. A voltage regulator circuit using an LM317T was also designed to step down the battery supply to a stable 3.3V rail.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/line_following/ldr_circuit.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/line_following/ldr_diagram.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Image of veroboard circuit (left) used to detect the start and stop light signals, and corresponding diagram (right).
</div>

The robot's motion control was initially developed and validated through MATLAB Simulink simulations, modelling straight-line travel and rotation before deployment to hardware. Rather than relying on wheel encoders for distance measurement, the final implementation used timed motor commands, which proved sufficiently accurate for maze navigation and freed up Arduino pins for other peripherals. Motor direction and speed were controlled via PWM signals, with the robot able to rotate on the spot, an important capability given the tight spatial constraints of the maze.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/line_following/line_following_sim.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Subsystem simulation of line following using MATLAB Simulink. If the line sensor detects a line, it will continue in that trajectory until it does not sense a line, in which case it will rotate until it finds the line again.
</div>

The maze-solving algorithm was based on the left-hand rule: always turn left when given a choice. Using four infrared sensors arranged across the front of the robot, the algorithm used sensor array states, represented as binary arrays such as [1 0 0 1] for line-following, to distinguish between straight paths, curves, four-way junctions, T-junctions, river crossings, and dead ends. A MATLAB Stateflow diagram drove the decision-making logic, with dedicated states handling each junction type and built-in redundancy to recover from lost lines. In testing, the robot successfully solved the maze across multiple runs, demonstrating the robustness of both the hardware design and the software algorithm.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/line_following/state_flow.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of the MATLAB Simulink stateflow, that is ported to the Arduino to control the robot based on the sensor inputs. 
</div>
