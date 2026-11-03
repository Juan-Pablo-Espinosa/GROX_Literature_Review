---
title: "Real-world embodied AI through a morphologically adaptive quadruped robot"
aliases:
  - DyRET in the wild
  - "Real-world Embodied AI - A Morphologically Adaptive Quadruped Robot in the Wild"
authors:
  - "[[Tønnes F. Nygaard]]"
  - "[[Kyrre Glette]]"
  - "[[Charles P. Martin]]"
  - "[[Jim Torresen]]"
  - "[[David Howard]]"
corresponding: "[[Tønnes F. Nygaard]]"
institutions:
  - "[[University of Oslo]]"
  - "[[Norwegian Defence Research Establishment]]"
  - "[[Australian National University]]"
  - "[[CSIRO]]"
labs:
  - "[[RITMO]]"
  - "[[CSIRO Cyber-Physical Systems Program]]"
countries:
  - Norway
  - Australia
year: 2021
date: ""
venue: Nature Machine Intelligence 2021 (Vol. 3, No. 5) · this PDF is the arXiv pre-print
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1038/s42256-021-00320-3
pdf: https://doi.org/10.1038/s42256-021-00320-3
code: https://github.com/dyret-robot
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/morphology
  - topic/reconfigurable-robots
  - topic/variable-length-limbs
  - topic/legged-locomotion
  - topic/perception
  - topic/co-design
  - method/regression-model
  - robot/dyret
  - eval/real-robot
---

# DyRET in the Wild

