---
title: "Design and control of the rapid legged platform GAZELLE"
aliases:
  - GAZELLE
authors:
  - "[[Hyobin Jeong]]"
  - "[[KangKyu Lee]]"
  - "[[Wooshik Kim]]"
  - "[[Inho Lee]]"
  - "[[Jun-Ho Oh]]"
corresponding: "[[Inho Lee]]"
institutions:
  - "[[Korea Advanced Institute of Science and Technology]]"
  - "[[Carnegie Mellon University]]"
  - "[[Institute for Human and Machine Cognition]]"
labs:
  - "[[KAIST Humanoid Robot Research Center]]"
countries:
  - South Korea
  - USA
year: 2020
date: 2020-01-22
venue: Mechatronics 2020 (Vol. 66, 102319)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1016/j.mechatronics.2019.102319
pdf: https://doi.org/10.1016/j.mechatronics.2019.102319
code: "No official repo (hardware paper, checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/legged-locomotion
  - topic/hardware-design
  - topic/leg-design
  - topic/parallel-mechanisms
  - topic/actuators
  - topic/state-estimation
  - method/mpc
  - sim/choreonoid
  - robot/gazelle
  - eval/real-robot
---

# GAZELLE

> [!info] Quick view
> **Authors:** [[Hyobin Jeong]], [[KangKyu Lee]], [[Wooshik Kim]], [[Inho Lee]] (corresponding), [[Jun-Ho Oh]]
> **From:** [[Korea Advanced Institute of Science and Technology]] · [[KAIST Humanoid Robot Research Center]] · [[Carnegie Mellon University]] · [[Institute for Human and Machine Cognition]]
> **Published:** Mechatronics · Jan 2020 · journal
> **Keywords:** biped platform, low leg inertia, link-driven knee, 2-RSU parallel ankle, motor cooling, direct-stacked fin, CoM damping control, capture point
> **Links:** [DOI](https://doi.org/10.1016/j.mechatronics.2019.102319)
> **One line:** a 13-DoF, 37 kg biped from the DRC-HUBO+ team that moves knee and ankle motors up the leg with linkages and uses light fin coolers to push motors 4x over nominal current

## Summary

GAZELLE is a lower body biped made by the KAIST team that won the DARPA Robotics Challenge, reusing the DRC-HUBO+ electronics. The goal is fast leg movement for robust walking, because a quick swing lets the robot change its landing position after a push. The design has three main ideas: the knee is driven through a four-bar from a motor in the thigh, the ankle is a 2-RSU parallel mechanism with both motors at the knee, and each Maxon 200 W motor gets a direct-stacked aluminum fin cooler. Hip roll and pitch axes are offset to get a wide range of motion.

Moving actuators up reduces leg inertia about the hip to 70.7% and about the knee to 54.6% with only around 5% more mass. In Choreonoid, walking at 2.16 km/h the ZMP RMS error was 0.048 m vs 0.076 m for a conventional leg, so the low inertia leg fits the LIPM controller better. The fin cooler gets R2 ≈ 0.33 K/W at 130 g and 2.5 W, letting the motor run about 20 A, close to water cooling (21 A) at a fraction of the weight. The controller is classic: preview control, a Kalman filter that fuses IMU and ZMP for CoM damping, and capture point based foot force/torque control. Walking on 0.5 to 3 cm wood blocks works, but max walking speed is only 1.8 km/h, which is a bit funny for a "rapid" platform.

## How it works

- 13 DoF (6 per leg plus waist yaw), 1.3 m, 37 kg with battery, Maxon EC-4pole 200 W motors with CSF harmonic drives, double motors at hip pitch and knee
- Knee: four-bar linkage with a bent link, motor in the thigh
- Ankle: 2-RSU parallel mechanism, closed form IK via a quadratic in tan(θ/2), forward kinematics solved by a differential Jacobian loop because the analytic one is an 8th order polynomial
- Ankle torque control: feedforward τ_motor = Jᵀ τ_end plus F/T sensor feedback
- Thermal model of the motor (core and housing resistances) gives the max current as a function of the cooler thermal resistance
- Direct-stacked fins: 0.3 mm aluminum fins burred and pressed on the motor with thermal grease, fin length 16 to 28 mm chosen with a fin heat transfer model
- Control: ZMP preview control for CoM, compliant LIPM with spring and damper, steady-state Kalman filter with IMU and ZMP, full-state feedback for CoM damping, capture point feedback for ankle torque and leg length (vertical force) control

## Results that matter

- Leg inertia about the hip 1.119 vs 1.583 kg·m² (70.7%), about the knee 0.185 vs 0.339 kg·m² (54.6%)
- Simulated fast walking (30 cm stride, 0.5 s): ZMP RMS error 0.048 m vs 0.076 m for the conventional leg, 59% larger error with the heavy leg
- Cooling: bare motor 5.4 A, DRC-HUBO+ plate 17.7 A, water 21.0 A (700 g, 8 W), direct-stacked fin about 20.2 A at 130 g and 2.5 W
- Natural frequency about 2 Hz with damping ratio < 0.1 laterally before the CoM damping controller
- Real walking on wood blocks 0.5 to 3 cm high with 5 to 10° local tilt, max walking speed 1.8 km/h

## Interesting stuff

- The cooling part is the most original and practical result, 4x continuous current for 130 g is very good
- Low leg inertia is argued from the LIPM point of view: less swing leg effect means the simple model is more accurate
- The 2-RSU ankle adds compliance and backlash, they say it does not matter because the ankle is torque controlled
- No comparison of the walking controller on the real robot with the heavy leg, only simulation
- Push recovery results are in their other T-RO paper, here only uneven terrain walking

## Useful for me

- **As prior work.** Classic reference for why actuators should be placed proximally and why leg inertia matters. When limbs grow, inertia grows too, and the policy will see a different swing dynamics.
- **Apply.** The thermal model is simple and useful: continuous torque is limited by cooling, so in simulation the torque limits for long tasks should come from a thermal model, not only the peak spec.
- **Watch out.** Parallel ankles add compliance and backlash. In sim-to-real this should be in the randomization.
- **Gap.** Fully model-based control. No learning, and the body is fixed.

## Key prior work

- [[Development of the humanoid disaster response platform DRC-HUBO+|DRC-HUBO+]] · the robot whose electronics and software GAZELLE reuses
- [[ATRIAS - Design and validation of a tether-free 3D-capable spring-mass bipedal robot|ATRIAS]] · fast low-inertia biped design reference
- [[System design and control of anthropomorphic walking robot LOLA|LOLA]] · link-driven joints to reduce leg inertia
- [[Design and comparative analysis of a retrofitted liquid cooling system for high-power actuators|Liquid cooling analysis]] · the motor thermal model they start from
- [[Biped walking stabilization based on foot placement control using capture point feedback|Capture point foot placement]] · their own previous stabilizer
- [[A robust walking controller based on online optimization of ankle, hip, and stepping strategies|Ankle hip stepping optimization]] · the push recovery work on GAZELLE

## References

1. [[The DARPA robotics challenge finals - results and perspectives|DRC finals results]] · Krotkov et al. · 2017 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=The+DARPA+robotics+challenge+finals+results+and+perspectives)
2. [[Team SNU's control strategies for enhancing a robot's capability - lessons from the 2015 DARPA robotics challenge finals|Team SNU DRC]] · Kim et al. · 2017 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=Team+SNU%27s+control+strategies+for+enhancing+a+robot%27s+capability+lessons+from+the+2015+DARPA+robotics+challenge+finals)
3. [[Team IHMC's lessons learned from the DARPA robotics challenge trials|Team IHMC DRC]] · Johnson et al. · 2015 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=Team+IHMC%27s+lessons+learned+from+the+DARPA+robotics+challenge+trials)
4. [[Team WPI-CMU - achieving reliable humanoid behavior in the DARPA robotics challenge|Team WPI-CMU DRC]] · DeDonato et al. · 2017 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=Team+WPI-CMU+achieving+reliable+humanoid+behavior+in+the+DARPA+robotics+challenge)
5. [[Development of the humanoid disaster response platform DRC-HUBO+|DRC-HUBO+]] · Jung et al. · 2018 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Development+of+the+humanoid+disaster+response+platform+DRC-HUBO%2B)
6. [[WALK-MAN humanoid lower body design optimization for enhanced physical performance|WALK-MAN lower body]] · Negrello et al. · 2016 · ICRA · [Scholar](https://scholar.google.com/scholar?q=WALK-MAN+humanoid+lower+body+design+optimization+for+enhanced+physical+performance)
7. [[Development of humanoid robot system for disaster response through team NEDO-JSK's approach to DARPA robotics challenge finals|NEDO-JSK DRC]] · Kakiuchi et al. · 2015 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Development+of+humanoid+robot+system+for+disaster+response+through+team+NEDO-JSK%27s+approach+to+DARPA+robotics+challenge+finals)
8. [[Biped walking robots created at Waseda University - WL and WABIAN family|WL and WABIAN family]] · Lim & Takanishi · 2007 · Phil. Trans. R. Soc. A · [Scholar](https://scholar.google.com/scholar?q=Biped+walking+robots+created+at+Waseda+university+WL+and+WABIAN+family)
9. [[Development of a new humanoid robot WABIAN-2|WABIAN-2]] · Ogura et al. · 2006 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Development+of+a+new+humanoid+robot+WABIAN-2)
10. [[ATRIAS - Design and validation of a tether-free 3D-capable spring-mass bipedal robot|ATRIAS]] · Hubicki et al. · 2016 · IJRR · [Scholar](https://scholar.google.com/scholar?q=ATRIAS+Design+and+validation+of+a+tether-free+3D-capable+spring-mass+bipedal+robot)
11. [[Mechanical design for robot locomotion|Abate thesis]] · Abate · 2018 · PhD thesis, Oregon State University · [Scholar](https://scholar.google.com/scholar?q=Abate+Mechanical+design+for+robot+locomotion)
12. [[Sensors and control concept of a biped robot|Johnnie sensors and control]] · Löffler et al. · 2004 · IEEE TIE · [Scholar](https://scholar.google.com/scholar?q=Sensors+and+control+concept+of+a+biped+robot)
13. [[System design and control of anthropomorphic walking robot LOLA|LOLA]] · Lohmeier et al. · 2009 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=System+design+and+control+of+anthropomorphic+walking+robot+LOLA)
14. [[Walking control of the humanoid platform KHR-1 based on torque feedback control|KHR-1]] · Kim & Oh · 2004 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Walking+control+of+the+humanoid+platform+KHR-1+based+on+torque+feedback+control)
15. [[System design and dynamic walking of humanoid robot KHR-2|KHR-2]] · Kim et al. · 2005 · ICRA · [Scholar](https://scholar.google.com/scholar?q=System+design+and+dynamic+walking+of+humanoid+robot+KHR-2)
16. [[Mechanical design of humanoid robot platform KHR-3 (KAIST Humanoid Robot 3 - HUBO)|KHR-3 HUBO]] · Park et al. · 2005 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Mechanical+design+of+humanoid+robot+platform+KHR-3+KAIST+humanoid+robot+3+HUBO)
17. [[The development of Honda humanoid robot|Honda humanoid]] · Hirai et al. · 1998 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+development+of+Honda+humanoid+robot)
18. [[Fast running experiments involving a humanoid robot|Toyota fast running]] · Tajima et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Fast+running+experiments+involving+a+humanoid+robot)
19. [[Development of the lower limbs for a humanoid robot|Roboray lower limbs]] · Kim et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=Development+of+the+lower+limbs+for+a+humanoid+robot+2012)
20. [[Design and experiments of advanced leg module (HRP-2L) for humanoid robot (HRP-2) development|HRP-2L]] · Kanehira et al. · 2002 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+experiments+of+advanced+leg+module+HRP-2L+for+humanoid+robot+HRP-2+development)
21. [[Cybernetic human HRP-4C|HRP-4C]] · Kaneko et al. · 2009 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Cybernetic+human+HRP-4C)
22. [[Humanoid robot HRP-3|HRP-3]] · Kaneko et al. · 2008 · IROS · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+HRP-3)
23. [[Balance control in whole body coordination framework for biped humanoid robot MAHRU-R|MAHRU-R]] · Chang et al. · 2008 · RO-MAN · [Scholar](https://scholar.google.com/scholar?q=Balance+control+in+whole+body+coordination+framework+for+biped+humanoid+robot+MAHRU-R)
24. [[Overview of the torque-controlled humanoid robot TORO|TORO]] · Englsberger et al. · 2014 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Overview+of+the+torque-controlled+humanoid+robot+TORO)
25. [[COMpliant huMANoid COMAN - Optimal joint stiffness tuning for modal frequency control|COMAN]] · Tsagarakis et al. · 2013 · ICRA · [Scholar](https://scholar.google.com/scholar?q=COMpliant+huMANoid+COMAN+optimal+joint+stiffness+tuning+for+modal+frequency+control)
26. [[Thermal control of electrical motors for high-power humanoid robots|Motor thermal control]] · Urata et al. · 2008 · IROS · [Scholar](https://scholar.google.com/scholar?q=Thermal+control+of+electrical+motors+for+high-power+humanoid+robots)
27. [[Design and comparative analysis of a retrofitted liquid cooling system for high-power actuators|Liquid cooling analysis]] · Paine & Sentis · 2015 · Actuators · [Scholar](https://scholar.google.com/scholar?q=Design+and+comparative+analysis+of+a+retrofitted+liquid+cooling+system+for+high-power+actuators)
28. [[Closed-form inverse kinematic joint solution for humanoid robots|Closed-form humanoid IK]] · Ali et al. · 2010 · IROS · [Scholar](https://scholar.google.com/scholar?q=Closed-form+inverse+kinematic+joint+solution+for+humanoid+robots)
29. [[Robust dynamic walking using online foot step optimization|Online footstep optimization]] · Feng et al. · 2016 · IROS · [Scholar](https://scholar.google.com/scholar?q=Robust+dynamic+walking+using+online+foot+step+optimization)
30. [[Biped walking pattern generation by using preview control of zero-moment point|ZMP preview control]] · Kajita et al. · 2003 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Biped+walking+pattern+generation+by+using+preview+control+of+zero-moment+point)
31. [[Biped walking stabilization based on linear inverted pendulum tracking|LIP tracking stabilization]] · Kajita et al. · 2010 · IROS · [Scholar](https://scholar.google.com/scholar?q=Biped+walking+stabilization+based+on+linear+inverted+pendulum+tracking)
32. [[High frequency walking pattern generation based on preview control of ZMP|High-frequency ZMP preview]] · Nishiwaki & Kagami · 2006 · ICRA · [Scholar](https://scholar.google.com/scholar?q=High+frequency+walking+pattern+generation+based+on+preview+control+of+ZMP)
33. [[Online decision of foot placement using singular LQ preview regulation|Singular LQ foot placement]] · Urata et al. · 2011 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Online+decision+of+foot+placement+using+singular+LQ+preview+regulation)
34. [[Capture Point - A Step toward Humanoid Push Recovery|Capture Point]] · Pratt et al. · 2006 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Capture+point+a+step+toward+humanoid+push+recovery)
35. [[Bipedal walking control based on capture point dynamics|Capture point walking control]] · Englsberger et al. · 2011 · IROS · [Scholar](https://scholar.google.com/scholar?q=Bipedal+walking+control+based+on+capture+point+dynamics)
36. [[Choreonoid - Extensible virtual robot environment built on an integrated GUI framework|Choreonoid]] · Nakaoka · 2012 · SII · [Scholar](https://scholar.google.com/scholar?q=Choreonoid+extensible+virtual+robot+environment+built+on+an+integrated+GUI+framework)
37. [[Balance recovery through model predictive control based on capture point dynamics for biped walking robot|CP-MPC balance recovery]] · Joe & Oh · 2018 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Balance+recovery+through+model+predictive+control+based+on+capture+point+dynamics+for+biped+walking+robot)
38. [[Online walking control system for humanoids with short cycle pattern generation|Short cycle pattern generation]] · Nishiwaki & Kagami · 2009 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Online+walking+control+system+for+humanoids+with+short+cycle+pattern+generation)
39. [[Dynamic balance control algorithm of a six-legged walking robot, Little Crabster|Little Crabster]] · Kim · 2015 · J. Intell. Robot. Syst. · [Scholar](https://scholar.google.com/scholar?q=Dynamic+balance+control+algorithm+of+a+six-legged+walking+robot+little+crabster)
40. [[Biped walking stabilization based on foot placement control using capture point feedback|Capture point foot placement]] · Jeong et al. · 2017 · IROS · [Scholar](https://scholar.google.com/scholar?q=Biped+walking+stabilization+based+on+foot+placement+control+using+capture+point+feedback)
41. [[Experimental realization of dynamic walking of the biped humanoid robot KHR-2 using zero moment point feedback and inertial measurement|KHR-2 dynamic walking]] · Kim et al. · 2006 · Advanced Robotics · [Scholar](https://scholar.google.com/scholar?q=Experimental+realization+of+dynamic+walking+of+the+biped+humanoid+robot+KHR-2+using+zero+moment+point+feedback+and+inertial+measurement)
42. [[A robust walking controller based on online optimization of ankle, hip, and stepping strategies|Ankle hip stepping optimization]] · Jeong et al. · 2019 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=A+robust+walking+controller+based+on+online+optimization+of+ankle+hip+and+stepping+strategies)

## See also

- [[Tello Leg - The Study of Design Principles and Metrics for Dynamic Humanoid Robots|Tello Leg]] · another leg designed around low inertia for dynamic humanoids
- [[Leg Design for a Humanoid Walking Robot|Lola leg]] · earlier link-driven humanoid leg
