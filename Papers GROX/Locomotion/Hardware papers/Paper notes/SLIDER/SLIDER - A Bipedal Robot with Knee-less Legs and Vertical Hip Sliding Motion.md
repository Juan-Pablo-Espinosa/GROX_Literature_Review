---
title: "SLIDER: A Bipedal Robot with Knee-less Legs and Vertical Hip Sliding Motion"
aliases:
  - SLIDER
  - SLIDER CLAWAR
authors:
  - "[[Ke Wang]]"
  - "[[Aksat Shah]]"
  - "[[Petar Kormushev]]"
corresponding: ""
institutions:
  - "[[Imperial College London]]"
labs:
  - "[[Robot Intelligence Lab]]"
countries:
  - UK
year: 2018
date: ""
venue: CLAWAR 2018 (21st International Conference on Climbing and Walking Robots)
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: http://www.imperial.ac.uk/robot-intelligence/robots/slider/
pdf: ""
code: "No code (design paper, checked 2026-11-02)"
project_page: http://www.imperial.ac.uk/robot-intelligence/robots/slider/
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/legged-locomotion
  - topic/hardware-design
  - topic/leg-design
  - topic/variable-length-limbs
  - topic/actuators
  - method/mpc
  - robot/slider
  - sim/gazebo
  - eval/sim-only
---

# SLIDER

