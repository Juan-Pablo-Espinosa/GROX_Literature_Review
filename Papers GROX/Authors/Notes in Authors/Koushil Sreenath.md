---
type: author
name: "Koushil Sreenath"
aliases:
  - Prof. Koushil Sreenath
position: Associate Professor of Mechanical Engineering and Vice Chair of Graduate Studies, UC Berkeley
institutions:
  - "[[University of California, Berkeley]]"
labs:
  - "[[Hybrid Robotics]]"
countries:
  - USA
phd: "[[University of Michigan]]"
phd_year: 2011
phd_advisors:
  - "[[Jessy W. Grizzle]]"
msc: "[[University of Michigan]]"
postdoc: "[[University of Pennsylvania]]"
postdoc_advisors:
  - "[[Vijay Kumar]]"
homepage: https://hybrid-robotics.berkeley.edu/koushil
scholar: https://scholar.google.com/citations?view_op=search_authors&mauthors=Koushil+Sreenath
github: https://github.com/HybridRobotics
advisor_candidate: true
relevance: high
added: 2026-11-01
tags:
  - author
  - topic/humanoids
  - topic/legged-locomotion
  - topic/whole-body-control
  - topic/sim-to-real
  - method/reinforcement-learning
  - method/hybrid-zero-dynamics
  - method/control-barrier-functions
  - robot/cassie
  - robot/digit
---

