---
title: "Multi-Embodiment Locomotion at Scale with extreme Embodiment Randomization"
aliases:
  - URMAv2
  - Multi-Embodiment Locomotion at Scale
authors:
  - "[[Nico Bohlinger]]"
  - "[[Jan Peters]]"
corresponding: ""
institutions:
  - "[[Technical University of Darmstadt]]"
  - "[[hessian.AI]]"
  - "[[German Research Center for Artificial Intelligence]]"
labs:
  - "[[Intelligent Autonomous Systems Lab]]"
  - "[[Robotics Institute Germany]]"
countries:
  - Germany
year: 2025
date: ""
venue: Humanoids 2025 Workshop on Sim-to-Real Transfer for Humanoid Robots (arXiv preprint)
venue_type: workshop
arxiv_category: cs.RO
arxiv_id: "2509.02815"
url: https://arxiv.org/abs/2509.02815
pdf: https://arxiv.org/pdf/2509.02815
code: "No official repo for this paper found (checked 2026-11-02)"
related_code: https://github.com/search?q=RL-X+Bohlinger&type=repositories
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/legged-locomotion
  - topic/humanoids
  - topic/cross-embodiment
  - topic/morphology
  - topic/domain-randomization
  - topic/sim-to-real
  - method/reinforcement-learning
  - method/ppo
  - method/curriculum-learning
  - method/transformer
  - sim/mujoco
  - robot/unitree-h1
  - robot/unitree-go2
  - eval/real-robot
---

# URMAv2

