---
title: "Bioinspired growable humanoid robot with bone-mimetic linkages for versatile mobility"
aliases:
  - GrowHR
authors:
  - "[[Hao Liu]]"
  - "[[Yanda Yang]]"
  - "[[Ting Wang]]"
  - "[[Jintao Yin]]"
  - "[[Hongqiang Wang]]"
corresponding: "[[Hongqiang Wang]]"
institutions:
  - "[[Southern University of Science and Technology]]"
labs: []
countries:
  - China
year: 2026
date: 2026-01-23
venue: Science Advances 2026 (Vol. 12, eaea2831)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://www.science.org/doi/10.1126/sciadv.aea2831
pdf: https://www.science.org/doi/pdf/10.1126/sciadv.aea2831
code: "No official repo (data and code said to be in the paper and supplementary, checked 2026-11-02)"
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/morphology
  - topic/reconfigurable-robots
  - topic/variable-length-limbs
  - topic/variable-stiffness
  - topic/soft-robotics
  - topic/hardware-design
  - topic/legged-locomotion
  - method/pneumatic-actuation
  - eval/real-robot
---

# GrowHR

> [!info] Quick view
> **Authors:** [[Hao Liu]], [[Yanda Yang]], [[Ting Wang]], [[Jintao Yin]], [[Hongqiang Wang]] (corresponding)
> **From:** [[Southern University of Science and Technology]]
> **Published:** Science Advances · Jan 2026 · journal
> **Keywords:** growable humanoid, bone-inspired linkage, inflatable limbs, height change, crawling, swimming, soft humanoid, safety
> **Links:** [DOI](https://doi.org/10.1126/sciadv.aea2831) · [PDF](https://www.science.org/doi/pdf/10.1126/sciadv.aea2831)
> **One line:** a 4.5 kg soft humanoid whose thighs and shanks are inflatable bone-like linkages, so it can change its height from 0.49 to 1.36 m and still walk

## Summary

This is probably the closest hardware paper to the idea of a humanoid that grows its own limbs. Inspired by developing bones, they build a growable linkage: a PVC gasbag inside a non-stretchable fabric cover (like compact bone), two rigid adapters at the ends, a synchronous cable mechanism that keeps the adapters parallel, and three carbon-fiber linear guides for stiffness. Each linkage weighs 350 g, extends 315% (73 to 230 mm) and changes stiffness about 10x. Four of them become the thighs and shanks of GrowHR, with Dynamixel servos at hips, knees and ankles.

The robot changes height by 278%, walks at full and at minimum height, goes under a 0.55 m opening and through a 0.25 m gap, switches to a worm-like crawling mode, floats, swims, walks on water and gets carried by fans and a drone. The honest part is that locomotion is very slow and quasistatic, 11 mm/s at full height, and the controller is an open loop gait with a COM correction for the leg deformation. So the paper is a strong demonstration of a growable body, not of dynamic control of it, and that is exactly the gap a learning approach could fill.

## How it works

- Linkage = PVC gasbag + 0.2 mm fabric cover (16.2x stiffer, 4.7x more payload than without cover) + rubber bands to pull the cover in when deflated
- Synchronous cable-constrained mechanism: four cables on a rotary ring with latex pretension, so all cables extend the same and the adapters stay parallel
- Carbon-fiber linear guides stop tilting, the stability analysis shows the linkage is most fragile when its height is near the adapter radius
- Body: four growable linkages for thighs and shanks, an inflatable gasbag torso, inflatable arms opened by one cable, 10 Dynamixel servos in the legs, two pumps and solenoid valves
- Walking: quasistatic gait planned with DH kinematics, inflation kept above 30 kPa so the legs barely deform, plus a servo angle correction α ≈ asin(Δx/L0) for the remaining deformation
- Crawling: legs split into a line, linkages expand and contract while hip servos lift the moving part to reduce friction
- Swimming: frog kick with leg servos, modeled with the Morison equation and the Swumsuit simulator

## Results that matter

- Height from 0.49 to 1.36 m, total mass 4.5 kg, density 5.8% of water
- Walking speed 11 mm/s at full height and 17 mm/s fully deflated, so very slow and quasistatic
- Without pressure compensation the COM drifts up to 0.192 m, beyond the 0.15 m foot support limit
- Shrinking the legs 50% cuts the leg splitting hip torque from 11.33 to 5.66 Nm, below the 8 Nm stall torque, so changing size makes a motion feasible for weak motors
- Crawling 1.87 mm/s with an external pump, 1122x faster than servos only, but still lower-middle compared with other crawling robots
- Fall impact 599 m/s² vs 1041 m/s² for a rigid copy (42.5% lower), and the rigid copy broke a leg

## Interesting stuff

- The torque argument for leg splitting is the best evidence in the paper that morphology change gives new capabilities, not only new sizes
- Stiffness is pressure dependent, so the dynamics of the legs change with the height, which a controller has to deal with
- The flying and water walking demos are mostly showmanship, the drone carries it and the water walking needs 5 kg weights on each foot
- Remote controlled, no feedback balance control, no learning, the future work even mentions LLMs vaguely
- Height and stiffness are coupled in this design, you cannot be short and stiff or tall and soft independently

## Useful for me

- **As prior work.** Main reference for a humanoid that actually changes limb length on hardware. Any paper on morphologically adaptive humanoids should cite it and compare to it.
- **Apply.** The leg splitting result is a nice motivation: shorter limbs reduce the required torque, so the body can choose its size to make a task feasible with the same motors. This is a good argument for task-driven morphology change.
- **Watch out.** Limb stiffness changes with length and pressure. In simulation, a growing limb should not be modeled as a rigid link whose length just changes, compliance has to vary with it.
- **Gap.** Locomotion here is quasistatic and open loop. A learned policy that walks dynamically while the body grows or shrinks is completely missing, and that is where my work fits.

## Key prior work

- [[A growing soft robot with climbing plant-inspired adaptive behaviors for navigation in unstructured environments|Plant-inspired growing robot]] · the plant growing robots they extend to animal-like locomotion
- [[Toward growing robots - A historical evolution from cellular to plant-inspired robotics|Toward growing robots]] · review of growing robots
- [[Tip-growing robots - Design, theory, application|Tip-growing robots]] · the other main growing robot line
- [[Bipedal humanoid hardware design - A technology review|Humanoid hardware review]] · what rigid humanoid legs look like today
- [[A bipedal walking robot that can fly, slackline, and skateboard|LEO]] · multimodal biped, closest in the "do everything" spirit
- [[Novel design of a soft lightweight pneumatic continuum robot arm with decoupled variable stiffness and positioning|Decoupled stiffness arm]] · soft arm that separates stiffness from position, something GrowHR cannot do

## References

1. [[Introduction to Humanoid Robotics|Introduction to Humanoid Robotics]] · Kajita et al. · 2014 · Springer book · [Scholar](https://scholar.google.com/scholar?q=Introduction+to+Humanoid+Robotics+Kajita)
2. [[Bipedal humanoid hardware design - A technology review|Humanoid hardware review]] · Ficht & Behnke · 2021 · Current Robotics Reports · [Scholar](https://scholar.google.com/scholar?q=Bipedal+humanoid+hardware+design+A+technology+review)
3. [[Humanoid robots from the past to the present|Humanoids past to present]] · Bogue · 2020 · Industrial Robot · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robots+from+the+past+to+the+present+Bogue)
4. [[On human-in-the-loop optimization of human-robot interaction|Human-in-the-loop optimization]] · Slade et al. · 2024 · Nature · [Scholar](https://scholar.google.com/scholar?q=On+human-in-the-loop+optimization+of+human-robot+interaction)
5. [[A comprehensive survey on humanoid robot development|Humanoid development survey]] · Saeedvand et al. · 2019 · Knowledge Engineering Review · [Scholar](https://scholar.google.com/scholar?q=A+comprehensive+survey+on+humanoid+robot+development)
6. [[Safety bounds in human robot interaction - A survey|HRI safety survey]] · Zacharaki et al. · 2020 · Safety Science · [Scholar](https://scholar.google.com/scholar?q=Safety+bounds+in+human+robot+interaction+A+survey)
7. Title not printed · Mukherjee et al. · 2022 · COM-IT-CON · [Scholar](https://scholar.google.com/scholar?q=Mukherjee+Baral+Pal+Chittipaka+Roy+Alam+2022+COM-IT-CON+robot)
8. [[A survey of robots in healthcare|Robots in healthcare survey]] · Kyrarini et al. · 2021 · Technologies · [Scholar](https://scholar.google.com/scholar?q=A+survey+of+robots+in+healthcare)
9. [[Survey on human-robot collaboration in industrial settings - Safety, intuitive interfaces and applications|HRC in industry survey]] · Villani et al. · 2018 · Mechatronics · [Scholar](https://scholar.google.com/scholar?q=Survey+on+human-robot+collaboration+in+industrial+settings+Safety+intuitive+interfaces+and+applications)
10. [[Humanoid robot adoption and labour productivity - A perspective on ambidextrous product innovation routines|Humanoid adoption and productivity]] · Del Giudice et al. · 2022 · Int. J. Hum. Resour. Manag. · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+adoption+and+labour+productivity+A+perspective+on+ambidextrous+product+innovation+routines)
11. [[Centaur - NASA's mobile humanoid designed for field work|Centaur]] · Mehling et al. · 2007 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Centaur+NASA%27s+mobile+humanoid+designed+for+field+work)
12. [[Teleoperation of humanoid robots - A survey|Humanoid teleoperation survey]] · Darvish et al. · 2023 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Teleoperation+of+humanoid+robots+A+survey)
13. [[Taikobot - A full-size and free-flying humanoid robot for intravehicular astronaut assistance and spacecraft housekeeping|Taikobot]] · Zhang et al. · 2022 · Machines · [Scholar](https://scholar.google.com/scholar?q=Taikobot+A+full-size+and+free-flying+humanoid+robot+for+intravehicular+astronaut+assistance+and+spacecraft+housekeeping)
14. [[Robonaut 2 - The first humanoid robot in space|Robonaut 2]] · Diftler et al. · 2011 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Robonaut+2+The+first+humanoid+robot+in+space)
15. [[Development and control of a military rescue robot for casualty extraction task|Military rescue robot]] · Choi et al. · 2019 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=Development+and+control+of+a+military+rescue+robot+for+casualty+extraction+task)
16. [[A bipedal walking robot that can fly, slackline, and skateboard|LEO]] · Kim et al. · 2021 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=A+bipedal+walking+robot+that+can+fly+slackline+and+skateboard)
17. [[Human-in-the-loop control of a humanoid robot for disaster response - A report from the DARPA Robotics Challenge Trials|DRC Trials report]] · DeDonato et al. · 2015 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=Human-in-the-loop+control+of+a+humanoid+robot+for+disaster+response+A+report+from+the+DARPA+Robotics+Challenge+Trials)
18. [[Bone development and growth|Bone development and growth]] · Setiawati & Rahardjo · 2019 · book chapter · [Scholar](https://scholar.google.com/scholar?q=Setiawati+Rahardjo+Bone+development+and+growth)
19. [[The bio-engineering approach for plant investigations and growing robots. A mini-review|Plant growing robots mini-review]] · Mazzolai et al. · 2020 · Frontiers in Robotics and AI · [Scholar](https://scholar.google.com/scholar?q=The+bio-engineering+approach+for+plant+investigations+and+growing+robots+A+mini-review)
20. [[Toward growing robots - A historical evolution from cellular to plant-inspired robotics|Toward growing robots]] · Del Dottore et al. · 2018 · Frontiers in Robotics and AI · [Scholar](https://scholar.google.com/scholar?q=Toward+growing+robots+A+historical+evolution+from+cellular+to+plant-inspired+robotics)
21. [[A growing soft robot with climbing plant-inspired adaptive behaviors for navigation in unstructured environments|Plant-inspired growing robot]] · Del Dottore et al. · 2024 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=A+growing+soft+robot+with+climbing+plant-inspired+adaptive+behaviors+for+navigation+in+unstructured+environments)
22. [[Tip-growing robots - Design, theory, application|Tip-growing robots]] · Al Harthy et al. · 2024 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Tip-growing+robots+Design+theory+application)
23. [[Variable stiffness soft robotic gripper - Design, development, and prospects|Variable stiffness gripper review]] · Shan et al. · 2023 · Bioinspiration & Biomimetics · [Scholar](https://scholar.google.com/scholar?q=Variable+stiffness+soft+robotic+gripper+design+development+and+prospects)
24. [[Novel design of a soft lightweight pneumatic continuum robot arm with decoupled variable stiffness and positioning|Decoupled stiffness arm]] · Giannaccini et al. · 2018 · Soft Robotics · [Scholar](https://scholar.google.com/scholar?q=Novel+design+of+a+soft+lightweight+pneumatic+continuum+robot+arm+with+decoupled+variable+stiffness+and+positioning)
25. [[Collection, collation and analysis of data in relation to reference heights and reference weights for female and male children and adolescents (0-18 years) in the EU|EU reference heights]] · van Buuren et al. · 2012 · EFSA Supporting Publications · [Scholar](https://scholar.google.com/scholar?q=Collection+collation+and+analysis+of+data+in+relation+to+reference+heights+and+reference+weights+for+female+and+male+children+and+adolescents)
26. [[Child-sized 3D printed igus humanoid open platform|igus Humanoid Open Platform]] · Allgeuer et al. · 2015 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Child-sized+3D+printed+igus+humanoid+open+platform)
27. [[Xiaomi presenta CyberOne, su primer robot humanoide|CyberOne]] · Álvarez · 2022 · Computerworld Spain · [Scholar](https://scholar.google.com/scholar?q=Xiaomi+presenta+CyberOne+su+primer+robot+humanoide)
28. [[Overview of the torque-controlled humanoid robot TORO|TORO]] · Englsberger et al. · 2014 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Overview+of+the+torque-controlled+humanoid+robot+TORO)
29. [[NimbRo-OP2 - Grown-up 3D printed open humanoid platform for research|NimbRo-OP2]] · Ficht et al. · 2017 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=NimbRo-OP2+Grown-up+3D+printed+open+humanoid+platform+for+research)
30. [[Lightweight mechatronic system for humanoid robot|Lightweight humanoid mechatronics]] · Jeong et al. · 2023 · Knowledge Engineering Review · [Scholar](https://scholar.google.com/scholar?q=Lightweight+mechatronic+system+for+humanoid+robot)
31. [[Development of the humanoid disaster response platform DRC-HUBO+|DRC-HUBO+]] · Jung et al. · 2018 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Development+of+the+humanoid+disaster+response+platform+DRC-HUBO%2B)
32. [[Humanoid robot HRP-5P - An electrically actuated humanoid robot with high-power and wide-range joints|HRP-5P]] · Kaneko et al. · 2019 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+HRP-5P+An+electrically+actuated+humanoid+robot+with+high-power+and+wide-range+joints)
33. [[Humanoid Robots at work - where are we|Humanoid robots at work]] · Noreils · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2404.04249)
34. [[Novel lightweight odometric learning method for humanoid robot localization|Odometric learning]] · Saeedvand et al. · 2018 · Mechatronics · [Scholar](https://scholar.google.com/scholar?q=Novel+lightweight+odometric+learning+method+for+humanoid+robot+localization)
35. [[The intelligent ASIMO - System overview and integration|ASIMO]] · Sakagami et al. · 2002 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+intelligent+ASIMO+System+overview+and+integration)
36. [[TALOS - A new humanoid research platform targeted for industrial applications|TALOS]] · Stasse et al. · 2017 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=TALOS+A+new+humanoid+research+platform+targeted+for+industrial+applications)
37. [[Cyber-Human Systems, Space Technologies, and Threats|Cyber-Human Systems book]] · Nichols et al. · 2023 · book · [Scholar](https://scholar.google.com/scholar?q=Cyber-Human+Systems+Space+Technologies+and+Threats)
38. [[Lightweight design optimization for legs of bipedal humanoid robot|Leg lightweight optimization]] · Ding et al. · 2021 · Structural and Multidisciplinary Optimization · [Scholar](https://scholar.google.com/scholar?q=Lightweight+design+optimization+for+legs+of+bipedal+humanoid+robot)
39. [[Stair-climbing robots - A review on mechanism, sensing, and performance evaluation|Stair-climbing review]] · Seo et al. · 2023 · IEEE Access · [Scholar](https://scholar.google.com/scholar?q=Stair-climbing+robots+A+review+on+mechanism+sensing+and+performance+evaluation)
40. Title not printed · Malik et al. · 2024 · HRI Companion · [Scholar](https://scholar.google.com/scholar?q=Malik+Masood+Brem+2024+HRI+companion+humanoid)
41. [[Year of the Humanoid - Legged robots from eight companies vie for jobs|Year of the Humanoid]] · Ackerman · 2024 · IEEE Spectrum · [Scholar](https://scholar.google.com/scholar?q=Year+of+the+Humanoid+Legged+robots+from+eight+companies+vie+for+jobs)
42. [[Reduced-dimensional whole-body control based on model simplification for bipedal robots with parallel mechanisms|Reduced-dimensional WBC]] · Liang et al. · 2025 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Reduced-dimensional+whole-body+control+based+on+model+simplification+for+bipedal+robots+with+parallel+mechanisms)
43. Title not printed (CSRT tracking) · Farkhodov et al. · 2020 · Bioimaging · [Scholar](https://scholar.google.com/scholar?q=Farkhodov+Lee+Kwon+2020+Bioimaging+CSRT+tracking)
44. Title not printed · Thakker et al. · 2014 · IROS · [Scholar](https://scholar.google.com/scholar?q=Thakker+Kamat+Bharambe+Chiddarwar+Bhurchandi+2014+IROS)
45. [[Design of a new air pressure perception multi-cavity pneumatic-driven earthworm-like soft robot|Earthworm-like soft robot]] · Tang et al. · 2020 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Design+of+a+new+air+pressure+perception+multi-cavity+pneumatic-driven+earthworm-like+soft+robot)
46. [[An in-pipe worm robot with pneumatic actuators based on origami paper-fabric composites|In-pipe worm robot]] · Jiang & Pei · 2021 · Textile Research Journal · [Scholar](https://scholar.google.com/scholar?q=An+in-pipe+worm+robot+with+pneumatic+actuators+based+on+origami+paper-fabric+composites)
47. [[Review of snake robots in constrained environments|Snake robots review]] · Liu et al. · 2021 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Review+of+snake+robots+in+constrained+environments)
48. [[Theoretical modelling and experimental analysis of the vertical stiffness of a convoluted air spring including the effect of the stiffness of the bellows|Air spring stiffness]] · Chen et al. · 2018 · Proc. IMechE Part D · [Scholar](https://scholar.google.com/scholar?q=Theoretical+modelling+and+experimental+analysis+of+the+vertical+stiffness+of+a+convoluted+air+spring+including+the+effect+of+the+stiffness+of+the+bellows)
49. [[A note on Denavit-Hartenberg notation in robotics|DH notation note]] · Lipkin · 2005 · ASME IDETC · [Scholar](https://scholar.google.com/scholar?q=A+note+on+Denavit-Hartenberg+notation+in+robotics)
50. [[Modelling of snake-like locomotion|Snake-like locomotion model]] · Chernousko · 2005 · Applied Mathematics and Computation · [Scholar](https://scholar.google.com/scholar?q=Modelling+of+snake-like+locomotion+Chernousko)
51. Title not printed · Yi et al. · 2016 · AIM · [Scholar](https://scholar.google.com/scholar?q=Yi+Jianqing+2016+AIM+swimming+robot+Morison)
52. Title not printed (Swumsuit) · Nakashima · 2005 · ISCSB · [Scholar](https://scholar.google.com/scholar?q=Nakashima+2005+ISCSB+swimming+simulation)
53. [[Development of swimming human simulation model considering rigid body dynamics and unsteady fluid force for whole body|Swumsuit model]] · Nakashima et al. · 2007 · J. Fluid Sci. Technol. · [Scholar](https://scholar.google.com/scholar?q=Development+of+swimming+human+simulation+model+considering+rigid+body+dynamics+and+unsteady+fluid+force+for+whole+body)
54. [[Design and modeling of a parallel-pipe-crawling pneumatic soft robot|Parallel-pipe crawling robot]] · Zhang et al. · 2019 · IEEE Access · [Scholar](https://scholar.google.com/scholar?q=Design+and+modeling+of+a+parallel-pipe-crawling+pneumatic+soft+robot)
55. [[A reconfigurable omnidirectional soft robot based on caterpillar locomotion|Caterpillar soft robot]] · Zou et al. · 2018 · Soft Robotics · [Scholar](https://scholar.google.com/scholar?q=A+reconfigurable+omnidirectional+soft+robot+based+on+caterpillar+locomotion)
56. Title not printed · Joey et al. · 2017 · ROBIO · [Scholar](https://scholar.google.com/scholar?q=Joey+Calderon+Perez-Arancibia+2017+ROBIO+soft+robot)
57. [[Earthworm-inspired multimodal pneumatic continuous soft robot enhanced by winding transmission|Earthworm multimodal robot]] · Liu et al. · 2025 · Cyborg and Bionic Systems · [Scholar](https://scholar.google.com/scholar?q=Earthworm-inspired+multimodal+pneumatic+continuous+soft+robot+enhanced+by+winding+transmission)
58. [[An earthworm-like modular soft robot for locomotion in multi-terrain environments|Earthworm modular robot]] · Das et al. · 2023 · Scientific Reports · [Scholar](https://scholar.google.com/scholar?q=An+earthworm-like+modular+soft+robot+for+locomotion+in+multi-terrain+environments)
59. [[A study of energy-efficient and optimal locomotion in a pneumatic artificial muscle-driven snake robot|PAM snake robot]] · Lopez & Naghshenas-Jaryani · 2023 · Robotics · [Scholar](https://scholar.google.com/scholar?q=A+study+of+energy-efficient+and+optimal+locomotion+in+a+pneumatic+artificial+muscle-driven+snake+robot)
60. [[A textile origami snake robot for rectilinear locomotion|Textile origami snake]] · Seyidoğlu & Rafsanjani · 2024 · Device · [Scholar](https://scholar.google.com/scholar?q=A+textile+origami+snake+robot+for+rectilinear+locomotion)
61. [[Worm-inspired, untethered, soft crawling robots for pipe inspections|Worm pipe inspection robots]] · Zhao et al. · 2024 · Soft Robotics · [Scholar](https://scholar.google.com/scholar?q=Worm-inspired+untethered+soft+crawling+robots+for+pipe+inspections)
62. [[Bioinspired 3D-printed snakeskins enable effective serpentine locomotion of a soft robotic snake|3D-printed snakeskins]] · Qi et al. · 2023 · Soft Robotics · [Scholar](https://scholar.google.com/scholar?q=Bioinspired+3D-printed+snakeskins+enable+effective+serpentine+locomotion+of+a+soft+robotic+snake)
63. [[Development of an inchworm-like soft pipe robot for detection|Inchworm pipe robot]] · Li et al. · 2023 · Int. J. Mech. Sci. · [Scholar](https://scholar.google.com/scholar?q=Development+of+an+inchworm-like+soft+pipe+robot+for+detection)

## See also

- [[A Compliant Telescopic Limb with Anisotropic Stiffness|Hexo-Flexo Telescopic Limb]] · same idea of a rigid core inside a pneumatic limb, earlier and simpler
- [[High Dynamic Bipedal Robot with Underactuated Telescopic Straight Legs|Telescopic straight legs biped]] · rigid telescopic legs on a dynamic biped
