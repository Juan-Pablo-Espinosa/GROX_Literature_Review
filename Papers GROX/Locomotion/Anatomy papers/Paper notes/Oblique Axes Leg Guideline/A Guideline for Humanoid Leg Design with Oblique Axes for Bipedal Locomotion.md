---
title: "A Guideline for Humanoid Leg Design with Oblique Axes for Bipedal Locomotion"
aliases:
  - Oblique Axes Leg Guideline
authors:
  - "[[Konrad Fründ]]"
  - "[[Anton Leonhard Shu]]"
  - "[[Florian Christoph Loeffl]]"
  - "[[Christian Ott]]"
corresponding: ""
institutions:
  - "[[German Aerospace Center]]"
  - "[[TU Wien]]"
labs:
  - "[[DLR Institute of Robotics and Mechatronics]]"
  - "[[TU Wien Automation and Control Institute]]"
countries:
  - Germany
  - Austria
year: 2022
date: 2022-11-28
venue: IEEE-RAS Humanoids 2022
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1109/HUMANOIDS53995.2022.10000126
pdf: https://doi.org/10.1109/HUMANOIDS53995.2022.10000126
code: "No code (analysis paper, checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/leg-design
  - topic/biomechanics
  - topic/hardware-design
  - topic/legged-locomotion
  - topic/actuators
  - eval/analytical
---

# Oblique Axes Leg Guideline