# Koushil Sreenath
![[Pasted image 20261101160113.png|335]]
> [!info] Quick view
> **Now:** Associate Professor of Mechanical Engineering at [[University of California, Berkeley]] · head of [[Hybrid Robotics]]
> **Trained at:** PhD EECS [[University of Michigan]] 2011 ([[Jessy W. Grizzle]]) · Postdoc GRASP Lab, [[University of Pennsylvania]] ([[Vijay Kumar]])
> **Known for:** compliant hybrid zero dynamics on MABEL, RES-CLFs and exponential CBFs, RL on Cassie, Digit and G1 humanoid locomotion, BeyondMimic and SuperMimic
> **Links:** [Homepage](https://hybrid-robotics.berkeley.edu/koushil) · [Scholar](https://scholar.google.com/citations?view_op=search_authors&mauthors=Koushil+Sreenath) · [GitHub](https://github.com/HybridRobotics)
> **One line:** Started from provable nonlinear control for hybrid systems (bipeds and aerial robots), and now combines that structure with large-scale RL to get agile bipeds and humanoids on real hardware.

## Who he is

Koushil Sreenath is basically one of the few people who is serious both on the nonlinear control theory side and on the RL-on-real-robots side. He did his PhD in EECS at Michigan (2011) with Jessy Grizzle, working on hybrid zero dynamics for the MABEL biped, plus an MS in applied math there and an earlier MS in EE at UT Arlington. Then he was a postdoc at Penn's GRASP Lab with Vijay Kumar working on aerial manipulation, an assistant professor at CMU from 2013 to 2017, and since 2017 at Berkeley ME, where he became associate professor in 2021. Awards include NSF CAREER, the Hellman Fellowship, the Google Faculty Research Award, and best paper awards at RSS (2013) and L4DC.

## Research line

- **2007 to 2011 · Hybrid zero dynamics on compliant bipeds.** At Michigan with Grizzle he extended virtual constraints and HZD to MABEL, a biped with springs in the drivetrain. It walked at 1.5 m/s and set a kneed-biped running record of 3.06 m/s, all with model-based feedback control.
- **2011 to 2013 · Aerial manipulation.** At GRASP with Kumar he worked on quadrotors carrying cable-suspended loads, modeled as differentially-flat hybrid systems. His RSS 2013 paper on cooperative cable-suspended payloads comes from this line.
- **2013 to 2017 · Safety and stability with guarantees.** At CMU: rapidly exponentially stabilizing CLFs for HZD walking and exponential control barrier functions for high relative-degree safety constraints. These are the CLF/CBF tools a lot of robotics still uses today.
- **2017 to 2024 · RL on Cassie.** At Berkeley the lab moves to learning. First RL that imitates an HZD gait library, then end-to-end RL with long and short I/O history for walking, running (a 400 m dash) and jumping on real Cassie.
- **2023 to 2026 · Humanoids.** A transformer policy for zero-shot outdoor walking on Digit (with Malik and Darrell), BeyondMimic motion tracking with diffusion on G1 (with Karen Liu), and SuperMimic's differentiable world model for scaling whole-body control.

## Key works (start here)

- [[A Compliant Hybrid Zero Dynamics Controller for Stable, Efficient and Fast Bipedal Walking on MABEL|Compliant HZD on MABEL]] · 2011 · IJRR · [Link](https://hybrid-robotics.berkeley.edu/publications/IJRR2011.pdf) · the control-theory root, HZD that keeps the natural compliance of the robot
- [[Rapidly Exponentially Stabilizing Control Lyapunov Functions and Hybrid Zero Dynamics|RES-CLF]] · 2014 · IEEE TAC · [Link](https://hybrid-robotics.berkeley.edu/publications/TAC2014.pdf) · the CLF result behind CLF-QP walking controllers
- [[Dynamics, Control and Planning for Cooperative Manipulation of Payloads Suspended by Cables from Multiple Quadrotor Robots|Cooperative Cable Payloads]] · 2013 · RSS 2013 · [Link](https://www.roboticsproceedings.org/rss09/p11.html) · the aerial side, hybrid systems with switching cable tension
- [[Exponential Control Barrier Functions for Enforcing High Relative-Degree Safety-Critical Constraints|Exponential CBFs]] · 2016 · ACC 2016 · [Link](https://ieeexplore.ieee.org/document/7524935) · how to do CBF safety filters when the constraint has high relative degree
- [[Real-world humanoid locomotion with reinforcement learning|Digit Transformer]] · 2024 · Science Robotics · [arXiv](https://arxiv.org/abs/2303.03381) · causal transformer over observation-action history, zero-shot outdoor humanoid walking
- [[Reinforcement Learning for Versatile, Dynamic, and Robust Bipedal Locomotion Control|Versatile Cassie RL]] · 2025 · IJRR · [arXiv](https://arxiv.org/abs/2401.16889) · dual-history architecture plus task randomization, walking, running and jumping on one biped
- [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · 2026 · Science Robotics · [DOI](https://doi.org/10.1126/scirobotics.adx8924) · the open motion tracking recipe on G1 that a lot of people now build on
- [[SuperMimic - Scaling Humanoid Whole-Body Control with One Differentiable World Model|SuperMimic]] · 2026 · CoRL 2026 Oral · [arXiv](https://arxiv.org/abs/2610.28480) · one learned world model replaces cluster-scale PPO for humanoid tracking

## Useful for me

- **Control meets learning.** His lab is the clearest example of using model-based structure (HZD gait libraries, CLFs, CBFs) together with RL instead of picking one side. For a body that changes during the task, a model-based layer with guarantees during the transition plus an RL policy for the locomotion is a very natural split, and this is the lab that knows both tools.
- **Apply.** The dual-history encoder from the versatile Cassie paper adapts to time-variant dynamics shifts using the robot's own I/O history. A morphology change is kinda the most extreme time-variant shift, so this is a baseline to try before building explicit morphology conditioning.
- **As prior work.** BeyondMimic and SuperMimic are the current state of the art for humanoid motion tracking, and the tracking code is open (HybridRobotics GitHub). That's the stack to build on for whole-body control on a humanoid.
- **Gap.** I didn't find any work from his lab on bodies that change shape or on co-design. The fit would come through humanoid control and learning, and the morphology part would be new for the lab. That can be good (my own niche) or bad (no one there with that experience).

## Advisor fit

- **Where.** Department of Mechanical Engineering at UC Berkeley, where he is also Vice Chair of Graduate Studies. Berkeley EECS's Ember lab also lists him as a faculty lead next to Sastry, Malik and Tomlin, and its students are EECS PhDs, so there may be a path through both departments.
- **Real robots by default.** The key works all end on hardware: MABEL, quadrotors, Cassie, Digit and Unitree G1. He also has a strong theory side, so expect nonlinear control and stability analysis to be part of the training, not only RL.
- **Humanoids are the core right now.** BeyondMimic, SuperMimic and the Digit work are all humanoid. He also collaborates a lot across Berkeley (Abbeel, Levine, Malik, Darrell) and with Peng and Karen Liu.
- **How to apply.** The lab's Join Us page has a PhD section that only says "Coming soon," and the general instructions say there's no need to email them. So the path seems to be the normal Berkeley grad admissions (ME or EECS) and naming him as the advisor you want.
- **Open questions.** Does he admit PhD students through EECS or only ME? How many does he take per year? Is there room for a hardware-heavy morphology project, or is the lab mostly going toward learned controllers on off-the-shelf humanoids? What are his current students working on beyond motion tracking?

## Papers in my vault

> [!note] This list fills itself. Every paper note that lists Koushil Sreenath in `authors` shows up in this note's Backlinks.