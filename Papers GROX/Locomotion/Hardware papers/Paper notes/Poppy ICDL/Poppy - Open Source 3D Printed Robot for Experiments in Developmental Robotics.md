---
title: "Poppy: Open Source 3D Printed Robot for Experiments in Developmental Robotics"
aliases:
  - Poppy ICDL
  - Poppy
authors:
  - "[[Matthieu Lapeyre]]"
  - "[[Pierre Rouanet]]"
  - "[[Jonathan Grizou]]"
  - "[[Steve N'Guyen]]"
  - "[[Alexandre Le Falher]]"
  - "[[Fabien Depraetre]]"
  - "[[Pierre-Yves Oudeyer]]"
corresponding: "[[Matthieu Lapeyre]]"
institutions:
  - "[[Inria]]"
  - "[[ENSTA ParisTech]]"
labs:
  - "[[Flowers Laboratory]]"
countries:
  - France
year: 2014
date: ""
venue: IEEE ICDL-EpiRob 2014 (extended abstract)
venue_type: workshop
arxiv_category: ""
arxiv_id: ""
url: https://scholar.google.com/scholar?q=Poppy+Open+Source+3D+Printed+Robot+for+Experiments+in+Developmental+Robotics
pdf: ""
code: https://github.com/poppy-project
project_page: http://www.poppy-project.org
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/hardware-design
  - topic/morphology
  - topic/reconfigurable-robots
  - topic/developmental-robotics
  - robot/poppy
  - eval/real-robot
---

# Poppy ICDL

