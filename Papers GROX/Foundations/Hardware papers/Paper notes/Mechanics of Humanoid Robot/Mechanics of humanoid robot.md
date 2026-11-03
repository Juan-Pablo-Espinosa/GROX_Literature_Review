---
title: "Mechanics of humanoid robot"
aliases:
  - Mechanics of Humanoid Robot
  - Hashimoto survey
authors:
  - "[[Kenji Hashimoto]]"
corresponding: "[[Kenji Hashimoto]]"
institutions:
  - "[[Meiji University]]"
labs: []
countries:
  - Japan
year: 2020
date: 2020-09-04
venue: Advanced Robotics 2020 (Vol. 34, No. 21-22), survey
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1080/01691864.2020.1813624
pdf: https://www.tandfonline.com/doi/pdf/10.1080/01691864.2020.1813624
code: "No code (survey)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/survey
  - topic/humanoids
  - topic/hardware-design
  - topic/leg-design
  - topic/actuators
  - topic/parallel-mechanisms
---

# Mechanics of Humanoid Robot

> [!info] Quick view
> **Authors:** [[Kenji Hashimoto]] (corresponding)
> **From:** [[Meiji University]]
> **Published:** Advanced Robotics · Sep 2020 · survey paper (open access)
> **Keywords:** actuators, strain wave gearing, ball screws, serial vs parallel mechanisms, hip knee ankle actuation, toe joints, foot mechanisms
> **Links:** [DOI](https://doi.org/10.1080/01691864.2020.1813624) · [PDF](https://www.tandfonline.com/doi/pdf/10.1080/01691864.2020.1813624)
> **One line:** a short survey of how humanoid legs are built (actuators, transmissions, serial vs parallel joints, knee and ankle layouts, feet), with the argument that hardware should behave close to the ideal model the controller assumes

## Summary

Hashimoto starts from a practical point: controllers that work in simulation fail on hardware mostly because of model errors like structural deflection, servo stiffness and backlash. So the hardware should be designed to behave like the ideal model, which means stiff links, low backlash, light distal links (inertia scales with the fifth power of size) and a CoM as high as possible (a higher CoM gives a smaller lateral sway for the same single support time, from the LIP model).

Then it goes through the options. Actuators: pneumatic, hydraulic (including electro-hydrostatic like Hydra), electric with multi-motor drives or frameless motors (CHIMP actuator with torque tube, clutch, brake and slip ring). Transmissions: strain wave gears, timing belts before the reducer, cables, ball and roller screws, four-bar linkages. Joints: a table comparing serial vs parallel mechanisms, hip layouts (mostly serial), three knee options (belt plus HD at the knee, crank, ballscrew like LOLA), four ankle options from serial to linear actuators as couplers, and feet with passive or active toes plus a list of patented foot mechanisms. It is short and descriptive, no quantitative comparison, but it is a good map of the design space.

## How it works

- Design principles: high stiffness and low backlash, low distal mass, high CoM
- Serial vs parallel table: workspace, error accumulation, stiffness, inertia, payload, kinematics and dynamics difficulty
- Knee: (a) belt plus strain wave at the knee, (b) crank, (c) ballscrew linear drive, plus variable ratio crossed four-bar
- Ankle: (a) serial HD, (b) parallel crank, (c) linear actuators as length-variable couplers (LOLA, WABIAN-2RIII), (d) screws housed on the shank with spherical joints (Johnnie)
- Feet: passive toe (WABIAN-2, HRP-2LT), active toe (H6, LOLA, HRP-4C), parallel four-bar toe for pad and tip contact, shock absorbing and sensing soles, patents from Honda, Sony, Toyota

## Results that matter

- Survey, no new experiments
- Key message: inertia grows with the fifth power of size, so life-size robots must keep distal mass low
- Parallel ankles are recommended because the pitch axis needs much more torque and speed than roll, and two actuators can share the load
- Linear drives give lighter limbs and stiffer joints but nonlinear input-output relations that complicate control

## Interesting stuff

- Short, readable map of the design space, good first read before the specific leg papers
- Very little about compliance and series elastic actuation, the author says compliant joints are out of scope
- Mostly Japanese and European robots, nothing about learning-based control
- Practical tips like putting the timing belt before the reducer to reduce backlash

## Useful for me

- **As prior work.** The standard citation for humanoid leg mechanics. Useful for the hardware section of any paper.
- **Watch out.** The fifth power scaling of inertia matters a lot for a body that grows: making a limb longer increases its inertia much faster than its mass, so the dynamics the policy sees change strongly with size.
- **Apply.** The serial vs parallel table is a quick checklist for choosing the actuation of a variable-length limb.

## Key prior work

- [[Humanoid robot LOLA|LOLA]] · reference for ballscrew knee and parallel ankle
- [[Leg Design for a Humanoid Walking Robot|Lola leg]] · the ankle coupler concept
- [[CHIMP, the CMU Highly Intelligent Mobile Platform|CHIMP]] · the actuator unit described in detail
- [[Mechanism and control of whole-body electro-hydrostatic actuator driven humanoid robot Hydra|Hydra]] · electro-hydrostatic humanoid
- [[Design of robot leg with variable reduction ratio crossed four-bar linkage mechanism|Variable ratio four-bar knee]] · nonlinear knee transmission
- [[Toe joint mechanism using parallel four-bar linkage enabling humanlike multiple support at toe pad and toe tip|Four-bar toe]] · human-like toe support

## References

1. [[Real-time 3D walking pattern generation for a biped robot with telescopic legs|Meltran V]] · Kajita et al. · 2001 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Real-time+3D+walking+pattern+generation+for+a+biped+robot+with+telescopic+legs)
2. [[Mowgli - A bipedal jumping and landing robot with an artificial musculoskeletal system|Mowgli]] · Niiyama et al. · 2007 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Mowgli+a+bipedal+jumping+and+landing+robot+with+an+artificial+musculoskeletal+system)
3. [[Pneumatic-driven jumping robot with anthropomorphic muscular skeleton structure|Pneumatic jumping robot]] · Hosoda et al. · 2010 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Pneumatic-driven+jumping+robot+with+anthropomorphic+muscular+skeleton+structure)
4. [[Legged Robots That Balance|Legged Robots That Balance]] · Raibert · 1986 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Raibert+Legged+Robots+That+Balance)
5. [[Boston Dynamics home page|Boston Dynamics]] · Boston Dynamics · 2020 · web page · [Link](http://www.bostondynamics.com)
6. [[A high torque to weight ratio robot actuator|High torque-to-weight actuator]] · Bobrow & Desai · 1995 · Robotica · [Scholar](https://scholar.google.com/scholar?q=A+high+torque+to+weight+ratio+robot+actuator)
7. [[Design of a new high performance electrohydraulic actuator|Electrohydraulic actuator]] · Habibi & Goldenberg · 1999 · AIM · [Scholar](https://scholar.google.com/scholar?q=Design+of+a+new+high+performance+electrohydraulic+actuator)
8. [[Backdrivable miniature hydrostatic transmission for actuation of anthropomorphic robot hands|Backdrivable HST]] · Kaminaga et al. · 2007 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Backdrivable+miniature+hydrostatic+transmission+for+actuation+of+anthropomorphic+robot+hands)
9. [[Mechanism and control of whole-body electro-hydrostatic actuator driven humanoid robot Hydra|Hydra]] · Kaminaga et al. · 2017 · ISER 2016 · [Scholar](https://scholar.google.com/scholar?q=Mechanism+and+control+of+whole-body+electro-hydrostatic+actuator+driven+humanoid+robot+hydra)
10. [[Humanoid robot HRP-2Kai - Improvement of HRP-2 towards disaster response tasks|HRP-2Kai]] · Kaneko et al. · 2015 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+HRP-2Kai+improvement+of+HRP-2+towards+disaster+response+tasks)
11. [[Development of life-sized high-power humanoid robot JAXON for real-world use|JAXON]] · Kojima et al. · 2015 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Development+of+life-sized+high-power+humanoid+robot+JAXON+for+real-world+use)
12. [[Robot system of DRC-HUBO+ and control strategy of team KAIST in DARPA robotics challenge finals|DRC-HUBO+ system]] · Lim et al. · 2017 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=Robot+system+of+DRC-HUBO%2B+and+control+strategy+of+team+KAIST+in+DARPA+robotics+challenge+finals)
13. [[Humanoid robot HRP-5P - An electrically actuated humanoid robot with high-power and wide-range joints|HRP-5P]] · Kaneko et al. · 2019 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+HRP-5P+an+electrically+actuated+humanoid+robot+with+high-power+and+wide-range+joints)
14. [[WALK-MAN humanoid lower body design optimization for enhanced physical performance|WALK-MAN lower body]] · Negrello et al. · 2016 · ICRA · [Scholar](https://scholar.google.com/scholar?q=WALK-MAN+humanoid+lower+body+design+optimization+for+enhanced+physical+performance)
15. [[CHIMP, the CMU Highly Intelligent Mobile Platform|CHIMP]] · Stentz et al. · 2015 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=CHIMP+the+CMU+highly+intelligent+mobile+platform)
16. [[ARMAR-6 - A High-Performance Humanoid for Human-Robot Collaboration in Real-World Scenarios|ARMAR-6]] · Asfour et al. · 2019 · IEEE RAM · [Scholar](https://scholar.google.com/scholar?q=ARMAR-6+a+high-performance+humanoid+for+human-robot+collaboration+in+real-world+scenarios)
17. [[WAREC-1 - A four-limbed robot having high locomotion ability with versatility in locomotion styles|WAREC-1]] · Hashimoto et al. · 2017 · SSRR · [Scholar](https://scholar.google.com/scholar?q=WAREC-1+a+four-limbed+robot+having+high+locomotion+ability+with+versatility+in+locomotion+styles)
18. [[Harmonic Drive - High Precision Gear|Harmonic Drive]] · Harmonic Drive · 2020 · web page · [Link](https://www.harmonicdrive.net/)
19. [[Design and development of the biped prototype ROBIAN|ROBIAN]] · Konno et al. · 2002 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+and+development+of+the+biped+prototype+ROBIAN)
20. [[Parallel manipulator hip joint for a bipedal robot|LISA parallel hip]] · Hofschulte et al. · 2005 · CLAWAR · [Scholar](https://scholar.google.com/scholar?q=Parallel+manipulator+hip+joint+for+a+bipedal+robot)
21. [[Development of life-sized humanoid robot platform with robustness for falling down, long time working and error occurrence|RHP2]] · Kakiuchi et al. · 2017 · IROS · [Scholar](https://scholar.google.com/scholar?q=Development+of+life-sized+humanoid+robot+platform+with+robustness+for+falling+down+long+time+working+and+error+occurrence)
22. [[Interactive control of environmental mode for biped walking|Environmental mode control]] · Morisawa & Ohnishi · 2003 · IECON · [Scholar](https://scholar.google.com/scholar?q=Interactive+control+of+environmental+mode+for+biped+walking)
23. [[WL-16RII - Prototype of biped walking wheelchair|WL-16RII]] · Sugahara et al. · 2006 · ROMANSY · [Scholar](https://scholar.google.com/scholar?q=WL-16RII+prototype+of+biped+walking+wheelchair)
24. [[Humanoid robot LOLA|LOLA]] · Lohmeier et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+LOLA)
25. [[Design of robot leg with variable reduction ratio crossed four-bar linkage mechanism|Variable ratio four-bar knee]] · Tomishiro et al. · 2019 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+of+robot+leg+with+variable+reduction+ratio+crossed+four-bar+linkage+mechanism)
26. [[Dynamic gait transition between walking, running and hopping for push recovery|Honda gait transition]] · Kamioka et al. · 2017 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Dynamic+gait+transition+between+walking+running+and+hopping+for+push+recovery)
27. [[Leg body structure of leg type mobile robot|Honda ankle patent]] · Ujino & Miyazaki · 2015 · Patent JP2015-54372 · [Scholar](https://scholar.google.com/scholar?q=Leg+body+structure+of+leg+type+mobile+robot+Honda+patent)
28. [[Leg Design for a Humanoid Walking Robot|Lola leg]] · Lohmeier et al. · 2006 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Leg+design+for+a+humanoid+walking+robot+Lohmeier)
29. [[New shank mechanism for humanoid robot mimicking human-like walking in horizontal and frontal plane|WABIAN-2RIII shank]] · Otani et al. · 2013 · ICRA · [Scholar](https://scholar.google.com/scholar?q=New+shank+mechanism+for+humanoid+robot+mimicking+human-like+walking+in+horizontal+and+frontal+plane)
30. [[Towards the design of a biped jogging robot|Biped jogging robot design]] · Gienger et al. · 2001 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Towards+the+design+of+a+biped+jogging+robot)
31. [[Human-Like Toe Joint Mechanism|Human-like toe joint]] · Yamamoto · 2019 · Humanoid Robotics - A Reference · [Scholar](https://scholar.google.com/scholar?q=Yamamoto+Human-Like+Toe+joint+mechanism)
32. [[Motion and role of the MP joints in walking|MP joints in walking]] · Fujita et al. · 1983 · Biomechanics · [Scholar](https://scholar.google.com/scholar?q=Motion+and+role+of+the+MP+joints+in+walking)
33. [[Human-like walking with knee stretched, heel-contact and toe-off motion by a humanoid robot|WABIAN-2 toe-off walking]] · Ogura et al. · 2006 · IROS · [Scholar](https://scholar.google.com/scholar?q=Human-like+walking+with+knee+stretched+heel-contact+and+toe-off+motion+by+a+humanoid+robot)
34. [[Shoes-wearable foot mechanism mimicking characteristics of human's foot arch and skin|Foot arch mechanism]] · Hashimoto et al. · 2013 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Shoes-wearable+foot+mechanism+mimicking+characteristics+of+human%27s+foot+arch+and+skin)
35. [[ZMP-based biped running enhanced by toe springs|Toe spring running]] · Kajita et al. · 2007 · ICRA · [Scholar](https://scholar.google.com/scholar?q=ZMP-based+biped+running+enhanced+by+toe+springs)
36. [[Toe joints that enhance bipedal and fullbody motion of humanoid robots|Toe joints]] · Nishiwaki et al. · 2002 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Toe+joints+that+enhance+bipedal+and+fullbody+motion+of+humanoid+robots)
37. [[Fast running experiments involving a humanoid robot|Toyota fast running]] · Tajima et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Fast+running+experiments+involving+a+humanoid+robot)
38. [[Hardware improvement of cybernetic human HRP-4C for entertainment use|HRP-4C hardware]] · Kaneko et al. · 2011 · IROS · [Scholar](https://scholar.google.com/scholar?q=Hardware+improvement+of+cybernetic+human+HRP-4C+for+entertainment+use)
39. [[Quick slip-turn of HRP-4C on its toes|HRP-4C slip-turn]] · Miura et al. · 2012 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Quick+slip-turn+of+HRP-4C+on+its+toes)
40. [[Realization of quick turn of biped humanoid robot by using slipping motion with both feet|Quick turn with slipping]] · Hashimoto et al. · 2011 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Realization+of+quick+turn+of+biped+humanoid+robot+by+using+slipping+motion+with+both+feet)
41. [[Toe joint mechanism using parallel four-bar linkage enabling humanlike multiple support at toe pad and toe tip|Four-bar toe]] · Yamamoto et al. · 2009 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Toe+joint+mechanism+using+parallel+four-bar+linkage+enabling+humanlike+multiple+support+at+toe+pad+and+toe+tip)
42. [[Experimental development of foot mechanism with shock absorbing material for acquisition of landing surface position information and stabilization of dynamic biped walking|Shock absorbing foot]] · Yamaguchi et al. · 1995 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Experimental+development+of+foot+mechanism+with+shock+absorbing+material+for+acquisition+of+landing+surface+position+information)
43. [[Walking on gravel with soft soles using linear inverted pendulum tracking and reaction force distribution|Soft soles on gravel]] · Pajon et al. · 2017 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Walking+on+gravel+with+soft+soles+using+linear+inverted+pendulum+tracking+and+reaction+force+distribution)
44. [[Foot part structure for leg type walking robot|Honda foot patent 1993]] · Hirose et al. · 1993 · Patent JP05-293776 · [Scholar](https://scholar.google.com/scholar?q=Foot+part+structure+for+leg+type+walking+robot+Honda)
45. [[Leg type mobile robot (Honda 2006)|Honda foot patent 2006]] · Takenaka et al. · 2006 · Patent JP2006-062057 · [Scholar](https://scholar.google.com/scholar?q=Leg+type+mobile+robot+Honda+patent+JP2006-062057)
46. [[Legged locomotion robot, controlling method for it, and relative movement measuring sensor for it|Sony slip sensor patent]] · Hattori & Yamaguchi · 2001 · Patent JP2001-277159 · [Scholar](https://scholar.google.com/scholar?q=Legged+locomotion+robot+controlling+method+for+it+and+relative+movement+measuring+sensor+for+it)
47. [[Control method of the leg, legged mobile robot|Sony adaptive foot patent]] · Kato et al. · 2003 · Patent WO03/068455 · [Scholar](https://scholar.google.com/scholar?q=Control+method+of+the+leg+legged+mobile+robot+Sony+patent)
48. [[Leg type mobile robot (Toyota 2008)|Toyota foot distance patent]] · Shimada & Matsumoto · 2008 · Patent JP2008-272846 · [Scholar](https://scholar.google.com/scholar?q=Leg+type+mobile+robot+Toyota+patent+JP2008-272846)
49. [[Foot part structure of bipedal walking robot|Toyota foot structure patent]] · Ebihara · 2006 · Patent JP2006-218550 · [Scholar](https://scholar.google.com/scholar?q=Foot+part+structure+of+bipedal+walking+robot+Toyota)
50. [[Compliant Leg Mechanism of COMAN|COMAN compliant leg]] · Tsagarakis et al. · 2017 · Humanoid Robotics - A Reference · [Scholar](https://scholar.google.com/scholar?q=Compliant+Leg+mechanism+of+Coman)
51. [[Development of experimental legged robot for inspection and disaster response in plants|Honda E2-DR]] · Yoshiike et al. · 2017 · IROS · [Scholar](https://scholar.google.com/scholar?q=Development+of+experimental+legged+robot+for+inspection+and+disaster+response+in+plants)

## See also

- [[Bipedal humanoid hardware design - A technology review|Humanoid hardware review]] · the other main survey of humanoid leg hardware
