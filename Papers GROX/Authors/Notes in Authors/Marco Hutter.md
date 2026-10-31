---
type: author
name: "Marco Hutter"
aliases:
  - Prof. Marco Hutter
position: Professor of Robotic Systems and Head of RSL at ETH Zurich · Director of the ETH Center for Robotics · Director of the RAI Institute Zurich office
institutions:
  - "[[ETH Zurich]]"
  - "[[Robotics and AI Institute]]"
labs:
  - "[[Robotic Systems Lab]]"
cofounded:
  - "[[ANYbotics]]"
  - "[[Gravis Robotics]]"
countries:
  - Switzerland
phd: "[[ETH Zurich]]"
phd_year: 2013
phd_advisors:
  - "[[Roland Siegwart]]"
msc: "[[ETH Zurich]]"
msc_advisors:
  - "[[Roland Siegwart]]"
homepage: https://rsl.ethz.ch
scholar: https://scholar.google.com/citations?view_op=search_authors&mauthors=Marco+Hutter
github: https://github.com/leggedrobotics
advisor_candidate: true
relevance: high
added: 2026-10-31
tags:
  - author
  - topic/legged-locomotion
  - topic/sim-to-real
  - topic/domain-randomization
  - topic/perception
  - topic/actuators
  - topic/hardware-design
  - method/reinforcement-learning
  - method/teacher-student
  - sim/isaac-lab
---

