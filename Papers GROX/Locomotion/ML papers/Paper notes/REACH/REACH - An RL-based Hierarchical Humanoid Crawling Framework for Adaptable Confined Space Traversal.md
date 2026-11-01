---
title: "REACH: An RL-based Hierarchical Humanoid Crawling Framework for Adaptable Confined Space Traversal"
aliases:
  - REACH
authors:
  - "[[Shengzhang Xu]]"
  - "[[Yuning Guo]]"
  - "[[Weiyi Hu]]"
  - "[[Yifeng Xu]]"
  - "[[Zhongyu Li]]"
  - "[[He Kong]]"
corresponding: ""
institutions:
  - "[[Southern University of Science and Technology]]"
countries:
  - China
year: 2026
date: 2026-10-27
venue: IEEE-RAS Humanoids 2026
venue_type: conference
arxiv_category: cs.RO
arxiv_id: "2610.36546"
url: https://arxiv.org/abs/2610.36546
pdf: https://arxiv.org/pdf/2610.36546
code: "Project page says code is linked there, repo not checked yet (2026-11-01)"
project_page: https://aislab-sustech.github.io/REACH/
status: to-read
relevance: medium
added: 2026-11-01
tags:
  - paper
  - topic/humanoids
  - topic/legged-locomotion
  - topic/confined-spaces
  - topic/multi-contact
  - topic/sim-to-real
  - topic/perception
  - method/reinforcement-learning
  - robot/unitree-g1
  - eval/real-robot
---

# REACH