> [!info] Quick view
> **Authors:** [[Matthieu Lapeyre]] (corresponding), [[Pierre Rouanet]], [[Jonathan Grizou]], [[Steve N'Guyen]], [[Alexandre Le Falher]], [[Fabien Depraetre]], [[Pierre-Yves Oudeyer]]
> **From:** [[Inria]] · [[Flowers Laboratory]] · [[ENSTA ParisTech]]
> **Published:** ICDL-EpiRob 2014 · 2-page extended abstract
> **Keywords:** open-source humanoid, 3D printing, morphology as experimental variable, embodied AI, Dynamixel, pypot, explauto, curiosity-driven learning
> **Links:** [Scholar](https://scholar.google.com/scholar?q=Poppy+Open+Source+3D+Printed+Robot+for+Experiments+in+Developmental+Robotics) · [Code and hardware](https://github.com/poppy-project) · [Project](http://www.poppy-project.org)
> **One line:** a 3.5 kg, 84 cm, 25-motor, fully 3D-printed open-source humanoid designed so that morphology can be changed quickly and treated as an experimental variable

## Summary

Short abstract presenting the Poppy project from the Flowers lab. The motivation comes from embodied AI (Brooks, Pfeifer, McGeer's passive walker): body shape matters for control, cognition and development, so it should be an experimental variable studied on real robots. The problem is that real robots break and are hard to modify. Poppy answers with 3D-printed structure and off-the-shelf Robotis Dynamixel motors and Arduino boards, so new body parts can be printed and swapped in minutes, and with fully open hardware and software.

The robot is 84 cm and 3.5 kg, with 25 motors, five joints in the trunk, human-like limb proportions and a 6° bent thigh that helped biped stability in their earlier work. It assembles in about two days, costs about 7,500 to 8,000 (currency not given), is designed to survive falls, and is controlled with their Python library pypot at about 50 Hz, linked to explauto for curiosity-driven sensorimotor learning. Honestly it is a project announcement, not a research result, but the framing "morphology as an experimental variable" is exactly my topic.

## How it works

- Structure fully 3D printed, modular, reconfigurable in minutes
- 25 Dynamixel servos, Arduino for extra sensors, human-like proportions and articulated trunk
- Bent thigh (6°) inspired by human femur to improve stability
- pypot: Python control library for Robotis actuators, sensorimotor loop at 50 Hz
- explauto: curiosity-driven exploration library for learning inverse models (motor and goal babbling)

## Results that matter

- 3.5 kg, 84 cm, 25 DoF, about 2 days of assembly
- No quantitative experiments in this abstract

## Interesting stuff

- Clear statement that morphology should be varied on real hardware, not only in simulation
- Servo-based, so low backdrivability and not suited for dynamic locomotion, as later papers note
- Good historical root for open, printable humanoids like Berkeley Humanoid Lite

## Useful for me

- **As prior work.** Good citation for the idea of morphology as an experimental variable on a real humanoid and for rapid body prototyping.
- **Apply.** Modular printed links make it cheap to test many body proportions on hardware, which is useful to validate a morphing-body policy on several real configurations.
- **Gap.** Bodies change only by manual reassembly, not autonomously.

## Key prior work

- [[Intelligence without representation|Intelligence without representation]] · Brooks, the embodied AI starting point
- [[Understanding Intelligence|Understanding Intelligence]] · Pfeifer and Scheier on embodiment
- [[Passive Dynamic Walking|Passive Dynamic Walking]] · morphology doing the work of control
- [[Intrinsic Motivation Systems for Autonomous Mental Development|Intrinsic motivation systems]] · curiosity-driven learning used with Poppy
- [[Poppy Humanoid Platform - Experimental Evaluation of the Role of a Bio-inspired Thigh Shape|Poppy thigh shape]] · the bent thigh study

## References

1. [[Intelligence without representation|Intelligence without representation]] · Brooks · 1991 · Artificial Intelligence · [Scholar](https://scholar.google.com/scholar?q=Brooks+Intelligence+without+representation)
2. [[The Artificial Life Route to Artificial Intelligence - Building Embodied, Situated Agents|Artificial life route to AI]] · Steels & Brooks · 1995 · Psychology Press · [Scholar](https://scholar.google.com/scholar?q=The+artificial+life+route+to+artificial+intelligence+Building+embodied+situated+agents)
3. [[Understanding Intelligence|Understanding Intelligence]] · Pfeifer & Scheier · 2001 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Pfeifer+Scheier+Understanding+intelligence)
4. [[Passive Dynamic Walking|Passive Dynamic Walking]] · McGeer · 1990 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Passive+Dynamic+Walking+McGeer)
5. [[Intrinsic Motivation Systems for Autonomous Mental Development|Intrinsic motivation systems]] · Oudeyer et al. · 2007 · IEEE TEVC · [Scholar](https://scholar.google.com/scholar?q=Intrinsic+Motivation+Systems+for+Autonomous+Mental+Development)
6. [[Exploration strategies in developmental robotics - a unified probabilistic framework|Exploration strategies]] · Moulin-Frier & Oudeyer · 2013 · ICDL · [Scholar](https://scholar.google.com/scholar?q=Exploration+strategies+in+developmental+robotics+a+unified+probabilistic+framework)
7. [[The Humanoid Robot Acroban - Leveraging Semi-Passive Dynamics in the Vertebral Column|Acroban]] · Ly et al. · 2010 · journal (as printed, no venue) · [Scholar](https://scholar.google.com/scholar?q=The+Humanoid+Robot+Acroban+Leveraging+Semi-Passive+Dynamics+in+the+Vertebral+Column)
8. [[Poppy Humanoid Platform - Experimental Evaluation of the Role of a Bio-inspired Thigh Shape|Poppy thigh shape]] · Lapeyre et al. · 2013 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Poppy+Humanoid+Platform+Experimental+Evaluation+of+the+Role+of+a+Bio-inspired+Thigh+Shape)

## See also

- [[Rapid morphological exploration with the Poppy humanoid platform|Poppy morphological exploration]] · the full Humanoids 2014 paper
- [[Demonstrating Berkeley Humanoid Lite - An Open-source, Accessible, and Customizable 3D-printed Humanoid Robot|Berkeley Humanoid Lite]] · modern open printed humanoid with backdrivable actuators
- [[Developmental robotics - a survey|Developmental robotics survey]] · the field Poppy was built for
