---
title: "Design and Modeling of an Extensible Soft Robotic Arm"
aliases:
  - ExtenSA
authors:
  - "[[Xiaojiao Chen]]"
  - "[[Yaoxin Guo]]"
  - "[[Dehao Duanmu]]"
  - "[[Jianshu Zhou]]"
  - "[[Wei Zhang]]"
  - "[[Zheng Wang]]"
corresponding: "[[Zheng Wang]]"
institutions:
  - "[[The University of Hong Kong]]"
  - "[[Southern University of Science and Technology]]"
labs: []
countries:
  - China
year: 2019
date: 2019-07-22
venue: IEEE Robotics and Automation Letters 2019 (Vol. 4, No. 4)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1109/LRA.2019.2929994
pdf: https://doi.org/10.1109/LRA.2019.2929994
code: "No official repo (hardware paper, checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/manipulation
  - topic/soft-robotics
  - topic/variable-length-limbs
  - topic/hardware-design
  - topic/actuators
  - topic/morphology
  - method/pneumatic-actuation
  - eval/real-robot
---

# ExtenSA

> [!info] Quick view
> **Authors:** [[Xiaojiao Chen]], [[Yaoxin Guo]], [[Dehao Duanmu]], [[Jianshu Zhou]], [[Wei Zhang]], [[Zheng Wang]] (corresponding)
> **From:** [[The University of Hong Kong]] · [[Southern University of Science and Technology]]
> **Published:** IEEE RA-L · Oct 2019 · journal
> **Keywords:** soft robotic arm, bellows, 400% elongation, backbone-less, force balance model, bending, rotation, elongation
> **Links:** [DOI](https://doi.org/10.1109/LRA.2019.2929994)
> **One line:** six long pneumatic bellows between two plates make an 800 g soft arm that goes from 100 to 500 mm long and carries more than 20 kg vertically

## Summary

Most soft arms bend and rotate but barely elongate, because backbones block length change and pneumatic artificial muscles only extend up to about 80%. ExtenSA uses six long commercial-style bellows placed in a circle between two acrylic plates, with two thin carbon fiber plates in the middle to stop buckling. No backbone and no tendons. By pressurizing or vacuuming each bellow the arm elongates up to 400% (100 to 500 mm), bends up to 100°, and rotates its bending plane.

The modeling is from force balance on the end plate instead of the usual constant curvature geometry. With linear spring bellows and symmetric placement, length, bending angle α and rotation β come out in closed form from the pressures, and with an extra term for an end load. They validate open loop prediction with a rope encoder and IMU: length error up to 38 mm (std 15 mm), bending up to 17° (std 10°), rotation up to 30°. Honestly the errors are large for anything precise, they come from hysteresis and friction, but the model is simple and does not need posture sensors.

## How it works

- 6 bellows at 60° spacing on radius R, glued to acrylic end plates, two carbon fiber plates as mid constraints against buckling
- Each bellow force F_i = P_i A − k(l_i − l_0), linear spring assumption away from the extremes
- Geometry: l_i = l_m − αR cos(θ_i − β), and the symmetric layout cancels cross terms
- Closed form: α = sqrt(Φc² + Φs²)/C1, β = atan2(Φs, Φc) − π, l_m from the sum of pressures, with Φc, Φs, Φp the weighted pressure sums
- External load term adds the end mass torque and pulling force with small angle approximations
- Hardware: pressure sensors per bellow (−100 to 300 kPa), rope encoder on the centerline, IMU on the end plate at 50 Hz

## Results that matter

- Length 100 to 500 mm, 400% elongation or 80% contraction, 800 g, payload above 20 kg vertically
- Length model error max 38 mm, std 15 mm, mostly from hysteresis during contraction
- Bending model error max 17°, std 10°, rotation model error max 30°
- With 0.51 and 0.9 kg loads the model predicts the drop of α and the extension of l_m

## Interesting stuff

- Force balance instead of geometry gives a model that directly says how to design: bending grows with bellow area and drops with installation radius
- No closed loop control at all, the paper is only design and open loop modeling
- Large hysteresis is ignored in the model, which explains most of the error
- The arm is huge in elongation but soft laterally, so precision manipulation is not the use case

## Useful for me

- **As prior work.** Example of a limb whose length is the main actuated variable, with a 5x range. Good for the related work on extensible limbs.
- **Watch out.** Hysteresis and friction dominate the error in extensible pneumatic limbs, so any learned controller for a length-changing limb needs history in the observation or a learned actuator model.
- **Gap.** Not usable for legged locomotion as is, too compliant laterally. The ideas transfer to manipulation arms of a morphing humanoid more than to legs.

## Key prior work

- [[Design and kinematic modeling of constant curvature continuum robots - A review|Constant curvature review]] · the standard geometric modeling they move away from
- [[Novel design of a soft lightweight pneumatic continuum robot arm with decoupled variable stiffness and positioning|Decoupled stiffness arm]] · PAM based soft arm with limited elongation
- [[Application-driven design of soft, 3-D printed, pneumatic actuators with bellows|3D printed bellows]] · bellows arm with mid plates, closest design
- [[An inductance-based sensing system for bellows-driven continuum joints in soft robots|Bellows sensing]] · sensing for bellows joints
- [[Design and experimental testing of the OctArm soft robot manipulator|OctArm]] · classic PAM continuum arm
- [[The bionic handling assistant - A success story of additive manufacturing|Bionic Handling Assistant]] · Festo bellows arm

## References

1. [[Fiber-reinforced origamic robotic actuator|Origamic actuator]] · Yi et al. · 2017 · Soft Robotics · [Scholar](https://scholar.google.com/scholar?q=Fiber-reinforced+origamic+robotic+actuator)
2. [[A three-dimensional-printed soft robotic glove with enhanced ergonomics and force capability|3D printed soft glove]] · Yi et al. · 2018 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=A+three-dimensional-printed+soft+robotic+glove+with+enhanced+ergonomics+and+force+capability)
3. [[A soft-robotic gripper with enhanced object adaptation and grasping reliability|Soft gripper]] · Zhou et al. · 2017 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=A+soft-robotic+gripper+with+enhanced+object+adaptation+and+grasping+reliability)
4. [[BCL-13 - A 13-DOF soft robotic hand for dexterous grasping and in-hand manipulation|BCL-13]] · Zhou et al. · 2018 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=BCL-13+A+13-DOF+soft+robotic+hand+for+dexterous+grasping+and+in-hand+manipulation)
5. [[A soft robotic approach to robust and dexterous grasping|Soft robust grasping]] · Zhou et al. · 2018 · RoboSoft · [Scholar](https://scholar.google.com/scholar?q=A+soft+robotic+approach+to+robust+and+dexterous+grasping)
6. [[Soft-actuator-based robotic joint for safe and forceful interaction with controllable impact response|Soft-actuator joint]] · Chen et al. · 2018 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Soft-actuator-based+robotic+joint+for+safe+and+forceful+interaction+with+controllable+impact+response)
7. [[Customizable three-dimensional-printed origami soft robotic joint with effective behavior shaping for safe interactions|Origami soft joint]] · Yi et al. · 2019 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Customizable+three-dimensional-printed+origami+soft+robotic+joint+with+effective+behavior+shaping+for+safe+interactions)
8. [[An HMM approach to realistic haptic human-robot interaction|HMM haptic HRI]] · Wang et al. · 2009 · World Haptics · [Scholar](https://scholar.google.com/scholar?q=An+HMM+approach+to+realistic+haptic+human-robot+interaction)
9. [[Fast online impedance estimation for robot control|Online impedance estimation]] · Wang et al. · 2009 · ICM · [Scholar](https://scholar.google.com/scholar?q=Fast+online+impedance+estimation+for+robot+control)
10. [[Haptic feedback and control of a flexible surgical endoscopic robot|Endoscopic robot haptics]] · Wang et al. · 2013 · Comput. Methods Programs Biomed. · [Scholar](https://scholar.google.com/scholar?q=Haptic+feedback+and+control+of+a+flexible+surgical+endoscopic+robot)
11. [[Interaction forces of soft fiber reinforced bending actuators|Soft bending actuator forces]] · Wang et al. · 2017 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Interaction+forces+of+soft+fiber+reinforced+bending+actuators)
12. [[Design and kinematic modeling of constant curvature continuum robots - A review|Constant curvature review]] · Webster & Jones · 2010 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Design+and+kinematic+modeling+of+constant+curvature+continuum+robots+A+review)
13. [[A robotic manipulator design with novel soft actuators|Soft actuator manipulator]] · Chen et al. · 2017 · ICRA · [Scholar](https://scholar.google.com/scholar?q=A+robotic+manipulator+design+with+novel+soft+actuators)
14. [[Continuous backbone continuum robot manipulators|Continuous backbone manipulators]] · Walker · 2013 · ISRN Robotics · [Scholar](https://scholar.google.com/scholar?q=Continuous+backbone+continuum+robot+manipulators)
15. [[The Robot - The Life Story of a Technology|The Robot book]] · Nocks · 2009 · Greenwood Press · [Scholar](https://scholar.google.com/scholar?q=Nocks+The+Robot+The+Life+Story+of+a+Technology)
16. [[Tensor Arm Manipulator Design|Tensor Arm]] · Anderson · 1967 · ASME · [Scholar](https://scholar.google.com/scholar?q=Tensor+Arm+Manipulator+Design+Anderson)
17. [[Active hose - An artificial elephant's nose with maneuverability for rescue operation|Active Hose]] · Tsukagoshi et al. · 2001 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Active+hose+An+artificial+elephant%27s+nose+with+maneuverability+for+rescue+operation)
18. [[The OmniTread OT-4 serpentine robot - Design and performance|OmniTread OT-4]] · Borenstein et al. · 2007 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=The+OmniTread+OT-4+serpentine+robot+Design+and+performance)
19. [[A novel elephant's trunk robot|Elephant's trunk robot]] · Walker & Hannan · 1999 · AIM · [Scholar](https://scholar.google.com/scholar?q=A+novel+elephant%27s+trunk+robot)
20. [[Design concept and validation of a robotic arm inspired by the octopus|Octopus arm concept]] · Cianchetti et al. · 2011 · Materials Science and Engineering C · [Scholar](https://scholar.google.com/scholar?q=Design+concept+and+validation+of+a+robotic+arm+inspired+by+the+octopus)
21. [[Design and development of a soft robotic octopus arm exploiting embodied intelligence|Octopus arm embodied]] · Cianchetti et al. · 2012 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+and+development+of+a+soft+robotic+octopus+arm+exploiting+embodied+intelligence)
22. [[Development of flexible microactuator and its applications to robotic mechanisms|Flexible microactuator]] · Suzumori et al. · 1991 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Development+of+flexible+microactuator+and+its+applications+to+robotic+mechanisms)
23. [[Design and implementation of a multi-section continuum robot - Air-Octor|Air-Octor]] · McMahan et al. · 2005 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+implementation+of+a+multi-section+continuum+robot+Air-octor)
24. [[Sensor-based guidance control of a continuum robot for a semi-autonomous colonoscopy|COLOBOT]] · Chen et al. · 2009 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Sensor-based+guidance+control+of+a+continuum+robot+for+a+semi-autonomous+colonoscopy)
25. [[Stiff-flop surgical manipulator - Mechanical design and experimental characterization of the single module|STIFF-FLOP]] · Cianchetti et al. · 2013 · IROS · [Scholar](https://scholar.google.com/scholar?q=STIFF-FLOP+surgical+manipulator+Mechanical+design+and+experimental+characterization+of+the+single+module)
26. [[Design and experimental testing of the OctArm soft robot manipulator|OctArm]] · Grissom et al. · 2006 · Proc. SPIE · [Scholar](https://scholar.google.com/scholar?q=Design+and+experimental+testing+of+the+OctArm+soft+robot+manipulator)
27. [[A novel continuum trunk robot based on contractor muscles|Contractor muscle trunk]] · Bartow et al. · 2013 · WSEAS · [Scholar](https://scholar.google.com/scholar?q=A+novel+continuum+trunk+robot+based+on+contractor+muscles)
28. [[Novel design of a soft lightweight pneumatic continuum robot arm with decoupled variable stiffness and positioning|Decoupled stiffness arm]] · Giannaccini et al. · 2018 · Soft Robotics · [Scholar](https://scholar.google.com/scholar?q=Novel+design+of+a+soft+lightweight+pneumatic+continuum+robot+arm+with+decoupled+variable+stiffness+and+positioning)
29. [[Design of an artificial muscle continuum robot|Artificial muscle continuum robot]] · Pritts & Rahn · 2004 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+of+an+artificial+muscle+continuum+robot)
30. [[The KSI tentacle manipulator|KSI tentacle]] · Immega & Antonelli · 2002 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+KSI+tentacle+manipulator)
31. [[Study on slime robot - Development of the mobile robot prototype model using bridle bellows|Slime robot]] · Aoki et al. · 2004 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Study+on+slime+robot+Development+of+the+mobile+robot+prototype+model+using+bridle+bellows)
32. [[The bionic handling assistant - A success story of additive manufacturing|Bionic Handling Assistant]] · Grzesiak et al. · 2011 · Assembly Automation · [Scholar](https://scholar.google.com/scholar?q=The+bionic+handling+assistant+A+success+story+of+additive+manufacturing)
33. [[BionicMotionRobot - Pneumatic Lightweight Robot with Natural Movement Patterns|BionicMotionRobot]] · Festo AG · n.d. · product information · [Link](https://www.festo.com/net/SupportPortal/Files/630184/Festo_BionicMotionRobot_en.pdf)
34. [[Application-driven design of soft, 3-D printed, pneumatic actuators with bellows|3D printed bellows]] · Drotman et al. · 2019 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Application-driven+design+of+soft+3-D+printed+pneumatic+actuators+with+bellows)
35. [[Printable hydraulics - A method for fabricating robots by 3D co-printing solids and liquids|Printable hydraulics]] · MacCurdy et al. · 2016 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Printable+hydraulics+A+method+for+fabricating+robots+by+3D+co-printing+solids+and+liquids)
36. [[An inductance-based sensing system for bellows-driven continuum joints in soft robots|Bellows sensing]] · Felt et al. · 2019 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=An+inductance-based+sensing+system+for+bellows-driven+continuum+joints+in+soft+robots)
37. [[Intuitive control of humanoid soft-robotic hand BCL-13|BCL-13 control]] · Zhou et al. · 2018 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Intuitive+control+of+humanoid+soft-robotic+hand+BCL-13)
38. [[RGB-T SLAM - A flexible SLAM framework by combining appearance and thermal information|RGB-T SLAM]] · Chen et al. · 2017 · ICRA · [Scholar](https://scholar.google.com/scholar?q=RGB-T+SLAM+A+flexible+SLAM+framework+by+combining+appearance+and+thermal+information)

## See also

- [[A Compliant Telescopic Limb with Anisotropic Stiffness|Hexo-Flexo Telescopic Limb]] · same bellows idea but with a rigid telescope inside for legs
- [[Bioinspired growable humanoid robot with bone-mimetic linkages for versatile mobility|GrowHR]] · inflatable extensible limbs used as humanoid legs