> [!info] Quick view
> **Authors:** [[Nico Bohlinger]], [[Jan Peters]]
> **From:** [[Technical University of Darmstadt]] · [[Intelligent Autonomous Systems Lab]] · [[hessian.AI]] · [[German Research Center for Artificial Intelligence]] · [[Robotics Institute Germany]]
> **Published:** Humanoids 2025 workshop paper · arXiv Sep 2025 · cs.RO
> **Keywords:** URMA, embodiment randomization, 50 legged robots, attention joint encoder, performance-based curriculum, MJX, zero-shot transfer
> **Links:** [arXiv](https://arxiv.org/abs/2509.02815) · [PDF](https://arxiv.org/pdf/2509.02815) · [RL-X library (not this paper's code)](https://github.com/search?q=RL-X+Bohlinger&type=repositories)
> **One line:** one locomotion policy trained on 50 legged robots plus up to 10 million randomized variations of them, with a single curriculum coefficient, transferring zero-shot to unseen quadrupeds and a humanoid

## Summary

This is a short scaling paper on top of URMA. The architecture takes per-joint description vectors (axis, limits, gains...) as attention keys and per-joint observations as values, so any number of joints maps into one fixed latent. URMAv2 makes the encoder and core bigger (5 layers, WeightNorm), adds a per-joint flag for "track nominal or free", removes foot contact inputs, and replaces the universal decoder by a simple dot product between the action latent and each joint's attention weights. The bigger change is extreme embodiment randomization: body sizes, masses, inertias, joint axes, limits, gains, damping and more are all randomized as a percentage of the URDF values and shown to the policy through the description vectors, and the embodiment is resampled with 0.2% probability every step, so about every 10 s even inside an episode.

Everything (DR ranges, ER ranges, pushes, terrain, terminations, penalties) is tied to one curriculum value β that goes up after successful episodes. Trained with PPO in MJX on 50 robots (15 quadrupeds, 23 humanoids, 8 bipeds, 4 hexapods), 25600 envs, 5B steps, about 40 h on one A100. URMAv2 reaches β = 0.88 vs 0.82 for URMA, and much more than zero-padding or multi-head baselines. Leave-one-out zero-shot works well for Go2 and the MAB Silver Badger (with a spine joint) and clearly worse for humanoids. On hardware, Go2, Badger work well, Booster T1 walks forward and sideways but falls turning, and H1 only works with the policy that saw H1 in training. Honestly it is a workshop paper with few numbers, but the design choices are useful.

## How it works

- Inputs: per-joint descriptions d_j, per-joint observations o_j, general observations (trunk velocity, gravity...)
- Joint encoder: attention weights α_j = softmax over f_φ(d_j)/τ, joint latent = Σ α_j f_ψ(o_j)
- Core MLP with 5 layers and WeightNorm, action mean per joint μ_j = z_action · α_j, std from the description encoding
- Critic keeps foot observations and gets noise-free observations
- ER: body part size and position, mass and inertia (coupled and decoupled), CoM, axis orientations, IMU position, motor limits, damping, friction, armature, stiffness, nominal pose, PD gains, action scale, then hidden DR on top
- Embodiment resampled with probability 0.2% per step, up to 10M embodiments per run
- Single curriculum β with adaptive steps n·Δβ based on consecutive successes or failures

## Results that matter

- Final curriculum level: URMAv2 0.88 vs URMA 0.82, zero-padding and multi-head much lower
- Leave-one-out zero-shot: strong on Go2 and Silver Badger, humanoids (H1, G1, T1) weaker but better than the 0 return of original URMA on H1
- Real robot: Go2 and Badger robust to pushes, T1 forward and lateral OK but falls when turning or walking backward, H1 zero-shot not stable, the all-50 policy walks H1 in every direction
- 40 h on a single A100 for 5B steps

## Interesting stuff

- Resampling the embodiment during the episode is very relevant: the policy already sees bodies that change mid-episode, even if abruptly
- Specifying all randomization as a percentage of URDF values makes it possible to use the same reward and ranges for every robot
- One curriculum knob for everything is pragmatic and probably the reason humanoids train at all
- Very few quantitative results, mostly a curve and qualitative real tests, and humanoid zero-shot transfer is still weak
- No code release found for this paper

## Useful for me

- **Apply.** Embodiment randomization that is visible to the policy through joint descriptions, plus hidden DR on top, is the right structure for a morphing humanoid: the body parameters the robot knows go in the input, the unknown ones get randomized.
- **Apply.** Resampling the body during the episode (0.2% per step) is a cheap first step toward training for bodies that change during a task.
- **As prior work.** Strongest evidence that massive body randomization works for legged locomotion, and that humanoids are the hardest case.
- **Watch out.** Zero-shot to unseen humanoids is still unreliable, so training must cover the actual range of our own body configurations.

## Key prior work

- [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · the architecture this extends
- [[Towards Embodiment Scaling Laws in Robot Locomotion|Embodiment scaling laws]] · scaling URMA to 1000 generated robots
- [[MetaMorph - Learning Universal Controllers with Transformers|MetaMorph]] · transformers for universal morphology control
- [[NerveNet - Learning Structured Policy with Graph Neural Networks|NerveNet]] · GNN policies over the body graph
- [[Domain Randomization for Transferring Deep Neural Networks from Simulation to the Real World|Domain randomization]] · the DR base idea
- [[RL-X - A Deep Reinforcement Learning Library (not only) for RoboCup|RL-X]] · the training library

## References

1. [[Learning coordinated badminton skills for legged manipulators|Legged badminton]] · Ma et al. · 2025 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+coordinated+badminton+skills+for+legged+manipulators)
2. [[HITTER - A Humanoid Table Tennis Robot via Hierarchical Planning and Learning|HITTER]] · Su et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2508.21043)
3. [[Walk These Ways - Tuning Robot Control for Generalization with Multiplicity of Behavior|Walk These Ways]] · Margolis & Agrawal · 2023 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Walk+these+ways+Tuning+robot+control+for+generalization+with+multiplicity+of+behavior)
4. [[Extreme Parkour with Legged Robots|Extreme Parkour]] · Cheng et al. · 2023 · CoRL Workshop · [arXiv](https://arxiv.org/abs/2309.14341)
5. [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · Bohlinger et al. · 2024 · CoRL · [arXiv](https://arxiv.org/abs/2409.06366)
6. [[Bridge the Gap - Enhancing Quadruped Locomotion with Vertical Ground Perturbations|Vertical ground perturbations]] · Stasica et al. · 2025 · IROS · [Scholar](https://scholar.google.com/scholar?q=Bridge+the+gap+Enhancing+quadruped+locomotion+with+vertical+ground+perturbations)
7. [[Orbit - A Unified Simulation Framework for Interactive Robot Learning Environments|Orbit]] · Mittal et al. · 2023 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Orbit+A+unified+simulation+framework+for+interactive+robot+learning+environments)
8. [[MuJoCo - A physics engine for model-based control|MuJoCo]] · Todorov et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=MuJoCo+A+physics+engine+for+model-based+control)
9. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
10. [[Domain Randomization for Transferring Deep Neural Networks from Simulation to the Real World|Domain randomization]] · Tobin et al. · 2017 · IROS · [Scholar](https://scholar.google.com/scholar?q=Domain+randomization+for+transferring+deep+neural+networks+from+simulation+to+the+real+world)
11. [[Rapid Locomotion via Reinforcement Learning|Rapid locomotion]] · Margolis et al. · 2024 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Rapid+locomotion+via+reinforcement+learning)
12. [[Gait in Eight - Efficient On-Robot Learning for Omnidirectional Quadruped Locomotion|Gait in Eight]] · Bohlinger et al. · 2025 · IROS · [Link](https://www.ias.informatik.tu-darmstadt.de/uploads/Team/NicoBohlinger/gait_in_eight.pdf)
13. [[NerveNet - Learning Structured Policy with Graph Neural Networks|NerveNet]] · Wang et al. · 2018 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Nervenet+Learning+structured+policy+with+graph+neural+networks)
14. [[MetaMorph - Learning Universal Controllers with Transformers|MetaMorph]] · Gupta et al. · 2022 · ICLR · [arXiv](https://arxiv.org/abs/2203.11931)
15. [[Towards Embodiment Scaling Laws in Robot Locomotion|Embodiment scaling laws]] · Ai et al. · 2025 · CoRL · [arXiv](https://arxiv.org/abs/2505.05753)
16. [[Weight Normalization - A Simple Reparameterization to Accelerate Training of Deep Neural Networks|WeightNorm]] · Salimans & Kingma · 2016 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Weight+normalization+A+simple+reparameterization+to+accelerate+training+of+deep+neural+networks)
17. [[RL-X - A Deep Reinforcement Learning Library (not only) for RoboCup|RL-X]] · Bohlinger & Dorer · 2023 · RoboCup Symposium · [GitHub search](https://github.com/search?q=RL-X+Bohlinger&type=repositories)

## See also

- [[Scalable and General Whole-Body Control for Cross-Humanoid Locomotion|XHugWBC]] · humanoid-only version with physics-consistent morphology randomization
- [[Multi-embodiment Legged Robot Control as a Sequence Modeling Problem|EAT]] · offline transformer route to body-conditioned control
