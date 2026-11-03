---
title: "Self-Modifying Morphology Experiments with DyRET: Dynamic Robot for Embodied Testing"
aliases:
  - DyRET Self-Modifying
  - DyRET self-modifying
  - DyRET
authors:
  - "[[Tønnes F. Nygaard]]"
  - "[[Charles P. Martin]]"
  - "[[Jim Torresen]]"
  - "[[Kyrre Glette]]"
corresponding: "[[Tønnes F. Nygaard]]"
institutions:
  - "[[University of Oslo]]"
labs:
  - "[[RITMO]]"
countries:
  - Norway
year: 2019
date: 2019-05-20
venue: IEEE ICRA 2019
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://scholar.google.com/scholar?q=Self-Modifying+Morphology+Experiments+with+DyRET+Dynamic+Robot+for+Embodied+Testing
pdf: ""
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
  - topic/hardware-design
  - topic/actuators
  - robot/dyret
  - sim/gazebo
  - eval/real-robot
---

# DyRET Self-Modifying

> [!info] Quick view
> **Authors:** [[Tønnes F. Nygaard]] (corresponding), [[Charles P. Martin]], [[Jim Torresen]], [[Kyrre Glette]]
> **From:** [[University of Oslo]] · [[RITMO]]
> **Published:** IEEE ICRA 2019 · May 2019 · conference
> **Keywords:** self-reconfigurable morphology, prismatic leg segments, open source quadruped, leg length as gearing, supply voltage, field tests
> **Links:** [Scholar](https://scholar.google.com/scholar?q=Self-Modifying+Morphology+Experiments+with+DyRET+Dynamic+Robot+for+Embodied+Testing) · [Code and hardware](https://github.com/dyret-robot)
> **One line:** the paper that introduces DyRET, an open-source quadruped whose femur and tibia lengths change by lead screws, and shows that the best leg length depends on available motor torque and terrain

## Summary

The hypothesis is simple: no single morphology is best for all situations, so a robot that can change its body should beat a fixed one. DyRET has four legs with three Dynamixel servos each plus two prismatic segments (femur and tibia) driven by small geared DC motors, chain, lead screw and rails, controlled by an Arduino board to 0.5 mm. Changing length is slow (about 1 mm/s), so it is used between gaits, not within a gait. The robot weighs 5.5 kg, costs about 6500 USD in parts, runs on ROS with Gazebo support, and is released as certified open-source hardware.

The key insight is that leg length works like a gear: longer legs need less joint rotation for the same foot motion (faster walking) but more torque. They test a short robot and a tall robot (80% of max length) with a hand-designed crawl gait, plus a faster "extended" gait only feasible for the tall robot. At 15 V the tall robot with extended gait is fastest (5.26 vs 3.26 m/min), at 10 V the short robot is best (2.90 vs 1.90 m/min). In the field the same pattern repeats: in an icy garage the tall robot wins, on an icy snowy footpath the short one wins. The evidence is small (few samples, two morphologies, speed only), but it is a clean demonstration and the platform is what the later Nature MI paper builds on.

## How it works

- Mammal-like quadruped, carbon fiber body, 3 MX-64 servos per leg plus 2 prismatic links (femur, tibia) with encoder, end switch and cable carriers
- Prismatic control: proportional controller per joint, lead screw holds load passively
- Software: ROS nodes for experiment manager, trajectory controller, gait controller, gait evaluator, same pipeline for Gazebo or hardware
- Gait: IK-based crawl gait from an 8-parameter cubic Hermite spline, ground height depends on leg lengths, side wag for static stability
- Lab test: 10 runs per pair walking 1.5 m forward and back, supply voltage 15 V vs 10 V to emulate battery depletion, Mann-Whitney U with Holm correction
- Field test: indoor garage on smooth concrete at +4°C and outdoor footpath with snow, ice and gravel at −5°C

## Results that matter

- 15 V: short base 3.25 m/min, tall base 3.14, tall extended 5.28 (best)
- 10 V: short base 2.70, tall base 1.88, tall extended 2.33, short is best
- Garage: tall extended best (≈2.46 m/min vs ≈1.8 for base gaits)
- Outdoor footpath: short best (≈1.31 m/min) vs tall base 0.99 and tall extended 0.83
- Field results have only 2 to 3 runs each, no significance tests

## Interesting stuff

- "Leg length as a mechanical gear" is a really useful way to see morphology change: it moves the robot along a speed vs torque trade-off
- Low torque (low voltage) and slippery terrain both favor short legs, which makes sense
- Only two morphologies and hand-tuned gaits, so the controller is not optimized per body
- Very slow reconfiguration, so this is body change between tasks, not during locomotion
- Open hardware is a big plus for the field

## Useful for me

- **As prior work.** The original DyRET paper, cite together with the Nature MI one. Simple and strong evidence that body length should depend on the condition.
- **Apply.** The speed vs torque gearing argument is a good motivation for a humanoid that changes limb length: shorter limbs for heavy loads or weak motors, longer for speed.
- **Apply.** Battery voltage drop is a nice real condition to test morphology adaptation, easy to emulate in simulation by scaling torque limits.
- **Gap.** No learned control and no transition during walking.

## Key prior work

- [[Real-World Evolution Adapts Robot Morphology and Control to Hardware Limitations|DyRET hardware limitations]] · their previous real-world evolution with this platform
- [[Morphological change in machines accelerates the evolution of robust behavior|Bongard morphological change]] · morphology change to help learning
- [[Evolutionary Developmental Robotics - Improving Morphology and Control of Physical Robots|Evo-devo robotics]] · physical morphology optimization
- [[Real-world evolution of robot morphologies - A proof of concept|Real-world morphology evolution]] · real-world morphology evolution with humans in the loop
- [[Modular self-reconfigurable robot systems (Grand challenges of robotics)|Modular self-reconfigurable robots]] · the modular alternative
- [[A flying robot with adaptive morphology for multi-modal locomotion|Adaptive morphology flying robot]] · discrete morphing between modes

## References

1. [[Comparing the effect of different spine and leg designs for a small bounding quadruped robot|Spine and leg designs]] · Eckert et al. · 2015 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Comparing+the+effect+of+different+spine+and+leg+designs+for+a+small+bounding+quadruped+robot)
2. [[Self-reconfigurable robots|Self-reconfigurable robots]] · Murata & Kurokawa · 2007 · IEEE RAM · [Scholar](https://scholar.google.com/scholar?q=Murata+Kurokawa+Self-reconfigurable+robots)
3. [[Embodied cognition is not what you think it is|Embodied cognition]] · Wilson & Golonka · 2013 · Frontiers in Psychology · [Scholar](https://scholar.google.com/scholar?q=Embodied+cognition+is+not+what+you+think+it+is)
4. [[Environmental influence on the evolution of morphological complexity in machines|Environment and morphological complexity]] · Auerbach & Bongard · 2014 · PLOS Comput. Biol. · [Scholar](https://scholar.google.com/scholar?q=Environmental+influence+on+the+evolution+of+morphological+complexity+in+machines)
5. [[Experimental dynamics of wing assisted running for a bipedal ornithopter|Bipedal ornithopter]] · Peterson & Fearing · 2011 · IROS · [Scholar](https://scholar.google.com/scholar?q=Experimental+dynamics+of+wing+assisted+running+for+a+bipedal+ornithopter)
6. [[Design of an improved land-air miniature robot|Land/air miniature robot]] · Kossett et al. · 2010 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+of+an+improved+land+air+miniature+robot)
7. [[A flying robot with adaptive morphology for multi-modal locomotion|Adaptive morphology flying robot]] · Daler et al. · 2013 · IROS · [Scholar](https://scholar.google.com/scholar?q=A+flying+robot+with+adaptive+morphology+for+multi-modal+locomotion)
8. [[Variable impedance actuators - A review|VIA review]] · Vanderborght et al. · 2013 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Variable+impedance+actuators+A+review)
9. [[Modular self-reconfigurable robot systems (Grand challenges of robotics)|Modular self-reconfigurable robots]] · Yim et al. · 2007 · IEEE RAM · [Scholar](https://scholar.google.com/scholar?q=Modular+self-reconfigurable+robot+systems+grand+challenges+of+robotics)
10. [[Molecubes extended - Diversifying capabilities of open-source modular robotics|Molecubes]] · Zykov et al. · 2008 · IROS Workshop · [Scholar](https://scholar.google.com/scholar?q=Molecubes+extended+Diversifying+capabilities+of+open-source+modular+robotics)
11. [[Selecting a meta-module to shape-change the ATRON self-reconfigurable robot|ATRON meta-module]] · Christensen & Stoy · 2006 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Selecting+a+meta-module+to+shape-change+the+atron+self-reconfigurable+robot)
12. [[Swarm-bot - from concept to implementation|Swarm-bot]] · Mondada et al. · 2003 · IROS · [Scholar](https://scholar.google.com/scholar?q=Swarm-bot+from+concept+to+implementation)
13. [[Overcoming initial convergence in multi-objective evolution of robot control and morphology using a two-phase approach|Two-phase evolution]] · Nygaard et al. · 2017 · EvoApplications · [Scholar](https://scholar.google.com/scholar?q=Overcoming+initial+convergence+in+multi-objective+evolution+of+robot+control+and+morphology+using+a+two-phase+approach)
14. [[Grand challenges for evolutionary robotics|ER grand challenges]] · Eiben · 2014 · Frontiers in Robotics and AI · [Scholar](https://scholar.google.com/scholar?q=Grand+challenges+for+evolutionary+robotics)
15. [[Real-world evolution of robot morphologies - A proof of concept|Real-world morphology evolution]] · Jelisavcic et al. · 2017 · Artificial Life · [Scholar](https://scholar.google.com/scholar?q=Real-world+evolution+of+robot+morphologies+A+proof+of+concept)
16. [[Evolutionary Developmental Robotics - Improving Morphology and Control of Physical Robots|Evo-devo robotics]] · Vujovic et al. · 2017 · Artificial Life · [Scholar](https://scholar.google.com/scholar?q=Evolutionary+developmental+robotics+Improving+morphology+and+control+of+physical+robots)
17. [[Morphological change in machines accelerates the evolution of robust behavior|Bongard morphological change]] · Bongard · 2011 · PNAS · [Scholar](https://scholar.google.com/scholar?q=Morphological+change+in+machines+accelerates+the+evolution+of+robust+behavior)
18. [[Real-World Evolution Adapts Robot Morphology and Control to Hardware Limitations|DyRET hardware limitations]] · Nygaard et al. · 2018 · GECCO · [Scholar](https://scholar.google.com/scholar?q=Real-world+evolution+adapts+robot+morphology+and+control+to+hardware+limitations)
19. [[DyRET GitHub repository|DyRET repo]] · Nygaard · n.d. · GitHub · [GitHub](https://github.com/dyret-robot/dyret_documentation)
20. [[ROS - an open-source Robot Operating System|ROS]] · Quigley et al. · 2009 · ICRA Workshop · [Scholar](https://scholar.google.com/scholar?q=ROS+an+open-source+robot+operating+system)
21. [[Design and use paradigms for Gazebo, an open-source multi-robot simulator|Gazebo]] · Koenig & Howard · 2004 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+use+paradigms+for+gazebo+an+open-source+multi-robot+simulator)
22. [[Combining MAP-Elites and incremental evolution to generate gaits for a mammalian quadruped robot|MAP-Elites quadruped gaits]] · Nordmoen et al. · 2018 · EvoApplications · [Scholar](https://scholar.google.com/scholar?q=Combining+map-elites+and+incremental+evolution+to+generate+gaits+for+a+mammalian+quadruped+robot)
23. [[Multi-objective evolution of fast and stable gaits on a physical quadruped robotic platform|Multi-objective gait evolution]] · Nygaard et al. · 2016 · SSCI · [Scholar](https://scholar.google.com/scholar?q=Multi-objective+evolution+of+fast+and+stable+gaits+on+a+physical+quadruped+robotic+platform)

## See also

- [[Real-world embodied AI through a morphologically adaptive quadruped robot|DyRET in the wild]] · the follow-up with continuous outdoor adaptation
- [[Multi-embodiment Legged Robot Control as a Sequence Modeling Problem|EAT]] · learned body-conditioned control for a quadruped with changing leg length