# Marco Hutter
![[Pasted image 20261031141619.png]]
> [!info] Quick view
> **Now:** Professor of Robotic Systems, head of the [[Robotic Systems Lab]] at [[ETH Zurich]] · Director of the [[Robotics and AI Institute]] Zurich office
> **Trained at:** PhD [[ETH Zurich]] 2013 ([[Roland Siegwart]]) · MSc [[ETH Zurich]] 2009 ([[Roland Siegwart]])
> **Known for:** ANYmal, actuator networks, teacher-student legged RL, perceptive locomotion in the wild, legged_gym and Orbit
> **Links:** [RSL](https://rsl.ethz.ch) · [Scholar](https://scholar.google.com/citations?view_op=search_authors&mauthors=Marco+Hutter) · [GitHub](https://github.com/leggedrobotics)
> **One line:** Build rugged torque-controlled legged robots, then train their controllers in massively parallel simulation and deploy them in the real world.

## Who he is

Marco Hutter is basically the person behind ANYmal, and behind the idea that learned legged policies can really go to the field and not only to a demo video. He did all his degrees in mechanical engineering at ETH Zurich. His master thesis and his PhD (2013) were with Roland Siegwart, and the thesis was about StarlETH and compliant actuation. He became assistant professor in 2015 and started the Robotic Systems Lab. Today he is a full professor, director of the ETH Center for Robotics, and since 2024 also director of the RAI Institute's Zurich office. He co-founded ANYbotics, Gravis Robotics and other ETH spin-offs. Awards include an ERC Starting Grant and the IEEE RAS Early Academic Career Award (both 2019), the ETH Medal for his thesis, and the 2024 Rössler Prize.

## Research line

- **2009 to 2015 · Compliant hardware and model-based control.** The ScarlETH leg, then the StarlETH quadruped with series elastic actuators, controlled with hierarchical operational space control. Very mechanical, torque control first.
- **2016 to 2018 · ANYmal and going to the field.** ANYmal as a rugged, torque-controllable platform for real applications like oil and gas inspection (ARGOS challenge). ANYbotics started to commercialize it.
- **2019 to 2022 · RL sim-to-real becomes the standard.** Hwangbo's actuator network, Lee's teacher-student blind locomotion, Miki's perceptive locomotion. All in Science Robotics, all on a real robot outdoors.
- **2021 to 2024 · Massively parallel sim and tooling.** legged_gym with NVIDIA (walking in minutes on one GPU), then Orbit, which became Isaac Lab, then parkour and agile navigation on ANYmal.
- **2024 to 2026 · Beyond quadrupeds.** Legged mobile manipulation, wheeled-legged robots, autonomous excavators, and more and more humanoid work, plus leading RAI Institute Zurich.

## Key works (start here)

- [[ANYmal - a highly mobile and dynamic quadrupedal robot|ANYmal]] · 2016 · IROS 2016 · [Link](https://ieeexplore.ieee.org/document/7758092) · the platform paper, compliant joint modules and why the robot was built for the real world
- [[Learning agile and dynamic motor skills for legged robots|Agile Motor Skills]] · 2019 · Science Robotics · [arXiv](https://arxiv.org/abs/1901.08652) · actuator network plus sim training, the paper that showed RL locomotion transfers to a real ANYmal
- [[Learning Quadrupedal Locomotion over Challenging Terrain|Challenging Terrain]] · 2020 · Science Robotics · [arXiv](https://arxiv.org/abs/2010.11251) · teacher-student with privileged info and terrain curriculum, blind policy that works in nature
- [[Learning to Walk in Minutes Using Massively Parallel Deep Reinforcement Learning|legged_gym]] · 2021 · CoRL 2021 · [arXiv](https://arxiv.org/abs/2109.11978) · thousands of robots on one GPU, flat terrain policy in under four minutes
- [[Learning robust perceptive locomotion for quadrupedal robots in the wild|Perceptive Locomotion]] · 2022 · Science Robotics · [arXiv](https://arxiv.org/abs/2201.08117) · fuses height map and proprioception with an attention-based recurrent encoder, falls back to blind walking when the perception is bad
- [[Orbit - A Unified Simulation Framework for Interactive Robot Learning Environments|Orbit]] · 2023 · IEEE RA-L · [arXiv](https://arxiv.org/abs/2301.04195) · the framework that turned into Isaac Lab
- [[ANYmal Parkour - Learning Agile Navigation for Quadrupedal Robots|ANYmal Parkour]] · 2024 · Science Robotics · [arXiv](https://arxiv.org/abs/2306.14874) · hierarchical skills plus a navigation policy, all trained in sim, up to 2 m/s on hardware

## Useful for me

- **Gold standard.** The 2019 to 2022 Science Robotics line is the reference recipe for deploying learned locomotion: actuator model, privileged teacher distilled into a student, terrain curriculum, heavy randomization. If our policy dont transfer, this is the checklist to compare against.
- **Apply.** legged_gym, Orbit, Isaac Lab and the rsl_rl library come from this lab, so a big part of the humanoid RL stack used today descends from RSL. Reading their papers is kinda reading the design decisions behind the tools.
- **Morphology.** When the body changes, the dynamics the policy sees change too. Their actuator modeling and randomization work is the base for thinking about what a morphology-aware policy needs to observe and what we can just randomize.
- **Gap.** Almost everything is quadrupeds with a fixed body. The humanoid work is newer, and a body that changes during the task isnt really their focus, so that is a space where we bring something they dont cover.
- **Co-design mindset.** He keeps saying hardware and software have to be developed and tested together. That matches our idea of designing the body and the controller as one problem.

## Advisor fit

- **Where.** RSL is in the Department of Mechanical and Process Engineering (D-MAVT) at ETH Zurich. So the PhD is in mechanical engineering and very hardware heavy, the opposite side of Peng's CS and graphics lab.
- **Real robots by default.** Nearly every flagship paper ends on ANYmal hardware in the field (Alps hikes, mines, offshore sites). Sim-only papers are the exception here.
- **Humanoids are growing.** RSL has research engineers working on humanoid platforms, and recent humanoid motion-retargeting and dataset work has Hutter as coauthor. The RAI Institute (Zurich office since 2024) also shows RL humanoid acrobatics.
- **Founder culture.** His group has produced several ETH spin-offs (ANYbotics, Gravis Robotics). Good fit with the startup side.
- **How you get in.** An ETH doctorate starts with a professor accepting you into a position, not with a central admissions cycle like in the US. I didn't find a current RSL PhD openings page, so check rsl.ethz.ch directly.
- **Open questions.** Does RSL take PhD students straight from a US bachelor's, or mostly after an ETH master's? With a lab this big, which senior scientist or postdoc would supervise humanoid work day to day? Does the humanoid research happen at RSL or mostly at RAI? How is humanoid work funded (ERC, industry, NCCR)?

## Papers in my vault

> [!note] This list fills itself. Every paper note that lists Marco Hutter in `authors` shows up in this note's Backlinks.