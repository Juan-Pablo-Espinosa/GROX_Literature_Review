---
title: "PolyMorph: Learning Locomotion Policy for Cross-Embodiment Adaptation"
aliases:
  - PolyMorph
authors:
  - "[[Hyunsik Oh]]"
  - "[[Seunghun Jeon]]"
  - "[[Donghoon Youm]]"
  - "[[Jungwoo Hur]]"
  - "[[Hyeongjun Kim]]"
  - "[[Jeong Hyun Lee]]"
  - "[[Moonkyu Jung]]"
  - "[[Jemin Hwangbo]]"
corresponding: ""
institutions:
  - "[[Korea Advanced Institute of Science and Technology]]"
labs:
  - "[[RaiLab]]"
countries:
  - South Korea
year: 2026
date: 2026-10-14
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2610.19615"
url: https://arxiv.org/abs/2610.19615
pdf: https://arxiv.org/pdf/2610.19615
code: "No official repo yet (checked 2026-10-31)"
status: to-read
relevance: high
added: 2026-10-31
tags:
  - paper
  - topic/cross-embodiment
  - topic/morphology
  - topic/legged-locomotion
  - topic/sim-to-real
  - topic/domain-randomization
  - method/reinforcement-learning
  - method/transformer
  - method/graph-neural-network
  - method/curriculum-learning
  - robot/raibo
  - robot/atlas
  - sim/raisim
  - eval/real-robot
---

# PolyMorph

