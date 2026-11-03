---
type: author
name: "Deepak Pathak"
aliases:
  - Prof. Deepak Pathak
position: Raj Reddy Associate Professor of Robotics, Robotics Institute, Carnegie Mellon University (reported on leave) · Co-founder and CEO, Skild AI
institutions:
  - "[[Carnegie Mellon University]]"
  - "[[Skild AI]]"
labs:
  - "[[Pathak Research Group]]"
countries:
  - USA
phd: "[[University of California, Berkeley]]"
phd_year: 2019
phd_advisors:
  - "[[Alexei A. Efros]]"
  - "[[Trevor Darrell]]"
bachelor: "[[Indian Institute of Technology Kanpur]]"
postdoc: "[[University of California, Berkeley]]"
homepage: https://www.cs.cmu.edu/~dpathak/
scholar: ""
github: https://github.com/pathak22
advisor_candidate: true
relevance: high
added: 2026-11-03
tags:
  - author
  - topic/legged-locomotion
  - topic/sim-to-real
  - topic/morphology
  - topic/foundation-models
  - topic/dexterous-hands
  - topic/loco-manipulation
  - method/reinforcement-learning
  - method/teacher-student
  - robot/unitree-a1
---

# Deepak Pathak
![[Pasted image 20261103105320.png]]