> [!info] Quick view
> **Authors:** [[Ke Wang]], [[Aksat Shah]], [[Petar Kormushev]]
> **From:** [[Imperial College London]] · [[Robot Intelligence Lab]]
> **Published:** CLAWAR 2018 · 2018 · conference
> **Keywords:** knee-less biped, vertical hip slide, timing belt rack and pinion, lightweight legs, LIP, ZMP preview control, Gazebo
> **Links:** [Project page](http://www.imperial.ac.uk/robot-intelligence/robots/slider/)
> **One line:** concept paper of SLIDER, a biped with straight carbon legs that slide vertically through the hip instead of having knees, so each leg weighs only 3 kg

## Summary

SLIDER replaces the knee by a prismatic joint at the hip: the straight carbon-fiber leg has timing belts glued on both sides, acting as a double rack, and a 3D-printed carriage with three pinions in the hip gearbox drives it up and down. Each leg has hip pitch, hip roll (cycloid drive), hip slide and ankle pitch (rod-driven like ASIMO), 8 DoF total, 96 cm tall, 18 kg, with legs of only 3 kg. All heavy parts sit in the pelvis, so leg inertia is very low.

They compare the design against anthropomorphic legs: fewer DoF on the leg, vertical compliance possible even with a straight leg (a spring on the slide), no knee singularity when stretched, smaller ankle range, more compact for stairs, but a more complex hip, less human-like look, and harder to stand up after a fall. Walking is only in simulation (MATLAB and Gazebo) with a two-stage LIP plus multi-body ZMP preview pattern generator at 0.18 m/s, and the prototype is under construction. Honestly it is a short concept paper, the IROS 2020 paper is the full version with hardware.

## How it works

- Hip gearbox in the pelvis contains the hip pitch (1:24) and hip slide (belt and spur gears) transmissions, hip roll on the side with a 1:36 cycloid
- Leg slide: timing belts on both sides of the carbon leg, three-pinion carriage to avoid tooth skipping
- Ankle pitch driven by a rod from above (1-stage cycloid), 20 cm 3D printed feet
- Gait: stage 1 LIP model plans foot positions and step timing from average CoM velocity, stage 2 multi-body model with ZMP preview control, IK and joint PD

## Results that matter

- Leg weight 3 kg on an 18 kg robot
- Stable walking at 0.18 m/s in Gazebo, ZMP close to reference in MATLAB
- No hardware results in this paper

## Interesting stuff

- Vertical compliance with straight legs is a real advantage over knees, where a straight knee sends the impact to the hip
- Removing the stretch singularity makes straight-leg walking natural for this robot
- The hip becomes the complex part, and the robot cannot kneel or stand up easily
- Only simulation, standard ZMP control

## Useful for me

- **As prior work.** One of the clearest telescopic-leg bipeds, good for the design space of prismatic legs on bipeds.
- **Watch out.** A prismatic leg changes the dynamics a lot compared with a knee leg (no stretch singularity, different impact path), so policies and reward terms tuned on knee robots may not transfer.
- **Apply.** A spring in series with a prismatic leg gives vertical compliance at any leg length, a good idea for a length-changing leg.

## Key prior work

- [[Online Regeneration of Bipedal Walking Gait Optimizing Footstep Placement and Timing|Two-stage gait regeneration]] · the pattern generator used
- [[Biped walking pattern generation by using preview control of zero-moment point|ZMP preview control]] · preview control of the ZMP
- [[Legged robots on rough terrain - experiments in adjusting step length|Hodgins straight-leg biped]] · early straight-leg planar biped
- [[Dynamic gait transition between walking, running and hopping for push recovery|Honda gait transition]] · ASIMO-like rod ankle reference

## References

1. [[The SLIDER robot walking in a Gazebo simulation|SLIDER video]] · Imperial Robot Intelligence Lab · 2018 · web page · [Link](http://www.imperial.ac.uk/robot-intelligence/robots/slider/)
2. [[Boston Dynamics - Atlas, The next generation|Atlas video]] · Boston Dynamics · n.d. · YouTube · [Link](https://www.youtube.com/watch?v=rVlhMGQgDkY)
3. [[Cassie, next generation robot|Cassie video]] · Agility Robotics · n.d. · YouTube · [Link](https://www.youtube.com/watch?v=Is4JZqhAy-M)
4. [[Google Owned Schaft Unveils New Bipedal Robot|Schaft video]] · Schaft · 2016 · YouTube · [Link](https://www.youtube.com/watch?v=iyZE0psQsX0)
5. [[Overview of the torque-controlled humanoid robot TORO|TORO]] · Englsberger et al. · 2014 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Overview+of+the+torque-controlled+humanoid+robot+TORO)
6. [[Dynamic gait transition between walking, running and hopping for push recovery|Honda gait transition]] · Kamioka et al. · 2017 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Dynamic+gait+transition+between+walking+running+and+hopping+for+push+recovery)
7. [[Legged robots on rough terrain - experiments in adjusting step length|Hodgins straight-leg biped]] · Hodgins · 1988 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Legged+robots+on+rough+terrain+experiments+in+adjusting+step+length)
8. [[Biped walking pattern generation by using preview control of zero-moment point|ZMP preview control]] · Kajita et al. · 2003 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Biped+walking+pattern+generation+by+using+preview+control+of+zero-moment+point)
9. [[Online Regeneration of Bipedal Walking Gait Optimizing Footstep Placement and Timing|Two-stage gait regeneration]] · Kryczka et al. · 2015 · IROS · [Scholar](https://scholar.google.com/scholar?q=Online+Regeneration+of+Bipedal+Walking+Gait+Optimizing+Footstep+Placement+and+Timing)
10. [[The 3D linear inverted pendulum mode - A simple modeling for a biped walking pattern generation|3D-LIPM]] · Kajita et al. · 2001 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+3D+linear+inverted+pendulum+mode+A+simple+modeling+for+a+biped+walking+pattern+generation)
11. [[Learning to exploit passive compliance for energy-efficient gait generation on a compliant humanoid|Exploiting passive compliance]] · Kormushev et al. · 2018 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Learning+to+exploit+passive+compliance+for+energy-efficient+gait+generation+on+a+compliant+humanoid)
12. [[The anatomy of a fall - Automated real-time analysis of raw force sensor data from bipedal walking robots and humans|Anatomy of a fall]] · Kormushev et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+anatomy+of+a+fall+Automated+real-time+analysis+of+raw+force+sensor+data+from+bipedal+walking+robots+and+humans)

## See also

- [[Design and Control of SLIDER - An Ultra-lightweight, Knee-less, Low-cost Bipedal Walking Robot|SLIDER IROS 2020]] · the full paper with the built robot
- [[High Dynamic Bipedal Robot with Underactuated Telescopic Straight Legs|L04 Telescopic Biped]] · another kneeless telescopic biped