> [!info] Quick view
> **Authors:** [[Tønnes F. Nygaard]] (corresponding), [[Kyrre Glette]], [[Charles P. Martin]], [[Jim Torresen]], [[David Howard]]
> **From:** [[University of Oslo]] · [[RITMO]] · [[Norwegian Defence Research Establishment]] · [[Australian National University]] · [[CSIRO]] · [[CSIRO Cyber-Physical Systems Program]]
> **Published:** Nature Machine Intelligence · 2021 · journal (the PDF in the folder is the pre-print)
> **Keywords:** morphological adaptation, variable leg length, cost of transport, terrain characterization, hardness, roughness, online model update, outdoor quadruped
> **Links:** [DOI](https://doi.org/10.1038/s42256-021-00320-3) · [Code and hardware](https://github.com/dyret-robot)
> **One line:** the DyRET quadruped changes its femur and tibia lengths while walking outdoors, choosing the most energy-efficient leg lengths from sensed terrain hardness and roughness

## Summary

DyRET is a 5 kg open-source quadruped whose femur (+50 mm) and tibia (+100 mm) are prismatic, driven by DC motor lead screws at about 1 mm/s, so leg length can change during operation. The paper builds a terrain-to-morphology model and uses it outdoors. First they walk all 25 leg-length combinations (5 femur × 5 tibia) on concrete, sand and gravel in terrain boxes, about 90 minutes of data, and fit one second-order polynomial regression per morphology from terrain features (hardness from foot force sensors, roughness from a downward RGBD camera) to cost of transport.

Indoors, with a known concrete-to-gravel transition, the robot stops and switches to the best morphology for each terrain, while each specialist static morphology loses 60 to 70% efficiency on the wrong terrain. Outdoors on unseen grass and road, it changes legs while walking, only to neighbor morphologies (one length step), updates the model online, and beats the best all-round static morphology by about 17%, 10% and 26% COT on the three sections, statistically significant. The prediction error drops quickly and is much smaller when it returns to grass. Honestly the controller is a fixed spline crawl gait scaled with leg length, so all the adaptation is in the body, which is exactly the point, but also the main limitation.

## How it works

- Robot: 4 legs, each with 3 Dynamixel servos plus 2 prismatic segments (lead screw, chain drive, guide rails), 60 to 73 cm tall, full tibia change takes about 90 s
- Changing legs changes the workspace a lot: longest legs give about 75% more workspace and lift the body 13 cm, only 6 to 11% overlap between extreme workspaces
- Gait: statically stable spline crawl gait, step height fixed at 100 mm, step length and width scaled with leg length (100% to 125%) to keep servo speed constant
- Terrain features: hardness = max of median-filtered front foot force over 6 s, roughness = mean squared distance of points to a fitted ground plane
- Model: 25 independent second-order polynomial regressions, terrain features to COT, updated with every evaluation
- Online policy: evaluate current morphology for 3 steps per leg, move to the neighbor with the best predicted COT if better than the measured current one

## Results that matter

- Baseline: best on concrete = long femur, short tibia, on sand = short to medium tibia, on gravel = shortest legs, best all-rounder = femur 37.5 mm, tibia 20 mm
- Indoor: concrete specialist COT 23 to 37 on gravel (about 60% worse), gravel specialist 36 to 26, adaptive good on both
- Outdoor: median COT adaptive 22 vs static 27 on first grass (≈17%), ≈10% on road, ≈26% on second grass, all significant
- Prediction error from ≈25 at start to ≈2.5 after first grass, spikes to 8 on road and 11 back on grass but converges in about 8 evaluations
- The adaptive robot uses almost the full morphology range

## Interesting stuff

- Morphology change alone, with a fixed gait, already gives clear energy gains, a strong argument for adaptive bodies
- Only neighbor morphologies are tried because changing legs is slow (1 mm/s), so the search gets stuck in local optima, and they show one good region only explored on the second grass pass
- One model per morphology with no sharing between morphologies, so data efficiency is limited
- Terrain sensing is simple but works, hardness and roughness are good features for leg length choice
- Pre-print says "first quadruped that morphologically adapts outdoors", which seems fair for 2021

## Useful for me

- **As prior work.** Main reference for real-world morphological adaptation in legged robots, very close to the motivation of a humanoid that changes its limbs for the task. Must cite.
- **Apply.** Terrain features → body configuration with an online-updated model is a simple high-level layer that could sit above a learned body-conditioned locomotion policy.
- **Gap.** The controller is not adapted to the body, only scaled. A learned policy conditioned on the current leg lengths could exploit each body much better, and could also handle walking while the legs change.
- **Watch out.** Slow actuators make morphology a slow variable, the policy and the high-level planner should know the transition time.

## Key prior work

- [[Self-Modifying Morphology Experiments with DyRET - Dynamic Robot for Embodied Testing|DyRET self-modifying]] · the robot and first indoor experiments
- [[Real-World Evolution Adapts Robot Morphology and Control to Hardware Limitations|DyRET hardware limitations]] · real-world evolution of morphology and control
- [[Environmental Adaptation of Robot Morphology and Control through Real-world Evolution|DyRET environmental adaptation]] · evolution on different surfaces
- [[How the Body Shapes the Way We Think - A New View of Intelligence|How the Body Shapes the Way We Think]] · the embodied cognition motivation
- [[Evolutionary Developmental Robotics - Improving Morphology and Control of Physical Robots|Evo-devo robotics]] · physical morphology optimization
- [[Variable stiffness morphing limb for amphibious legged robots inspired by chelonian environmental adaptations|Morphing amphibious limb]] · another morphing limb

## References

1. [[Emergency response by robots to Fukushima-Daiichi accident - summary and lessons learned|Fukushima robots]] · Kawatsuma et al. · 2012 · Industrial Robot · [Scholar](https://scholar.google.com/scholar?q=Emergency+response+by+robots+to+Fukushima-Daiichi+accident+summary+and+lessons+learned)
2. [[How the Body Shapes the Way We Think - A New View of Intelligence|How the Body Shapes the Way We Think]] · Pfeifer & Bongard · 2006 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=How+the+body+shapes+the+way+we+think+a+new+view+of+intelligence)
3. [[Embodied cognition is not what you think it is|Embodied cognition]] · Wilson & Golonka · 2013 · Frontiers in Psychology · [Scholar](https://scholar.google.com/scholar?q=Embodied+cognition+is+not+what+you+think+it+is)
4. [[Resilient robots - Concept, review, and future directions|Resilient robots]] · Zhang et al. · 2017 · Robotics · [Scholar](https://scholar.google.com/scholar?q=Resilient+robots+Concept+review+and+future+directions)
5. [[Morphologically induced stability on an underwater legged robot with a deformable body|Underwater legged robot]] · Picardi et al. · 2019 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Morphologically+induced+stability+on+an+underwater+legged+robot+with+a+deformable+body)
6. [[Variable stiffness morphing limb for amphibious legged robots inspired by chelonian environmental adaptations|Morphing amphibious limb]] · Baines et al. · 2020 · Bioinspiration & Biomimetics · [Scholar](https://scholar.google.com/scholar?q=Variable+stiffness+morphing+limb+for+amphibious+legged+robots+inspired+by+chelonian+environmental+adaptations)
7. [[Robotic origamis - Self-morphing modular robot|Robotic origamis]] · Paik et al. · 2012 · ICMC · [Scholar](https://scholar.google.com/scholar?q=Robotic+origamis+Self-morphing+modular+robot)
8. [[Real-World Evolution Adapts Robot Morphology and Control to Hardware Limitations|DyRET hardware limitations]] · Nygaard et al. · 2018 · GECCO · [DOI](https://doi.org/10.1145/3205455.3205567)
9. [[Environmental Adaptation of Robot Morphology and Control through Real-world Evolution|DyRET environmental adaptation]] · Nygaard et al. · 2020 · arXiv · [arXiv](https://arxiv.org/abs/2003.13254)
10. [[A testbed that evolves hexapod controllers in hardware|Hexapod hardware evolution]] · Heijnen et al. · 2017 · ICRA · [Scholar](https://scholar.google.com/scholar?q=A+testbed+that+evolves+hexapod+controllers+in+hardware)
11. [[A review of gait optimization based on evolutionary computation|Evolutionary gait review]] · Gong et al. · 2010 · Appl. Comput. Intell. Soft Comput. · [Scholar](https://scholar.google.com/scholar?q=A+review+of+gait+optimization+based+on+evolutionary+computation)
12. [[Learning to Walk in the Real World with Minimal Human Effort|Walk with minimal human effort]] · Ha et al. · 2020 · arXiv · [arXiv](https://arxiv.org/abs/2002.08550)
13. [[Reinforcement learning in robotics - A survey|RL in robotics survey]] · Kober et al. · 2013 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+learning+in+robotics+A+survey)
14. [[Bayesian optimization for learning gaits under uncertainty|BO gaits]] · Calandra et al. · 2016 · Annals of Mathematics and AI · [Scholar](https://scholar.google.com/scholar?q=Bayesian+optimization+for+learning+gaits+under+uncertainty)
15. [[Combining simulations and real-robot experiments for Bayesian optimization of bipedal gait stabilization|Sim and real BO]] · Rodriguez et al. · 2018 · RoboCup · [Scholar](https://scholar.google.com/scholar?q=Combining+simulations+and+real-robot+experiments+for+bayesian+optimization+of+bipedal+gait+stabilization)
16. [[Perception-less terrain adaptation through whole body control and hierarchical optimization|Perception-less adaptation]] · Bellicoso et al. · 2016 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Perception-less+terrain+adaptation+through+whole+body+control+and+hierarchical+optimization)
17. [[Learning agile and dynamic motor skills for legged robots|ANYmal agile skills]] · Hwangbo et al. · 2019 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+agile+and+dynamic+motor+skills+for+legged+robots)
18. [[Learning, planning, and control for quadruped locomotion over challenging terrain|LittleDog challenging terrain]] · Kalakrishnan et al. · 2011 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Learning+planning+and+control+for+quadruped+locomotion+over+challenging+terrain)
19. [[Fast online adaptation in robotics through meta-learning embeddings of simulated priors|Meta-learned priors]] · Kaushik et al. · 2020 · arXiv · [arXiv](https://arxiv.org/abs/2003.04663)
20. [[STANCE - Locomotion Adaptation over Soft Terrain|STANCE]] · Fahmi et al. · 2020 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Stance+Locomotion+adaptation+over+soft+terrain)
21. [[Walking posture adaptation for legged robot navigation in confined spaces|Posture adaptation confined spaces]] · Buchanan et al. · 2019 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Walking+posture+adaptation+for+legged+robot+navigation+in+confined+spaces)
22. [[Evolving 3D Morphology and Behavior by Competition|Sims 1994]] · Sims · 1994 · Artificial Life · [Scholar](https://scholar.google.com/scholar?q=Evolving+3d+morphology+and+behavior+by+competition)
23. [[Darwin's Devices - What Evolving Robots Can Teach Us About the History of Life and the Future of Technology|Darwin's Devices]] · Long · 2012 · Basic Books · [Scholar](https://scholar.google.com/scholar?q=Darwin%27s+devices+What+evolving+robots+can+teach+us+about+the+history+of+life)
24. [[Learning-In-The-Loop Optimization - End-To-End Control And Co-Design Of Soft Robots Through Learned Deep Latent Representations|Learning-in-the-loop co-design]] · Spielberg et al. · 2019 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Learning-in-the-loop+optimization+End-to-end+control+and+co-design+of+soft+robots+through+learned+deep+latent+representations)
25. [[20 years of reality gap - a few thoughts about simulators in evolutionary robotics|20 years of reality gap]] · Mouret & Chatzilygeroudis · 2017 · GECCO Companion · [Scholar](https://scholar.google.com/scholar?q=20+years+of+reality+gap+a+few+thoughts+about+simulators+in+evolutionary+robotics)
26. [[Automated shapeshifting for function recovery in damaged robots|Automated shapeshifting]] · Kriegman et al. · 2019 · arXiv · [arXiv](https://arxiv.org/abs/1905.09264)
27. [[Online optimization of modular robot locomotion|Modular robot online optimization]] · Marbach & Ijspeert · 2005 · ICMA · [Scholar](https://scholar.google.com/scholar?q=Online+optimization+of+modular+robot+locomotion)
28. [[Optimizing morphology and locomotion on a corpus of parametric legged robots|Parametric legged robots]] · Passault et al. · 2016 · Living Machines · [Scholar](https://scholar.google.com/scholar?q=Optimizing+morphology+and+locomotion+on+a+corpus+of+parametric+legged+robots)
29. [[Automatic design and manufacture of robotic lifeforms|Golem]] · Lipson & Pollack · 2000 · Nature · [Scholar](https://scholar.google.com/scholar?q=Automatic+design+and+manufacture+of+robotic+lifeforms)
30. [[Generative representations for the automated design of modular physical robots|Generative representations]] · Hornby et al. · 2003 · IEEE T-RA · [Scholar](https://scholar.google.com/scholar?q=Generative+representations+for+the+automated+design+of+modular+physical+robots)
31. [[Joint optimization of robot design and motion parameters using the implicit function theorem|Implicit function co-design]] · Ha et al. · 2017 · RSS · [Scholar](https://scholar.google.com/scholar?q=Joint+optimization+of+robot+design+and+motion+parameters+using+the+implicit+function+theorem)
32. [[Scalable sim-to-real transfer of soft robot designs|Soft robot sim-to-real]] · Kriegman et al. · 2019 · arXiv · [arXiv](https://arxiv.org/abs/1911.10290)
33. [[Towards the targeted environment-specific evolution of robot components|Environment-specific components]] · Collins et al. · 2018 · GECCO · [Scholar](https://scholar.google.com/scholar?q=Towards+the+targeted+environment-specific+evolution+of+robot+components)
34. [[Noise and the reality gap - The use of simulation in evolutionary robotics|Noise and the reality gap]] · Jakobi et al. · 1995 · ECAL · [Scholar](https://scholar.google.com/scholar?q=Noise+and+the+reality+gap+The+use+of+simulation+in+evolutionary+robotics)
35. [[Simulation tools for model-based robotics - Comparison of Bullet, Havok, MuJoCo, ODE and PhysX|Simulator comparison]] · Erez et al. · 2015 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Simulation+tools+for+model-based+robotics+Comparison+of+bullet+havok+mujoco+ode+and+physx)
36. [[Modules design of a reconfigurable multi-legged walking robot|Reconfigurable multi-legged modules]] · Sun et al. · 2006 · ROBIO · [Scholar](https://scholar.google.com/scholar?q=Modules+design+of+a+reconfigurable+multi-legged+walking+robot)
37. [[Development of novel robots with modular methodology|Modular methodology robots]] · Guan et al. · 2009 · IROS · [Scholar](https://scholar.google.com/scholar?q=Development+of+novel+robots+with+modular+methodology)
38. [[Evolutionary Developmental Robotics - Improving Morphology and Control of Physical Robots|Evo-devo robotics]] · Vujovic et al. · 2017 · Artificial Life · [Scholar](https://scholar.google.com/scholar?q=Evolutionary+developmental+robotics+Improving+morphology+and+control+of+physical+robots)
39. [[Automated reconfiguration of modular robots using robot manipulators|Automated modular reconfiguration]] · Moreno et al. · 2018 · SSCI · [Scholar](https://scholar.google.com/scholar?q=Automated+reconfiguration+of+modular+robots+using+robot+manipulators)
40. [[Design methodology for constructing multimaterial origami robots and machines|Multimaterial origami robots]] · Zhakypov & Paik · 2018 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Design+methodology+for+constructing+multimaterial+origami+robots+and+machines)
41. [[Agile robotic fliers - A morphing-based approach|Morphing fliers]] · Riviere et al. · 2018 · Soft Robotics · [Scholar](https://scholar.google.com/scholar?q=Agile+robotic+fliers+A+morphing-based+approach)
42. [[Design and control of a passively morphing quadcopter|Passively morphing quadcopter]] · Bucki & Mueller · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+and+control+of+a+passively+morphing+quadcopter)
43. [[Skaterbots - Optimization-based design and motion synthesis for robotic creatures with legs and wheels|Skaterbots]] · Geilinger et al. · 2018 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=Skaterbots+Optimization-based+design+and+motion+synthesis+for+robotic+creatures+with+legs+and+wheels)
44. [[Flying STAR, a hybrid crawling and flying sprawl tuned robot|Flying STAR]] · Meiri & Zarrouk · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Flying+star+a+hybrid+crawling+and+flying+sprawl+tuned+robot)
45. [[A scalable pipeline for designing reconfigurable organisms|Reconfigurable organisms]] · Kriegman et al. · 2020 · PNAS · [Scholar](https://scholar.google.com/scholar?q=A+scalable+pipeline+for+designing+reconfigurable+organisms)
46. [[Shape-Changing Smart Materials|Shape-changing smart materials]] · Ritter · 2007 · Birkhäuser book chapter · [Scholar](https://scholar.google.com/scholar?q=Ritter+Shape-Changing+Smart+Materials)
47. [[Self-Modifying Morphology Experiments with DyRET - Dynamic Robot for Embodied Testing|DyRET self-modifying]] · Nygaard et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Self-modifying+morphology+experiments+with+dyret+Dynamic+robot+for+embodied+testing)
48. [[Design principles for energy-efficient legged locomotion and implementation on the MIT Cheetah robot|MIT Cheetah energy efficiency]] · Seok et al. · 2014 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Design+principles+for+energy-efficient+legged+locomotion+and+implementation+on+the+mit+cheetah+robot)
49. [[Vision-based terrain characterization and traversability assessment|Terrain characterization]] · Howard & Seraji · 2001 · Journal of Robotic Systems · [Scholar](https://scholar.google.com/scholar?q=Vision-based+terrain+characterization+and+traversability+assessment)
50. [[Evolving robots on easy mode - Towards a variable complexity controller for quadrupeds|Variable complexity controller]] · Nygaard et al. · 2019 · EvoApplications · [Scholar](https://scholar.google.com/scholar?q=Evolving+robots+on+easy+mode+Towards+a+variable+complexity+controller+for+quadrupeds)
51. [[How can we ensure visibility and diversity in research contributions - How the Contributor Role Taxonomy (CRediT) is helping the shift from authorship to contributorship|CRediT]] · Allen et al. · 2019 · Learned Publishing · [DOI](https://doi.org/10.1002/leap.1210)

## See also

- [[Multi-embodiment Legged Robot Control as a Sequence Modeling Problem|EAT]] · a learned body-conditioned controller that cites this as motivation
- [[Bioinspired growable humanoid robot with bone-mimetic linkages for versatile mobility|GrowHR]] · the humanoid version of changing limb length