> [!info] Quick view
> **Authors:** [[Hyunsik Oh]], [[Seunghun Jeon]], [[Donghoon Youm]], [[Jungwoo Hur]], [[Hyeongjun Kim]], [[Jeong Hyun Lee]], [[Moonkyu Jung]], [[Jemin Hwangbo]]
> **From:** [[Korea Advanced Institute of Science and Technology]] · [[RaiLab]]
> **Published:** arXiv preprint · October 2026 · cs.RO
> **Keywords:** cross-embodiment locomotion, morphology-conditioned policy, URDF tokens, graph transformer, per-joint decoder, reward normalization, automatic PD gains, legged robots
> **Links:** [arXiv](https://arxiv.org/abs/2610.19615) · [PDF](https://arxiv.org/pdf/2610.19615) · [HTML](https://arxiv.org/html/2610.19615)
> **One line:** Turn every robot's URDF into joint-link tokens, run them through a morphology-masked graph transformer with per-joint decoders, and one RL policy walks bipeds, tripods and quadrupeds, including real quadrupeds with legs removed.

## Summary

This paper try to make one locomotion policy that works across legged robots with different numbers of legs, joints and mechanical properties, without retraining for each robot. The trick is to read the robot directly from its URDF: each joint plus its child link becomes a token with mass, inertia, CoM, joint state and PD gains, a graph transformer with an adjacency mask mixes the tokens along the kinematic tree, and a per-joint decoder outputs the target position for each actuated joint. To make training across very different robots stable, they normalize rewards by joint and leg count, oversample the robots that are doing badly, and compute PD gains automatically from the URDF mass matrix.

It works and it's honest about where it breaks. One policy runs 10+ robots in sim from 11 kg to 135 kg, and on real Raibo quadrupeds it walks 20/20 as a quadruped and 20/20 as a tripod, 9/10 as a biped, and 7/10 on an unseen tripod configuration. But it doesn't beat URMA or the MLP ablation on every robot, the comparison tables are not fully consistent, and the authors say themselves that generalization only holds inside the morphology range seen in training.

## How it works

- **URDF to tokens.** Each robot is parsed into a sequence of joint-link modules ordered by traversing the kinematic tree from the root, zero-padded up to 15 actuated joints. Token features include link mass, inertia, CoM position, joint state, recent target actions and PD gains.
- **Recurrent encoder.** A shared GRU encodes proprioceptive history (roll-pitch, angular velocity, joint positions and velocities, previous targets). The critic also gets privileged info.
- **Graph transformer.** Each token gets its own linear projection plus an absolute positional embedding. Attention uses a morphology adjacency mask in the Body Transformer style, so modules only attend to connected modules.
- **Per-joint decoder.** Each module has a decoder head that combines its transformer output, the GRU latent and the velocity command, and outputs a Gaussian action mean for that joint only. The critic averages value estimates over the root and actuated modules.
- **Morphology-normalized rewards.** Joint penalties are divided by the number of actuated joints, and leg and foot terms by the number of legs, so a 12-joint robot doesn't dominate the gradient just because it has more joints.
- **Adaptive robot sampling.** Robots with lower reward get sampled more through a softmax over the reward gap, while half the environments stay uniform.
- **PD gains from the URDF.** Kp = α²λ, with λ the sum of the first three diagonal mass-matrix entries at the nominal pose, and Kd from a damping ratio (α = ζ = 0.8 for all robots). No manual tuning per robot.
- **Training.** RaiSim with 350 parallel envs for 20k iterations (about 2.7 days) on an EPYC CPU and an RTX 4090, plus curriculum, cosine LR with warm restarts and heavy domain randomization (noise, link and joint params, PD gains, latency, friction, pushes). The actor is only ~0.33M params, 3 seeds, deployed at 100 Hz through ONNX with 0.366 ms latency.

## Results that matter

- **Real hardware (50 m outdoor blind route).** Raibo1 and Raibo2 as quadrupeds 20/20 and as tripods 20/20, Raibo2 as a biped 9/10, and an unseen Raibo1 tripod configuration 7/10. The unseen failures were swing feet catching on obstacles, and the biped failure was loss of balance.
- **Reward normalization ablation.** Evaluation return 5.024 with normalization vs 1.109 without, termination rate 0.03% vs 1.56%, and the correlation between joint count and joint reward drops from 0.904 to 0.256. It's basically the piece that makes multi-morphology training work.
- **Adaptive sampling.** With uniform sampling, Atlas never learns and its reward goes down during training. Adaptive sampling fixes it.
- **Automatic PD gains.** Close to the hand-tuned ones (Raibo1 Kp 51 vs 50, Atlas 260 vs 300), and the same α and ζ work on unseen B2 and Solo12 without retuning.
- **vs MetaMorph and URMA (Vxy RMSE, lower is better).** PolyMorph is the most consistent across robots. MetaMorph is much worse everywhere (around 1 m/s on several robots), and URMA fails on unseen Hound and on the unseen HR tripod.
- **Where it doesn't win.** URMA is better on Anymal-C (0.107 vs 0.160), and the plain MLP variant beats PolyMorph on the seen HL tripod and the unseen HR tripod. Solo12 (2.5 kg, much lighter than the training robots) tracks noticeably worse.

## Interesting stuff

- Reading the robot straight from the URDF is a very practical interface. You don't hand-design an embodiment vector, you just give it the robot file, and the PD gains come from the same file.
- Reward normalization by joint and leg count sounds trivial, but the ablation shows it's the difference between working and not working. Most multi-embodiment papers kinda hide this kind of detail.
- The real-robot tripod and biped tests are done by locking or removing legs on the same quadruped. That's clever and cheap, but a "biped" Raibo is not a humanoid, and the only humanoid (Atlas) is sim-only.
- Tables IV and V give different numbers for the same robots, which probably means different evaluation runs, and that makes the baseline comparison harder to trust.
- Training is small by today's standards (350 CPU envs, 2.7 days). Good for reproducibility, but nobody knows yet how it scales to hundreds of procedurally generated bodies.

## Useful for me

- **As prior work.** One of the closest papers to what we need: a policy that reads its own body description and outputs per-joint actions with a variable joint count. The URDF-token design is the obvious starting point for a policy that knows its current morphology.
- **Apply.** Morphology-normalized rewards and URDF-derived PD gains are directly reusable. When the body changes, the gains and reward scales should change with it, and this gives a principled way to do it without hand-tuning each configuration.
- **Gap.** Every morphology is fixed during an episode. Nothing here handles the body changing while the robot is walking, and the token features (mass, inertia, CoM) would need to update online through that transition. That is exactly the gap for a humanoid that changes its limbs during the task.
- **Watch out.** The authors say generalization degrades outside the training morphology range. If we want to transfer to body shapes we haven't trained on, we need to procedurally generate a lot of body variants and not just a handful of robots, which is their own future-work idea too.
- **Watch out.** No real humanoid results. Balance for a tall humanoid is a harder problem than a quadruped walking on two legs, so their 9/10 biped number doesn't say much about our case.

## Key prior work

- [[MetaMorph - Learning Universal Controllers with Transformers|MetaMorph]] · the original transformer-over-limbs universal controller, a main baseline and clearly weaker here
- [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · the strongest baseline, morphology-agnostic encoders and decoders, still wins on some robots
- [[Body Transformer - Leveraging Robot Embodiment for Policy Learning|Body Transformer]] · where the morphology adjacency mask in attention comes from
- [[NerveNet - Learning Structured Policy with Graph Neural Networks|NerveNet]] · the classic graph-network policy over the robot's body graph
- [[GenLoco - Generalized Locomotion Controllers for Quadrupedal Robots|GenLoco]] · one controller for many quadrupeds, but only within the same morphology class
- [[RMA - Rapid Motor Adaptation for Legged Robots|RMA]] · implicit adaptation from history, the alternative to giving the policy the body explicitly
- [[Per-Contact Iteration Method for Solving Contact Dynamics|RaiSim]] · the simulator they train in

## References

1. [[Imitating and Finetuning Model Predictive Control for Robust and Symmetric Quadrupedal Locomotion|MPC Imitation Finetuning]] · Youm et al. · 2023 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Imitating+and+finetuning+model+predictive+control+for+robust+and+symmetric+quadrupedal+locomotion)
2. [[DeepMimic - Example-Guided Deep Reinforcement Learning of Physics-Based Character Skills|DeepMimic]] · Peng et al. · 2018 · ACM TOG · [arXiv](https://arxiv.org/abs/1804.02717)
3. [[Learning Fine-Grained Bimanual Manipulation with Low-Cost Hardware|ALOHA]] · Zhao et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2304.13705)
4. [[Learning agile and dynamic motor skills for legged robots|Agile Motor Skills]] · Hwangbo et al. · 2019 · Science Robotics · [arXiv](https://arxiv.org/abs/1901.08652)
5. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
6. [[Learning robust perceptive locomotion for quadrupedal robots in the wild|Perceptive Locomotion]] · Miki et al. · 2022 · Science Robotics · [arXiv](https://arxiv.org/abs/2201.08117)
7. [[RMA - Rapid Motor Adaptation for Legged Robots|RMA]] · Kumar et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2107.04034)
8. [[High-speed control and navigation for quadrupedal robots on complex and discrete terrain|Raibo High-Speed Navigation]] · Kim et al. · 2025 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=High-speed+control+and+navigation+for+quadrupedal+robots+on+complex+and+discrete+terrain)
9. [[Learning coordinated badminton skills for legged manipulators|Badminton Legged Manipulator]] · Ma et al. · 2025 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+coordinated+badminton+skills+for+legged+manipulators)
10. [[RobotSketch - An Interactive Showcase of Superfast Design of Legged Robots|RobotSketch]] · Lee et al. · 2024 · ACM SIGGRAPH 2024 Emerging Technologies · [Scholar](https://scholar.google.com/scholar?q=RobotSketch+an+interactive+showcase+of+superfast+design+of+legged+robots)
11. [[Meta Reinforcement Learning for Optimal Design of Legged Robots|Meta-RL Legged Design]] · Belmonte-Baeza et al. · 2022 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Meta+reinforcement+learning+for+optimal+design+of+legged+robots)
12. [[Real-world humanoid locomotion with reinforcement learning|Real-World Humanoid RL]] · Radosavovic et al. · 2024 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Real-world+humanoid+locomotion+with+reinforcement+learning)
13. [[Attention Is All You Need|Transformer]] · Vaswani et al. · 2017 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Attention+is+all+you+need)
14. [[Body Transformer - Leveraging Robot Embodiment for Policy Learning|Body Transformer]] · Sferrazza et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2408.06316)
15. [[MetaMorph - Learning Universal Controllers with Transformers|MetaMorph]] · Gupta et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2203.11931)
16. [[GenLoco - Generalized Locomotion Controllers for Quadrupedal Robots|GenLoco]] · Feng et al. · 2023 · CoRL 2023 · [Scholar](https://scholar.google.com/scholar?q=GenLoco+Generalized+locomotion+controllers+for+quadrupedal+robots)
17. [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · Bohlinger et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2409.06366)
18. [[NerveNet - Learning Structured Policy with Graph Neural Networks|NerveNet]] · Wang et al. · 2018 · ICLR 2018 · [Scholar](https://scholar.google.com/scholar?q=NerveNet+Learning+structured+policy+with+graph+neural+networks)
19. [[GCNT - Graph-Based Transformer Policies for Morphology-Agnostic Reinforcement Learning|GCNT]] · Luo et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2505.15211)
20. [[LocoFormer - Generalist Locomotion via Long-Context Adaptation|LocoFormer]] · Liu et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.23745)
21. [[Cross-robot behavior adaptation through intention alignment|Intention Alignment]] · Chen et al. · 2026 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Cross-robot+behavior+adaptation+through+intention+alignment)
22. [[Demonstrate once, execute on many - Kinematic intelligence for cross-robot skill transfer|Demonstrate Once Execute on Many]] · Gupta et al. · 2026 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Demonstrate+once+execute+on+many+Kinematic+intelligence+for+cross-robot+skill+transfer)
23. [[Per-Contact Iteration Method for Solving Contact Dynamics|RaiSim]] · Hwangbo et al. · 2018 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Per-contact+iteration+method+for+solving+contact+dynamics)

## See also

- [[Towards Embodiment Scaling Laws in Robot Locomotion|Embodiment Scaling Laws]] · ~1,000 procedurally generated embodiments distilled into one URMA policy, the scale-up PolyMorph lists as future work
- [[H-Zero - Cross-Humanoid Locomotion Pretraining Enables Few-shot Novel Embodiment Transfer|H-Zero]] · cross-embodiment pretraining but for humanoids only, closer to our robot class