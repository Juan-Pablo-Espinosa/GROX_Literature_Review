---
title: "High Dynamic Bipedal Robot with Underactuated Telescopic Straight Legs"
aliases:
  - L04 Telescopic Biped
  - L04 Robot
authors:
  - "[[Haiming Mou]]"
  - "[[Jun Tang]]"
  - "[[Jian Liu]]"
  - "[[Wenqiong Xu]]"
  - "[[Yunfeng Hou]]"
  - "[[Jianwei Zhang]]"
corresponding: "[[Jian Liu]]"
institutions:
  - "[[University of Shanghai for Science and Technology]]"
  - "[[University of Hamburg]]"
labs:
  - "[[USST Institute of Machine Intelligence]]"
countries:
  - China
  - Germany
year: 2024
date: 2024-02-17
venue: Mathematics 2024 (Vol. 12, 600)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.3390/math12040600
pdf: https://www.mdpi.com/2227-7390/12/4/600/pdf
code: "No official repo (checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/legged-locomotion
  - topic/hardware-design
  - topic/leg-design
  - topic/variable-length-limbs
  - topic/actuators
  - method/mpc
  - eval/real-robot
---

# L04 Telescopic Biped

> [!info] Quick view
> **Authors:** [[Haiming Mou]], [[Jun Tang]], [[Jian Liu]] (corresponding), [[Wenqiong Xu]], [[Yunfeng Hou]], [[Jianwei Zhang]]
> **From:** [[University of Shanghai for Science and Technology]] · [[USST Institute of Machine Intelligence]] · [[University of Hamburg]]
> **Published:** Mathematics (MDPI) · Feb 2024 · journal
> **Keywords:** kneeless biped, double-slider telescopic leg, virtual ankle, coupled split hip, underactuated, ALIP angular momentum control
> **Links:** [DOI](https://doi.org/10.3390/math12040600) · [PDF](https://www.mdpi.com/2227-7390/12/4/600/pdf)
> **One line:** a kneeless humanoid whose legs are two parallel ball-screw sliders each (front and back), giving leg length and a virtual ankle pitch with no ankle motor, plus a coupled hip that does split and yaw with two motors

## Summary

L04 is the follow-up of L03 from the same USST group. It is a full-size humanoid shape with arms and head (Dynamixel servos), but the legs are straight telescopic bars. The hip packs everything in a 180 mm cylinder: two Maxon RE40 motors drive two vertical ball screws through a linkage that splits both legs laterally (±15°) and rotates them (±10°) together, and two more motors swing each thigh. Each leg is a "double slider": a front and a rear bar driven by separate ball screws. Moving both changes leg length (200 mm stroke), moving them in opposite directions tilts the foot, so the front and rear bars act like toe and heel and give a virtual ankle pitch without an ankle motor. Each leg holds about 56 kg and moves at about 1 m/s.

Control is ALIP style: the angular momentum about the contact point is predicted to the end of the next step and the foothold is chosen to reach a desired value, with a lateral version that alternates support side. Simulation forward walking at 0.5 m/s and lateral at 0.3 m/s, and real forward, lateral and push walking are reported only as standard deviations of CoM velocity and angular momentum. Honestly the evaluation is thin (no speed, no push magnitude, no comparison), and the "high dynamic" in the title is not really shown, but the double-slider leg is a smart mechanism.

## How it works

- 17 joints: 2 head, 3 per arm (Dynamixel MX64), 5 hip rotations, 2 linear joints per leg
- Hip: crossbar and linkage mechanism, a 15 mm ball screw for lateral split (±15°), a 6 mm screw for coupled rotation (±10°), both legs coupled through one mechanism
- Leg: two 8 mm, 12 mm lead ball screws per leg, front and rear sliders, length change when they move together, virtual ankle pitch when they move opposite
- Four ground contact points in double support, so it can stand unpowered
- Geometric IK for leg angles and lengths from foot positions, terrain height estimated from joints and gyro instead of force sensors
- ALIP: state (x, L) with L the angular momentum about the contact point, desired foothold from predicted L at end of step

## Results that matter

- Leg stroke 200 mm, about 56 kg load and about 1 m/s per leg
- Simulation forward 0.5 m/s with velocity std [0.18, 0.09, 0.05] and lateral 0.3 m/s
- Real forward walking velocity std [0.025, 0.098, 0.113], lateral [0.126, 0.092, 0.084], walking while hit [0.070, 0.102, 0.115]
- No numbers on max speed, push force, energy or terrain

## Interesting stuff

- The double-slider leg is a clever way to get an ankle-like DoF and a support polygon without any distal motor
- Coupling lateral split and yaw in one mechanism cuts motors but also couples DoFs that a controller may want independent
- Only standard deviations are reported, which say little about walking quality
- They cite PedestriANS, a biped with adaptive morphology, which is worth checking
- DecARt cites this robot as LR04, the paper itself calls it L04

## Useful for me

- **As prior work.** Telescopic straight legs with 200 mm of stroke on a human-size body, a mid point between gait-level length change and real morphology change.
- **Apply.** Two parallel prismatic actuators per leg give length plus foot pitch. For a growing limb this kind of redundancy could give both a size change and an ankle-like DoF.
- **Watch out.** Ball screws are not backdrivable in general, so impacts go straight into the transmission. For learned dynamic locomotion this hurts sim-to-real unless the screws are modeled.

## Key prior work

- [[Design, Control, and Validation of a Symmetrical Hip and Straight-Legged Vertically-Compliant Bipedal Robot|L03]] · the previous robot from the same group
- [[Design and Control of SLIDER - An Ultra-lightweight, Knee-less, Low-cost Bipedal Walking Robot|SLIDER]] · kneeless sliding biped reference
- [[Angular momentum about the contact point for control of bipedal locomotion - Validation in a LIP-based controller|ALIP]] · the control variable they use
- [[Real-time 3D walking pattern generation for a biped robot with telescopic legs|Meltran V]] · classic telescopic-leg biped
- [[PedestriANS - A bipedal robot with adaptive morphology|PedestriANS]] · biped with adaptive morphology, closest to morphology change
- [[Efficiency analysis of telescopic-legged bipedal robots|Telescopic leg efficiency]] · energy argument for telescopic legs

## References

1. [[Biologically inspired jumping robots - A comprehensive review|Jumping robots review]] · Zhang et al. · 2020 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Biologically+inspired+jumping+robots+A+comprehensive+review)
2. [[An overview of humanoid robots technologies|Humanoid technologies overview]] · Stasse & Flayols · 2019 · Springer chapter · [Scholar](https://scholar.google.com/scholar?q=An+overview+of+humanoid+robots+technologies+Stasse+Flayols)
3. [[Humanoid robots - past, present and the future|Humanoids past present future]] · Denny et al. · 2016 · Eur. J. Adv. Eng. Technol. · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robots+past+present+and+the+future+Denny)
4. [[Bipedal humanoid hardware design - A technology review|Humanoid hardware review]] · Ficht & Behnke · 2021 · Current Robotics Reports · [Scholar](https://scholar.google.com/scholar?q=Bipedal+humanoid+hardware+design+A+technology+review)
5. [[Real time motion generation and control for biped robot - 1st report - Walking gait pattern generation|Honda real-time gait generation]] · Takenaka et al. · 2009 · IROS · [Scholar](https://scholar.google.com/scholar?q=Real+time+motion+generation+and+control+for+biped+robot+1st+report+Walking+gait+pattern+generation)
6. [[Development of humanoid robot ASIMO|ASIMO development]] · Hirose · 2001 · IROS Workshop · [Scholar](https://scholar.google.com/scholar?q=Development+of+humanoid+robot+ASIMO+Hirose)
7. [[Atlas Humanoid Robot Control with Flexible Finite State Machines for Playing Soccer|Atlas soccer FSM]] · Catherman et al. · 2020 · SoutheastCon · [Scholar](https://scholar.google.com/scholar?q=Atlas+Humanoid+Robot+Control+with+Flexible+Finite+State+Machines+for+Playing+Soccer)
8. [[Humanoid Robotics - A Reference|Humanoid Robotics reference]] · Goswami & Vadakkepat · 2019 · Springer · [Scholar](https://scholar.google.com/scholar?q=Humanoid+Robotics+A+Reference)
9. [[Inverted pendulum model for turn-planning for biped robot|Turn-planning IP]] · Chang et al. · 2020 · Physical Communication · [Scholar](https://scholar.google.com/scholar?q=Inverted+pendulum+model+for+turn-planning+for+biped+robot)
10. [[Biped walking stabilization based on linear inverted pendulum tracking|LIP tracking stabilization]] · Kajita et al. · 2010 · IROS · [Scholar](https://scholar.google.com/scholar?q=Biped+walking+stabilization+based+on+linear+inverted+pendulum+tracking)
11. [[Angular momentum about the contact point for control of bipedal locomotion - Validation in a LIP-based controller|ALIP]] · Gong & Grizzle · 2020 · arXiv · [arXiv](https://arxiv.org/abs/2008.10763)
12. [[The Poppy humanoid robot - Leg design for biped locomotion|Poppy leg design]] · Lapeyre et al. · 2013 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+poppy+humanoid+robot+Leg+design+for+biped+locomotion)
13. [[A Multi-Agent Reinforcement Learning Method for Omnidirectional Walking of Bipedal Robots|MARL biped walking]] · Mou et al. · 2023 · Biomimetics · [Scholar](https://scholar.google.com/scholar?q=A+Multi-Agent+Reinforcement+Learning+Method+for+Omnidirectional+Walking+of+Bipedal+Robots)
14. [[Walking Control of Telescopic Leg Bipedal Robot Based on Angular Momentum Predictive Foothold|Telescopic biped ALIP control]] · Gan et al. · 2023 · ROBIO · [Scholar](https://scholar.google.com/scholar?q=Walking+Control+of+Telescopic+Leg+Bipedal+Robot+Based+on+Angular+Momentum+Predictive+Foothold)
15. [[Underactuated motion planning and control for jumping with wheeled-bipedal robots|Wheeled-biped jumping]] · Chen et al. · 2020 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Underactuated+motion+planning+and+control+for+jumping+with+wheeled-bipedal+robots)
16. [[PedestriANS - A bipedal robot with adaptive morphology|PedestriANS]] · Ahmad et al. · 2021 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=PedestriANS+A+bipedal+robot+with+adaptive+morphology)
17. [[Gait cycles of passive walking biped robot model with flexible legs|Flexible-leg passive walker]] · Safartoobi et al. · 2021 · Mechanism and Machine Theory · [Scholar](https://scholar.google.com/scholar?q=Gait+cycles+of+passive+walking+biped+robot+model+with+flexible+legs)
18. [[Legged Robots That Balance|Legged Robots That Balance]] · Raibert · 1986 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Raibert+Legged+Robots+That+Balance)
19. [[Real-time 3D walking pattern generation for a biped robot with telescopic legs|Meltran V]] · Kajita et al. · 2001 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Real-time+3D+walking+pattern+generation+for+a+biped+robot+with+telescopic+legs)
20. [[Google Owned Schaft Unveils New Bipedal Robot|Schaft video]] · Schaft · 2016 · YouTube · [Link](https://www.youtube.com/watch?v=iyZE0psQsX0)
21. [[SLIDER - A novel bipedal walking robot without knees|SLIDER TAROS]] · Wang et al. · 2018 · TAROS · [Scholar](https://scholar.google.com/scholar?q=Slider+A+novel+bipedal+walking+robot+without+knees)
22. [[Design and Control of SLIDER - An Ultra-lightweight, Knee-less, Low-cost Bipedal Walking Robot|SLIDER]] · Wang et al. · 2020 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+control+of+SLIDER+An+ultra-lightweight+knee-less+low-cost+bipedal+walking+robot)
23. [[Design, Control, and Validation of a Symmetrical Hip and Straight-Legged Vertically-Compliant Bipedal Robot|L03]] · Tang et al. · 2023 · Biomimetics · [DOI](https://doi.org/10.3390/biomimetics8040340)
24. [[Optimum hip trajectory generation of a biped robot during single support phase using genetic algorithm|GA hip trajectory]] · Udai · 2008 · ICETET · [Scholar](https://scholar.google.com/scholar?q=Optimum+hip+trajectory+generation+of+a+biped+robot+during+single+support+phase+using+genetic+algorithm)
25. [[Planning adaptive brachistochrone and circular arc hip trajectory for a toe-foot bipedal robot going downstairs|Brachistochrone hip trajectory]] · Bhardwaj et al. · 2021 · J. Phys. Conf. Ser. · [Scholar](https://scholar.google.com/scholar?q=Planning+adaptive+brachistochrone+and+circular+arc+hip+trajectory+for+a+toe-foot+bipedal+robot+going+downstairs)
26. [[Efficiency analysis of telescopic-legged bipedal robots|Telescopic leg efficiency]] · Harata et al. · 2018 · Artificial Life and Robotics · [Scholar](https://scholar.google.com/scholar?q=Efficiency+analysis+of+telescopic-legged+bipedal+robots)
27. [[Introduction to Humanoid Robotics|Introduction to Humanoid Robotics]] · Kajita et al. · 2014 · Springer · [Scholar](https://scholar.google.com/scholar?q=Introduction+to+Humanoid+Robotics+Kajita)
28. [[Capture Point - A Step toward Humanoid Push Recovery|Capture Point]] · Pratt et al. · 2006 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Capture+point+A+step+toward+humanoid+push+recovery)
29. [[Mechanics of humanoid robot|Mechanics of humanoid robot]] · Hashimoto · 2020 · Advanced Robotics · [DOI](https://doi.org/10.1080/01691864.2020.1813624)
30. [[Feedback Control of Dynamic Bipedal Robot Locomotion|Feedback control of bipedal locomotion]] · Westervelt et al. · 2018 · CRC Press · [Scholar](https://scholar.google.com/scholar?q=Feedback+Control+of+Dynamic+Bipedal+Robot+Locomotion)
31. [[The 3D linear inverted pendulum mode - A simple modeling for a biped walking pattern generation|3D-LIPM]] · Kajita et al. · 2001 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+3D+linear+inverted+pendulum+mode+A+simple+modeling+for+a+biped+walking+pattern+generation)
32. [[Hopping in legged systems - Modeling and simulation for the two-dimensional one-legged case|Raibert hopping]] · Raibert · 1984 · IEEE T-SMC · [Scholar](https://scholar.google.com/scholar?q=Hopping+in+legged+systems+Modeling+and+simulation+for+the+two-dimensional+one-legged+case)
33. [[Biped gait stabilization via foot placement|Foot placement stabilization]] · Townsend · 1985 · Journal of Biomechanics · [Scholar](https://scholar.google.com/scholar?q=Biped+gait+stabilization+via+foot+placement)
34. [[From 2D design of underactuated bipedal gaits to 3D implementation - Walking with speed tracking|2D to 3D underactuated gaits]] · Da et al. · 2016 · IEEE Access · [Scholar](https://scholar.google.com/scholar?q=From+2D+design+of+underactuated+bipedal+gaits+to+3D+implementation+Walking+with+speed+tracking)

## See also

- [[DecARt Leg - Design and Evaluation of a Novel Humanoid Robot Leg with Decoupled Actuation for Agile Locomotion|DecARt Leg]] · a quasi-telescopic leg that keeps a human look
- [[SLIDER - A Bipedal Robot with Knee-less Legs and Vertical Hip Sliding Motion|SLIDER CLAWAR]] · the original SLIDER concept paper
