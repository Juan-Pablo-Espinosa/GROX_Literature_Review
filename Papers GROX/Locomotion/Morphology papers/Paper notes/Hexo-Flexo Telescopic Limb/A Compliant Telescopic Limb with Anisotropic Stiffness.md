---
title: "A Compliant Telescopic Limb with Anisotropic Stiffness"
aliases:
  - Hexo-Flexo Telescopic Limb
  - Hexo-Flexo bot
authors:
  - "[[Aaron Fishman]]"
  - "[[Martin Stephen Garrad]]"
  - "[[Andrew Hinitt]]"
  - "[[Plinio Zanini]]"
  - "[[Tim Barker]]"
  - "[[Jonathan Rossiter]]"
corresponding: "[[Aaron Fishman]]"
institutions:
  - "[[University of Bristol]]"
  - "[[University of the West of England]]"
labs:
  - "[[Bristol Robotics Laboratory]]"
countries:
  - UK
year: 2017
date: 2017-02-01
venue: Frontiers in Robotics and AI 2017 (Vol. 3, Article 80)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.3389/frobt.2016.00080
pdf: https://www.frontiersin.org/articles/10.3389/frobt.2016.00080/pdf
code: "No official repo (hardware paper, checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/morphology
  - topic/reconfigurable-robots
  - topic/variable-length-limbs
  - topic/variable-stiffness
  - topic/soft-robotics
  - topic/hardware-design
  - topic/actuators
  - topic/legged-locomotion
  - method/pneumatic-actuation
  - eval/real-robot
---

# Hexo-Flexo Telescopic Limb