> [!info] Quick view
> **Authors:** [[Shengzhang Xu]], [[Yuning Guo]], [[Weiyi Hu]], [[Yifeng Xu]], [[Zhongyu Li]], [[He Kong]]
> **From:** [[Southern University of Science and Technology]]
> **Published:** IEEE-RAS Humanoids 2026 · October 2026 · cs.RO
> **Keywords:** humanoid crawling, confined spaces, variable-height walking, mode transitions, latent adaptation, LiDAR planning, hierarchical control, Unitree G1
> **Links:** [arXiv](https://arxiv.org/abs/2610.36546) · [PDF](https://arxiv.org/pdf/2610.36546) · [Project](https://aislab-sustech.github.io/REACH/)
> **One line:** Two height-commanded RL policies (bipedal walking down to 0.50 m and crawling from 0.50 to 0.20 m), a transition policy between them and a LiDAR planner on top let an unmodified G1 get through passages as low as 40 cm.

## Summary

This paper try to solve something most humanoid locomotion work ignores. When the space gets lower than what crouched walking can handle, the robot has to go down and crawl. Most existing work only does height-adjustable bipedal walking, so REACH gives the G1 two command-conditioned modes: variable-height bipedal walking over 1.27 to 0.50 m, and crawling on hands and knees over 0.50 to 0.20 m. A bidirectional transition policy switches between them, and a LiDAR-based planner on top chooses the mode and height for the passage in front.

The idea is simple and practical. Treating crawling as a separate command-conditioned skill with its own height range, instead of forcing one policy to do everything, is basically the reason it works on hardware. The crawling policy uses a history-conditioned latent module inspired by DreamWaQ to deal with all the extra contacts. They claim good height and velocity tracking, reliable mode transitions and real confined-space traversal on the robot. I can't judge how strong the evidence is until I read the full results tables.

## How it works

- **Two command-conditioned skills.** A variable-height bipedal walking policy (body height command 1.27 to 0.50 m) and a crawling policy (0.50 to 0.20 m). The ranges meet at 0.50 m, so together they cover everything from standing to almost lying down.
- **Contact-rich crawling.** Crawling uses hands and knees as contacts (they call it quadrupedal crawling), with its own reward set, including an "overhead traversal suppression" term.
- **Latent adaptation.** A history-conditioned latent module in the style of DreamWaQ infers the hidden terrain and contact state from proprioceptive history, adapted for the many contacts in crawling.
- **Bidirectional transitions.** A separate transition policy handles walking to crawling and crawling back to walking.
- **Planner on top.** A LiDAR-based planner decides the mode and the height command, so the learned skills plug into a normal navigation stack.
- **Unmodified G1.** No extra hardware or knee pads, just the stock robot.

## Results that matter

> [!warning] Couldn't get the full paper text yet. Only the numbers from the abstract are here. Add the tables (tracking errors, success rates, ablations) from the PDF.

- Body height regulated over 1.27 to 0.50 m in bipedal mode and 0.50 to 0.20 m in crawling mode.
- Quadrupedal crawling gets through passages as low as 40 cm, which is below what crouched walking can reach.
- In real-world tests the authors report accurate height and velocity tracking, robust terrain traversal, reliable mode transitions and full confined-space traversal with the whole pipeline.

## Interesting stuff

- Splitting the height range between two skills that meet at 0.50 m is a clean design. Each policy only has to be good in its own range, and the transition policy is the only place where the contact topology changes.
- Crawling changes which body parts carry the load (feet, then hands and knees), so the problem is kinda a change of effective embodiment on the same hardware.
- Using a DreamWaQ-style latent for crawling makes sense, since contact-rich motion makes explicit state estimation much harder than for walking.
- **Open questions until I read the full text:** how reliable the transitions are (success rate over trials), whether they compare against a single policy that does both, and what the latent ablation shows.

## Useful for me

- **Apply.** A height command that crosses into a different contact mode is a good template for a body that reconfigures. A morphology change could be one more conditioned skill with its own range, plus a dedicated transition policy, instead of one huge policy.
- **As prior work.** The bidirectional transition policy between bipedal and quadrupedal contact modes is the closest thing to "switching between body configurations" on a real humanoid that I've found. Worth comparing with how we would handle the body changing mid-task.
- **Watch out.** It's a hierarchy of separate skills with a planner. That's robust but it doesn't scale to a continuous range of morphologies the way a single conditioned policy (like PolyMorph) might.
- **Gap.** The body itself never changes, only the posture and the contacts. A robot that can also change its limb length could maybe get through low passages without crawling at all. That is a different answer to the same confined-space problem.

## References

1. [[Humanoid Locomotion and Manipulation - Current Progress and Challenges in Control, Planning, and Learning|Humanoid Loco-Manip Survey]] · Gu et al. · 2026 · IEEE/ASME Transactions on Mechatronics · [Scholar](https://scholar.google.com/scholar?q=Humanoid+locomotion+and+manipulation+Current+progress+and+challenges+in+control+planning+and+learning)
2. [[Learning to walk in confined spaces using 3D representation|Confined Spaces 3D]] · Miki et al. · 2024 · ICRA 2024 · [arXiv](https://arxiv.org/abs/2403.00187)
3. [[Design of Crawling Motion for a Biped Walking Humanoid with 3-DoF Rigid-Flexible Waist|Humanoid Crawling Waist]] · Huang et al. · 2018 · Humanoids 2018 · [Scholar](https://scholar.google.com/scholar?q=Design+of+crawling+motion+for+a+biped+walking+humanoid+with+3-dof+rigid-flexible+waist)
4. [[Reinforcement Learning for Versatile, Dynamic, and Robust Bipedal Locomotion Control|Versatile Cassie RL]] · Li et al. · 2025 · IJRR · [arXiv](https://arxiv.org/abs/2401.16889)
5. [[Reinforcement Learning for Robust Parameterized Locomotion Control of Bipedal Robots|Parameterized Cassie RL]] · Li et al. · 2021 · ICRA 2021 · [arXiv](https://arxiv.org/abs/2103.14295)
6. [[Stubborn - A Streamlined and Unified Reinforcement Learning Framework for Robust Motion Tracking and Fall Recovery for Humanoids|Stubborn]] · Ren et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2606.12814)
7. [[Learning Humanoid Locomotion over Challenging Terrain|Humanoid Challenging Terrain]] · Radosavovic et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2410.03654)
8. [[Gait-Adaptive Perceptive Humanoid Locomotion with Real-Time Under-Base Terrain Reconstruction|Gait-Adaptive Perceptive]] · Song et al. · 2025 · IEEE RA-L · [arXiv](https://arxiv.org/abs/2512.07464)
9. [[Collision-Free Humanoid Traversal in Cluttered Indoor Scenes|HumanoidPF]] · Xue et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2601.16035)
10. [[Gallant - Voxel Grid-based Humanoid Locomotion and Local-navigation across 3D Constrained Terrains|Gallant]] · Ben et al. · 2026 · CVPR 2026 · [Scholar](https://scholar.google.com/scholar?q=Gallant+Voxel+grid-based+humanoid+locomotion+and+local-navigation+across+3d+constrained+terrains)
11. [[DreamWaQ - Learning Robust Quadrupedal Locomotion With Implicit Terrain Imagination via Deep Reinforcement Learning|DreamWaQ]] · Nahrendra et al. · 2023 · ICRA 2023 · [Scholar](https://scholar.google.com/scholar?q=DreamWaQ+Learning+robust+quadrupedal+locomotion+with+implicit+terrain+imagination+via+deep+reinforcement+learning)
12. [[EGO-Planner - An ESDF-free Gradient-based Local Planner for Quadrotors|EGO-Planner]] · Zhou et al. · 2021 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=EGO-Planner+An+ESDF-free+gradient-based+local+planner+for+quadrotors)
13. [[Hiking in the Wild - A Scalable Perceptive Parkour Framework for Humanoids|Hiking in the Wild]] · Zhu et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2601.07718)
14. [[RPL - Learning Robust Humanoid Perceptive Locomotion on Challenging Terrains|RPL]] · Zhang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.03002)
15. [[Learning Humanoid Locomotion with Perceptive Internal Model|Perceptive Internal Model]] · Long et al. · 2025 · ICRA 2025 · [arXiv](https://arxiv.org/abs/2411.14386)
16. [[Attention-Based Map Encoding for Learning Generalized Legged Locomotion|AME]] · He et al. · 2025 · Science Robotics · [arXiv](https://arxiv.org/abs/2506.09588)
17. [[AME-2 - Agile and Generalized Legged Locomotion via Attention-Based Neural Map Encoding|AME-2]] · Zhang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2601.08485)
18. [[GeoLoco - Leveraging 3D Geometric Priors from Visual Foundation Model for Robust RGB-Only Humanoid Locomotion|GeoLoco]] · Liu et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2603.07624)
19. [[Humanoid Whole-Body Locomotion on Narrow Terrain via Dynamic Balance and Reinforcement Learning|Narrow Terrain WBL]] · Xie et al. · 2025 · IROS 2025 · [Scholar](https://scholar.google.com/scholar?q=Humanoid+whole-body+locomotion+on+narrow+terrain+via+dynamic+balance+and+reinforcement+learning)
20. [[Walk the PLANC - Physics-Guided RL for Agile Humanoid Locomotion on Constrained Footholds|Walk the PLANC]] · Dai et al. · 2026 · IROS 2026 · [Scholar](https://scholar.google.com/scholar?q=Walk+the+PLANC+Physics-guided+RL+for+agile+humanoid+locomotion+on+constrained+footholds)
21. [[Mobius - A Multi-Modal Bipedal Robot that can Walk, Crawl, Climb, and Roll|Mobius]] · Schperberg et al. · 2026 · RSS 2026 · [Scholar](https://scholar.google.com/scholar?q=Mobius+A+multi-modal+bipedal+robot+that+can+walk+crawl+climb+and+roll)
22. [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · Liao et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2508.08241)
23. [[Kimodo - Scaling Controllable Human Motion Generation|Kimodo]] · Rempe et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2603.15546)
24. [[Low-profile Crawling for Humanoid Motion in Tight Spaces|Low-profile Crawling]] · Brooks et al. · 2015 · IROS 2015 · [Scholar](https://scholar.google.com/scholar?q=Low-profile+crawling+for+humanoid+motion+in+tight+spaces)
25. [[MASH - MAstering Safe and High-speed Quadruped Navigation with Adaptive Gait Transitions|MASH]] · Ma et al. · 2026 · IEEE Transactions on Field Robotics · [Scholar](https://scholar.google.com/scholar?q=MASH+Mastering+safe+and+high-speed+quadruped+navigation+with+adaptive+gait+transitions)
26. [[Kinematic and Kinetic Patterns in Human Gait - Variability and Compensating Effects|Human Gait Patterns]] · Winter · 1984 · Human Movement Science · [Scholar](https://scholar.google.com/scholar?q=Kinematic+and+kinetic+patterns+in+human+gait+Variability+and+compensating+effects)
27. [[Probabilistic Terrain Mapping for Mobile Robots with Uncertain Localization|Elevation Mapping]] · Fankhauser et al. · 2018 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Probabilistic+terrain+mapping+for+mobile+robots+with+uncertain+localization)
28. [[Point-LIO - Robust High-Bandwidth Light Detection and Ranging Inertial Odometry|Point-LIO]] · Zheng et al. · 2022 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Point-LIO+Robust+high-bandwidth+light+detection+and+ranging+inertial+odometry)
29. [[Isaac Lab - A GPU-Accelerated Simulation Framework for Multi-Modal Robot Learning|Isaac Lab]] · Mittal et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2511.04831)
30. [[MuJoCo - A physics engine for model-based control|MuJoCo]] · Todorov et al. · 2012 · IROS 2012 · [Scholar](https://scholar.google.com/scholar?q=MuJoCo+A+physics+engine+for+model-based+control)
## See also

These papers are close to REACH, but I couldn't check whether it cites them. If they show up in the bibliography, move them to References.

- [[Locomotion Beyond Feet|Locomotion Beyond Feet]] · keyframes plus RL for crawling, climbing and transitions on a small humanoid · [arXiv](https://arxiv.org/abs/2601.03607)
- [[APEX - Learning Adaptive High-Platform Traversal for Humanoid Robots|APEX]] · multi-skill climbing, crawling and stand-up or lie-down transitions on G1 · [arXiv](https://arxiv.org/abs/2602.11143)
- [[Collision-Free Humanoid Traversal in Cluttered Indoor Scenes|HumanoidPF]] · one generalist policy for crouching and squeezing through clutter · [arXiv](https://arxiv.org/abs/2601.16035)
- [[Whole-Body Planning for Humanoids Navigating Confined Spaces via Self-Collision Avoidance References|Confined-Space WBP]] · the planning-based alternative on G1 · [arXiv](https://arxiv.org/abs/2608.10220)
- [[Dexterous Legged Locomotion in Confined 3D Spaces with Reinforcement Learning|Confined 3D Quadruped]] · hierarchical planner plus RL for quadrupeds in confined spaces · [arXiv](https://arxiv.org/abs/2403.03848)