> [!info] Quick view
> **Authors:** [[Konrad Fründ]], [[Anton Leonhard Shu]], [[Florian Christoph Loeffl]], [[Christian Ott]]
> **From:** [[German Aerospace Center]] · [[DLR Institute of Robotics and Mechatronics]] · [[TU Wien]] · [[TU Wien Automation and Control Institute]]
> **Published:** IEEE-RAS Humanoids 2022 · Nov 2022 · conference
> **Keywords:** oblique joint axes, knee axis, subtalar joint, hip rotation, step width, biomechanics, actuator torque, walking vs running
> **Links:** [DOI](https://doi.org/10.1109/HUMANOIDS53995.2022.10000126)
> **One line:** human leg joint axes are tilted, not perpendicular, and tilting a humanoid's knee, subtalar and hip axes in the same way can lower actuator and bearing loads

## Summary

Most humanoids use the classic six joint leg with perpendicular axes (hip yaw-roll-pitch, knee, ankle pitch-roll). The human leg is different, the knee axis is tilted about 3° in adduction, the upper ankle around 84° and 80°, the subtalar joint 23° and 42°. This paper reviews these oblique axes joint by joint, explains the mechanism behind each (narrow step width, constraint torque, rocker gait, shared load) and turns them into a guideline for humanoid leg kinematics, separated for walking and running.

The arguments are back-of-the-envelope geometry: with a narrow step width an oblique knee puts all the torque in the sagittal plane, tilting the tibia toward the GRF reduces knee constraint torque about 25%, and rotating the hip pitch and roll axes 26° in the transversal plane drops the peak hip torque from about 88 to 70 Nm on a human running dataset. There is no robot, no simulation and no gait optimization, so it is a guideline in the literal sense. Still, the hip load sharing idea is basically what LOLA and ARMAR-4 already do and is easy to evaluate in simulation.

## How it works

- Goes through the knee, upper ankle, subtalar and hip, for each one gives the human biomechanics and then the mechanism a robot could copy
- Knee: polycentric four-bar for passive stance stability in walking, oblique axis for a narrow step width, tilted tibia to reduce medio-lateral constraint torque
- Upper ankle: obliquity only makes sense together with an elastic foot that guides the center of pressure (windlass effect, up to 17% energy return)
- Subtalar: the 42° sagittal tilt lowers the impact torque at heel strike and helps lateral and turning push-off, but in stance it adds a transversal torque the hip must cancel
- Hip: rotate the pitch and roll axes by θtrans so the sagittal and frontal torque peaks are shared by two motors
- Everything is computed with simple trigonometric ratios on human anthropometry and a public running dataset at 2.5 m/s

## Results that matter

- Oblique knee: only around 0.1% torque saving at walking step width, so this one is basically negligible for torque
- Tilting the tibia toward the GRF reduces knee constraint torque about 25%, and bearing mass scales with constraint torque to the 3/2
- Oblique subtalar at heel strike: lower impact torque on the joint, and a non-oblique subtalar needs about 34.5% more torque for a lateral push-off
- Hip rotated by 26°: max actuator torque goes from about 88 Nm to 70 Nm on human running data
- Walking benefits more from oblique axes than running, for running the main goal is to align motion with the sagittal plane

## Interesting stuff

- Several numbers in the PDF text are garbled (percent and decimals), so always check the figures, for example the hip plot shows 88.26 vs 69.69 Nm
- The hip load sharing is the strongest and simplest argument, any two-actuator joint whose torque peaks happen at different times in different planes can benefit
- No validation at all, they say whole-body simulation will come later
- The trade-off is control and coupling complexity, rotated axes make kinematics and model identification less clean
- The human reference values change with body size (smaller people and wider hips give more knee adduction), which is a kinda nice hint that the optimal tilt depends on proportions

## Useful for me

- **Apply.** The hip rotation trick is easy to test in simulation: take joint torque logs from a trained policy, rotate the frame, and check how much the peak torque per actuator drops.
- **Watch out.** The optimal oblique angles depend on step width, hip width and leg length. If the body proportions change, a fixed tilt that is optimal for one configuration will not be optimal for the others.
- **As prior work.** Good reference when justifying non-standard leg kinematics, together with the hip axes rotation papers from Zorjan and Hugel.
- **Gap.** Nobody combines oblique axes with learned locomotion here. Whether an RL policy actually exploits these geometries is still open.

## Key prior work

- [[Humanoid robot LOLA|LOLA]] · real humanoid that already uses an oblique hip axis
- [[ARMAR-4 - A 63 DOF torque controlled humanoid robot|ARMAR-4]] · the other robot with rotated hip axes they point to
- [[Generalized humanoid leg inverse kinematics to deal with singularities|Generalized leg IK]] · Zorjan and Hugel, rotated hip to avoid singularities
- [[The major determinants in normal and pathological gait|Determinants of gait]] · the origin of the standard six joint leg layout
- [[A public dataset of running biomechanics and the effects of running speed on lower extremity kinematics and kinetics|Running biomechanics dataset]] · the human data used for the hip torque numbers
- [[The spring-mass model for running and hopping|Spring-mass model]] · the SLIP template behind the running guideline

## References

1. [[Passive Dynamic Walking|Passive Dynamic Walking]] · McGeer · 1990 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Passive+Dynamic+Walking+McGeer)
2. [[The 3D linear inverted pendulum mode - A simple modeling for a biped walking pattern generation|3D-LIPM]] · Kajita et al. · 2001 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+3D+linear+inverted+pendulum+mode+A+simple+modeling+for+a+biped+walking+pattern+generation)
3. [[The spring-mass model for running and hopping|Spring-mass model]] · Blickhan · 1989 · Journal of Biomechanics · [Scholar](https://scholar.google.com/scholar?q=The+spring-mass+model+for+running+and+hopping)
4. [[Compliant leg behaviour explains basic dynamics of walking and running|Compliant leg behaviour]] · Geyer et al. · 2006 · Proc. Royal Society B · [Scholar](https://scholar.google.com/scholar?q=Compliant+leg+behaviour+explains+basic+dynamics+of+walking+and+running)
5. [[High-speed humanoid running through control with a 3D-SLIP model|3D-SLIP running]] · Wensing & Orin · 2013 · IROS · [Scholar](https://scholar.google.com/scholar?q=High-speed+humanoid+running+through+control+with+a+3D-SLIP+model)
6. [[SLIP running with an articulated robotic leg|SLIP articulated leg]] · Hutter et al. · 2010 · IROS · [Scholar](https://scholar.google.com/scholar?q=SLIP+running+with+an+articulated+robotic+leg)
7. [[Endurance running and the evolution of Homo|Endurance running]] · Bramble & Lieberman · 2004 · Nature · [Scholar](https://scholar.google.com/scholar?q=Endurance+running+and+the+evolution+of+Homo)
8. [[Rethinking the evolution of the human foot - Insights from experimental research|Evolution of the human foot]] · Holowka & Lieberman · 2018 · Journal of Experimental Biology · [Scholar](https://scholar.google.com/scholar?q=Rethinking+the+evolution+of+the+human+foot+Insights+from+experimental+research)
9. [[Foot Biomechanics During Walking and Running|Foot biomechanics]] · Chan & Rudins · 1994 · Mayo Clinic Proceedings · [Scholar](https://scholar.google.com/scholar?q=Foot+Biomechanics+During+Walking+and+Running+Chan+Rudins)
10. [[Study of the human foot for the design of an anthropomorphic robot foot|Anthropomorphic robot foot thesis]] · Lee · 2008 · thesis · [Scholar](https://scholar.google.com/scholar?q=Study+of+the+human+foot+for+the+design+of+an+anthropomorphic+robot+foot)
11. [[Bipedal walking with oblique mid-foot joint in foot|Oblique mid-foot joint]] · Kawakami & Hosoda · 2015 · ROBIO · [Scholar](https://scholar.google.com/scholar?q=Bipedal+walking+with+oblique+mid-foot+joint+in+foot)
12. [[The design of an anthropomorphic dexterous humanoid foot|Dexterous humanoid foot]] · Davis & Caldwell · 2010 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+design+of+an+anthropomorphic+dexterous+humanoid+foot)
13. [[Development of the skewed rotation plane (SRP) waist joint for humanoid robots|SRP waist]] · Ellenberg et al. · 2013 · TePRA · [Scholar](https://scholar.google.com/scholar?q=Development+of+the+skewed+rotation+plane+SRP+waist+joint+for+humanoid+robots)
14. [[ARMAR-4 - A 63 DOF torque controlled humanoid robot|ARMAR-4]] · Asfour et al. · 2013 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=ARMAR-4+A+63+DOF+torque+controlled+humanoid+robot)
15. [[Humanoid robot LOLA|LOLA]] · Lohmeier et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+LOLA)
16. [[Human mimetic musculoskeletal humanoid Kengoro toward real world physically interactive actions|Kengoro]] · Asano et al. · 2016 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Human+mimetic+musculoskeletal+humanoid+Kengoro+toward+real+world+physically+interactive+actions)
17. [[Mechanical, Anatomical, and Kinematic Axis in TKA - Concepts and Practical Applications|Axes in TKA]] · Cherian et al. · 2014 · Curr Rev Musculoskelet Med · [Scholar](https://scholar.google.com/scholar?q=Mechanical+Anatomical+and+Kinematic+Axis+in+TKA+Concepts+and+Practical+Applications)
18. [[The major determinants in normal and pathological gait|Determinants of gait]] · Saunders et al. · 1953 · J Bone Joint Surg Am · [Scholar](https://scholar.google.com/scholar?q=The+major+determinants+in+normal+and+pathological+gait)
19. [[The Movement of the Knee Studied by Magnetic Resonance Imaging|Knee MRI motion]] · Freeman & Pinskerova · 2003 · Clin Orthop Relat Res · [Scholar](https://scholar.google.com/scholar?q=The+Movement+of+the+Knee+Studied+by+Magnetic+Resonance+Imaging)
20. [[Designing for Scale - Development of the ReMotion Knee for Global Emerging Markets|ReMotion Knee]] · Hamner et al. · 2013 · Ann Biomed Eng · [Scholar](https://scholar.google.com/scholar?q=Designing+for+Scale+Development+of+the+ReMotion+Knee+for+Global+Emerging+Markets)
21. [[The biomechanics of running|Biomechanics of running]] · Novacheck · 1998 · Gait & Posture · [Scholar](https://scholar.google.com/scholar?q=The+biomechanics+of+running+Novacheck)
22. [[The Physiology of the Joints - Annotated Diagrams of the Mechanics of the Human Joints|Physiology of the Joints]] · Kapandji et al. · 1974 · Churchill Livingstone book · [Scholar](https://scholar.google.com/scholar?q=Kapandji+The+Physiology+of+the+Joints)
23. [[Relationship between foot placement and mediolateral ground reaction forces during running|Foot placement and ML GRF]] · McClay & Cavanagh · 1994 · Clinical Biomechanics · [Scholar](https://scholar.google.com/scholar?q=Relationship+between+foot+placement+and+mediolateral+ground+reaction+forces+during+running)
24. [[The effects of step width and arm swing on energetic cost and lateral balance during running|Step width and arm swing]] · Arellano & Kram · 2011 · Journal of Biomechanics · [Scholar](https://scholar.google.com/scholar?q=The+effects+of+step+width+and+arm+swing+on+energetic+cost+and+lateral+balance+during+running)
25. [[Walking with wider steps increases stance phase gluteus medius activity|Wider steps gluteus medius]] · Kubinski et al. · 2015 · Gait & Posture · [Scholar](https://scholar.google.com/scholar?q=Walking+with+wider+steps+increases+stance+phase+gluteus+medius+activity)
26. [[Gait Analysis - An Introduction|Gait Analysis, Whittle]] · Whittle · 2014 · Elsevier book · [Scholar](https://scholar.google.com/scholar?q=Whittle+Gait+Analysis+An+Introduction)
27. [[Body Segment Parameters|Body Segment Parameters]] · Drillis & Contini · 1966 · NYU tech report · [Scholar](https://scholar.google.com/scholar?q=Drillis+Contini+Body+Segment+Parameters)
28. [[Biomechanics of Human Motion|Biomechanics of Human Motion]] · Williams & Lissner · 1962 · Saunders book · [Scholar](https://scholar.google.com/scholar?q=Williams+Lissner+Biomechanics+of+Human+Motion)
29. [[Clinical evaluation of the treatment of osteoarthritic knees using a newly designed wedged insole|Wedged insole]] · Sasaki & Yasuda · 1987 · Clin Orthop Relat Res · [Scholar](https://scholar.google.com/scholar?q=Clinical+evaluation+of+the+treatment+of+osteoarthritic+knees+using+a+newly+designed+wedged+insole)
30. [[Estimation models for the preliminary design of electromechanical actuators|EMA estimation models]] · Budinger et al. · 2012 · Proc. IMechE Part G · [Scholar](https://scholar.google.com/scholar?q=Estimation+models+for+the+preliminary+design+of+electromechanical+actuators)
31. [[The geometrical axis of the talocrural joint - Suggestions for a new measurement of the talocrural joint axis|Talocrural axis]] · Claassen et al. · 2019 · Foot and Ankle Surgery · [Scholar](https://scholar.google.com/scholar?q=The+geometrical+axis+of+the+talocrural+joint+Suggestions+for+a+new+measurement+of+the+talocrural+joint+axis)
32. [[Biomechanics of the ankle|Biomechanics of the ankle]] · Brockett & Chapman · 2016 · Orthopaedics and Trauma · [Scholar](https://scholar.google.com/scholar?q=Brockett+Chapman+Biomechanics+of+the+ankle)
33. [[Kinesiology of the Musculoskeletal System - Foundations for Rehabilitation|Kinesiology of the Musculoskeletal System]] · Neumann · 2017 · Elsevier book · [Scholar](https://scholar.google.com/scholar?q=Neumann+Kinesiology+of+the+Musculoskeletal+System+Foundations+for+Rehabilitation)
34. [[Functional Anatomy, Pathomechanics, and Pathophysiology of Lateral Ankle Instability|Lateral ankle instability]] · Hertel · 2002 · Journal of Athletic Training · [Scholar](https://scholar.google.com/scholar?q=Functional+Anatomy+Pathomechanics+and+Pathophysiology+of+Lateral+Ankle+Instability)
35. [[Humanlike ankle-foot complex for a biped robot|Humanlike ankle-foot]] · Narioka et al. · 2012 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Humanlike+ankle-foot+complex+for+a+biped+robot)
36. [[The spring in the arch of the human foot|Spring in the arch]] · Ker et al. · 1987 · Nature · [Scholar](https://scholar.google.com/scholar?q=The+spring+in+the+arch+of+the+human+foot)
37. [[Rehabilitation in Orthopädie und Unfallchirurgie|Rehabilitation in Orthopädie]] · Stein & Greitemann (eds.) · 2005 · Springer book · [Scholar](https://scholar.google.com/scholar?q=Rehabilitation+in+Orthop%C3%A4die+und+Unfallchirurgie)
38. [[Biomechanics of the subtalar joint complex|Subtalar joint complex]] · Sarrafian · 1993 · Clin Orthop Relat Res · [Scholar](https://scholar.google.com/scholar?q=Biomechanics+of+the+subtalar+joint+complex)
39. [[Gait Analysis - Normal and Pathological Function|Gait Analysis, Perry]] · Perry · 1992 · SLACK book · [Scholar](https://scholar.google.com/scholar?q=Perry+Gait+Analysis+Normal+and+Pathological+Function)
40. [[Heel strike angle and foot angular velocity in the sagittal plane during running in different shoe conditions|Heel strike angle]] · Heidenfelder et al. · 2008 · J Foot Ankle Res · [Scholar](https://scholar.google.com/scholar?q=Heel+strike+angle+and+foot+angular+velocity+in+the+sagittal+plane+during+running+in+different+shoe+conditions)
41. [[A public dataset of running biomechanics and the effects of running speed on lower extremity kinematics and kinetics|Running biomechanics dataset]] · Fukuchi et al. · 2017 · PeerJ · [Scholar](https://scholar.google.com/scholar?q=A+public+dataset+of+running+biomechanics+and+the+effects+of+running+speed+on+lower+extremity+kinematics+and+kinetics)
42. [[Power analysis of gait|Power analysis of gait]] · Thompson · 2002 · web page · [Link](https://ouhsc.edu/bserdac/dthompso/web/gait/epow/pow1.htm)
43. [[Generalized humanoid leg inverse kinematics to deal with singularities|Generalized leg IK]] · Zorjan & Hugel · 2013 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Generalized+humanoid+leg+inverse+kinematics+to+deal+with+singularities)

## See also

- [[Influence of rotation of humanoid hip joint axes on joint power during locomotion|Hip axes rotation and joint power]] · the quantitative version of the hip rotation argument
- [[Comparison Between Different Humanoid Leg Hip Kinematics Trough Dynamic Simulation|Hip kinematics comparison]] · simulation comparison of hip layouts