> [!info] Quick view
> **Now:** Raj Reddy Associate Professor at the Robotics Institute, [[Carnegie Mellon University]] (reported on leave) · head of [[Pathak Research Group]] · Co-founder and CEO of [[Skild AI]]
> **Trained at:** PhD [[University of California, Berkeley]] 2019 ([[Alexei A. Efros]], [[Trevor Darrell]]) · researcher at Meta AI Research with [[Jitendra Malik]] and visiting postdoc at Berkeley with [[Pieter Abbeel]] · BTech [[Indian Institute of Technology Kanpur]] 2014
> **Known for:** curiosity-driven exploration (ICM), self-assembling modular agents, RMA, egocentric-vision locomotion and extreme parkour, LEAP Hand, Skild Brain
> **Links:** [Homepage](https://www.cs.cmu.edu/~dpathak/) · [GitHub](https://github.com/pathak22) · [Scholar search](https://scholar.google.com/scholar?q=Deepak+Pathak)
> **One line:** From self-supervised curiosity to cheap robots that learn agile skills in sim and transfer zero-shot, and now one "robot brain" foundation model for any body.

## Who they are

Deepak Pathak did a BTech in CSE at IIT Kanpur (2014, Institute Gold Medal) and then a PhD at Berkeley (2019) with Alyosha Efros and Trevor Darrell, so the root is computer vision and self-supervised learning, not control. After that, a year at Meta AI Research with Jitendra Malik and a visiting postdoc with Pieter Abbeel, and in 2020 they joined the CMU Robotics Institute. The awards are many: Sloan Fellowship 2025, MIT Tech Review 35 under 35 in 2024, ONR Young Investigator and PAMI Young Researcher in 2026. In 2023 they co-founded Skild AI with Abhinav Gupta to build a general robot foundation model (Skild Brain). Skild raised 1.4B at a 14B valuation in January 2026, and several sources say both founders are on leave from CMU.

## Research line

- **2016 to 2019 · Self-supervised vision and curiosity.** Context Encoders, then the Intrinsic Curiosity Module: reward is the prediction error in a learned feature space. This is still the most cited idea from them.
- **2019 · Morphology and modularity.** Self-assembling agents where limbs link together and the policy is a modular graph that mirrors the body. Honestly this is the paper closest to my topic in their whole line.
- **2021 to 2022 · Sim-to-real legged locomotion with Malik.** RMA learns an adaptation module that estimates an extrinsics latent from proprioception history, so the policy adapts online to terrain and payload without fine-tuning. Then egocentric depth policies for stairs and gaps, and Deep Whole-Body Control for a legged manipulator.
- **2023 to 2024 · Agility and cheap hardware.** Extreme parkour on a low-cost A1 with one depth camera, LEAP Hand as a low-cost dexterous hand for learning, plus learning from human videos.
- **2023 to now · Robot brains at Skild AI.** One foundation model that generalizes across tasks and robot embodiments. Little of this is published as papers, so it's hard to follow the details.

## Key works (start here)

- [[Curiosity-driven Exploration by Self-supervised Prediction|Curiosity ICM]] · 2017 · ICML 2017 · [arXiv](https://arxiv.org/abs/1705.05363) · intrinsic reward from prediction error, the classic exploration paper
- [[Learning to Control Self-Assembling Morphologies - A Study of Generalization via Modularity|Self-assembling morphologies]] · 2019 · NeurIPS 2019 · [arXiv](https://arxiv.org/abs/1902.05546) · policy that follows a body which builds itself, read this first for my work
- [[RMA - Rapid Motor Adaptation for Legged Robots|RMA]] · 2021 · RSS 2021 · [arXiv](https://arxiv.org/abs/2107.04034) · base policy plus adaptation module, the standard way to adapt online to unknown dynamics
- [[Legged Locomotion in Challenging Terrains using Egocentric Vision|Egocentric vision locomotion]] · 2022 · CoRL 2022 · [arXiv](https://arxiv.org/abs/2211.07638) · RL with cheap scandots then distill to depth, a clean teacher-student recipe
- [[Deep Whole-Body Control - Learning a Unified Policy for Manipulation and Locomotion|Deep Whole-Body Control]] · 2022 · CoRL 2022 · [arXiv](https://arxiv.org/abs/2210.10044) · one policy for legs and arm with advantage mixing and regularized online adaptation
- [[Extreme Parkour with Legged Robots|Extreme Parkour]] · 2023 · ICRA 2024 · [arXiv](https://arxiv.org/abs/2309.14341) · one depth camera and a cheap quadruped jumping gaps of twice its size
- [[LEAP Hand - Low-Cost, Efficient, and Anthropomorphic Hand for Robot Learning|LEAP Hand]] · 2023 · RSS 2023 · [arXiv](https://arxiv.org/abs/2309.06440) · shows their taste for cheap hardware made for learning

## Useful for me

- **As prior work.** RMA is basically the reference for online adaptation from proprioception. A humanoid that changes its limbs has dynamics that change during the task, and an RMA-style latent that tracks the current body is the obvious baseline.
- **As prior work.** The self-assembling morphologies paper is one of the few that trains policies for bodies that change their structure, and it shows modular policies generalize better to new bodies than monolithic ones.
- **Apply.** The scandots-to-depth distillation and the advantage mixing trick for coupled action spaces both transfer directly to whole-body control of a reconfigurable humanoid.
- **Gap.** Their legged work is mostly quadrupeds with a fixed body, and the cross-embodiment work at Skild is not published in detail. A real humanoid that changes morphology during the task is not covered.

## Advisor fit

- **Where.** Robotics Institute, School of Computer Science at CMU, Pittsburgh, same department as [[Guanya Shi]], so co-advising inside RI is at least structurally possible.
- **Real robots by default.** The legged and hand papers all end on real, low-cost hardware with sim-to-real transfer and no fine-tuning.
- **Strong overlap with my topic.** Morphology (self-assembling agents), online adaptation (RMA) and cross-embodiment robot brains (Skild) are all near a humanoid that changes its body.
- **Big caveat.** Third-party sources say they are on leave from CMU to be CEO of Skild AI. I couldn't find a prospective-students page or any statement that they are taking new PhD students.
- **Open questions.** Are they actually advising new RI PhD students in 2027, or only finishing current ones? Is any student co-advised with Guanya Shi right now? Would a CMU PhD with them have a link to Skild (internships, data, robots)? Check the homepage and ask current group students before emailing.

## Papers in my vault

> [!note] This list fills itself. Every paper note that lists Deepak Pathak in `authors` shows up in this note's Backlinks.