> [!info] Quick view
> **Authors:** [[Aaron Fishman]] (corresponding), [[Martin Stephen Garrad]], [[Andrew Hinitt]], [[Plinio Zanini]], [[Tim Barker]], [[Jonathan Rossiter]]
> **From:** [[University of Bristol]] · [[Bristol Robotics Laboratory]] · [[University of the West of England]]
> **Published:** Frontiers in Robotics and AI · Feb 2017 · Soft Robotics section
> **Keywords:** variable length limb, telescopic endoskeleton, anisotropic stiffness, pneumatic bellow, buckling, hexapod, RoboSoft challenge
> **Links:** [DOI](https://doi.org/10.3389/frobt.2016.00080) · [PDF](https://www.frontiersin.org/articles/10.3389/frobt.2016.00080/pdf) · [Data](http://dx.doi.org/10.5523/bris.l9g38ayyb4nm15x3sotqrlf2p)
> **One line:** a rubber bellow with a rigid 3D-printed telescope inside gives a pneumatic limb that can change its length but still does not buckle under load

## Summary

The paper try to solve a classic soft robotics problem: soft limbs are safe and adapt to the ground, but they buckle when you push on them axially, so they cannot carry a robot. They put a rigid telescopic endoskeleton (three concentric PLA stages) inside a car steering rack bellow. The bellow gives the soft outside and the pneumatic length control, and the telescope gives axial and radial stiffness. With vacuum and pressure pumps plus two solenoid valves they can shrink or grow the limb by about 30% in each direction.

The result is a limb that is 5 to 13x stiffer radially and 4.5 to 11x stiffer axially than the bare bellow, and it stops buckling. They build a six-legged walker (Hexo-Flexo bot) for the RoboSoft challenge, but the body fractured and the robot never competed, so there is no locomotion result at all. Honestly this is a short design paper, the value for me is the idea of a variable length limb that keeps a rigid load path, not the numbers.

## How it works

- Limb module = commercial bellow (stretch ratio 3.4, natural length 150 mm) sealed with varnished PLA plugs
- Inside there is a telescope of concentric cylinders, the number of stages is set by the max diameter, wall thickness and gap between stages
- The gap g decide when the telescope starts to stiffen radially, so you can tune how "loose" the limb is
- Length control is pneumatic, one pump for expansion, one for compression, one valve to switch pumps and one to vent
- Each leg of the hexapod uses two limb modules joined by a knee, plus two hobby servos at the hip (lift and swing)
- Power and air come through a tether, one ATmega328P controls valves and servos

## Results that matter

- Radial stiffness goes up 13x compressed (0.13 to 1.7 N/mm), 8x at atmospheric, about 9x expanded
- Axial stiffness goes up 4.5x compressed, 11x atmospheric, 6.4x expanded, enough to prevent buckling
- Stiffness drops as the limb extends, the telescope gets more compliant when it is longer
- Length range about 120 to 220 mm, but compression takes around 40 s and recovery about 60 s, so it is very slow
- The endoskeleton adds 58 g (doubles the module mass) and reduces the expansion ratio from 3.4 to 2.1
- The hexapod never walked in the competition because the main body broke

## Interesting stuff

- The anisotropic stiffness idea is nice, stiff along the load path but still soft to small perturbations, kinda like a bone inside muscle
- The tradeoff is clear and general: more telescope stages give more stroke but each stage is weaker
- Stiffness decreasing with extension is exactly the problem any telescopic leg will have, longer means more bending compliance
- No locomotion data, no controller, and response time in tens of seconds, so it is a concept more than a working legged robot
- The interesting part is the challenge rule itself, RoboSoft rewarded robots that can change size, which is the same motivation as morphology adaptation

## Useful for me

- **As prior work.** Early example of a legged robot built around limbs that change length, good to cite in the related work about variable length limbs and size changing robots.
- **Watch out.** Any telescopic limb loses stiffness when extended, so a policy trained in simulation with rigid links will see a different body at long extensions. That compliance should be modeled or randomized.
- **Apply.** The anisotropic idea, rigid axial load path with compliance in the other directions, is a useful design principle when thinking about how a growing limb takes impacts.
- **Gap.** Slow pneumatic actuation means the length can only change between tasks, not during a gait. Fast length change during locomotion is still open here.

## Key prior work

- [[Stiff-flop surgical manipulator - Mechanical design and experimental characterization of the single module|STIFF-FLOP]] · the reference for soft modules that switch stiffness
- [[A hybrid combining hard and soft robots|Hard-soft hybrid]] · the idea of mixing rigid and soft materials that this limb follows
- [[Universal robotic gripper based on the jamming of granular material|Jamming gripper]] · the classic stiffness switching mechanism they compare against
- [[RHex - A simple and highly mobile hexapod robot|RHex]] · the hexapod reference they mention for future whegs version
- [[The challenges ahead for bio-inspired soft robotics|Soft robotics challenges]] · the motivation for soft but capable bodies
- [[The ARL monopod II running robot - control and energetics|ARL Monopod II]] · hopping with a prismatic compliant leg, possible future use of the limb

## References

1. [[The ARL monopod II running robot - control and energetics|ARL Monopod II]] · Ahmadi & Buehler · 1999 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+ARL+monopod+II+running+robot+control+and+energetics)
2. [[Universal robotic gripper based on the jamming of granular material|Jamming gripper]] · Brown et al. · 2010 · PNAS · [DOI](https://doi.org/10.1073/pnas.1003250107)
3. [[Stiff-flop surgical manipulator - Mechanical design and experimental characterization of the single module|STIFF-FLOP]] · Cianchetti et al. · 2013 · IROS · [Scholar](https://scholar.google.com/scholar?q=Stiff-flop+surgical+manipulator+mechanical+design+and+experimental+characterization+of+the+single+module)
4. [[Mechanically programmable bend radius for fiber-reinforced soft actuators|Programmable bend radius]] · Galloway et al. · 2013 · ICAR · [Scholar](https://scholar.google.com/scholar?q=Mechanically+programmable+bend+radius+for+fiber-reinforced+soft+actuators)
5. [[Fluid Actuated Motor System and Stroking Device|McKibben patent]] · Gaylord · 1958 · US Patent 2,844,126 · [Scholar](https://scholar.google.com/scholar?q=Fluid+Actuated+Motor+System+and+Stroking+Device)
6. [[A soft jellyfish robot driven by a dielectric elastomer actuator|Jellyfish robot]] · Godaba et al. · 2016 · IEEE RA-L · [DOI](https://doi.org/10.1109/LRA.2016.2522498)
7. [[Soft robotics for chemists|Soft robotics for chemists]] · Ilievski et al. · 2011 · Angewandte Chemie · [DOI](https://doi.org/10.1002/anie.201006464)
8. [[Soft robotics - a bioinspired evolution in robotics|Soft robotics evolution]] · Kim et al. · 2013 · Trends in Biotechnology · [DOI](https://doi.org/10.1016/j.tibtech.2013.03.002)
9. [[GoQBot - a caterpillar-inspired soft-bodied rolling robot|GoQBot]] · Lin et al. · 2011 · Bioinspiration & Biomimetics · [DOI](https://doi.org/10.1088/1748-3182/6/2/026007)
10. [[Caterpillars use the substrate as their external skeleton - a behavior confirmation|Caterpillar substrate skeleton]] · Lin & Trimmer · 2010 · Communicative & Integrative Biology · [DOI](https://doi.org/10.4161/cib.3.5.12560)
11. [[The challenges ahead for bio-inspired soft robotics|Soft robotics challenges]] · Pfeifer et al. · 2012 · Communications of the ACM · [DOI](https://doi.org/10.1145/2366316.2366335)
12. [[RHex - A simple and highly mobile hexapod robot|RHex]] · Saranli et al. · 2001 · IJRR · [DOI](https://doi.org/10.1177/02783640122067570)
13. [[Meshworm - a peristaltic soft robot with antagonistic nickel titanium coil actuators|Meshworm]] · Seok et al. · 2013 · IEEE/ASME TMECH · [DOI](https://doi.org/10.1109/TMECH.2012.2204070)
14. [[A hybrid combining hard and soft robots|Hard-soft hybrid]] · Stokes et al. · 2014 · Soft Robotics · [DOI](https://doi.org/10.1089/soro.2013.0002)
15. [[McKibben artificial muscle using shape-memory polymer|SMP McKibben]] · Takashima et al. · 2010 · Sensors and Actuators A · [DOI](https://doi.org/10.1016/j.sna.2010.09.010)
16. [[A journal of soft robotics - why now|Soft Robotics journal editorial]] · Trimmer · 2014 · Soft Robotics · [DOI](https://doi.org/10.1089/soro.2013.0003)
17. [[Soft robotics - Biological inspiration, state of the art, and future research|Soft robotics review]] · Trivedi et al. · 2008 · Applied Bionics and Biomechanics · [DOI](https://doi.org/10.1155/2008/520417)

## See also

- [[Real-world embodied AI through a morphologically adaptive quadruped robot|DyRET in the wild]] · a legged robot that actually changes leg length during deployment
