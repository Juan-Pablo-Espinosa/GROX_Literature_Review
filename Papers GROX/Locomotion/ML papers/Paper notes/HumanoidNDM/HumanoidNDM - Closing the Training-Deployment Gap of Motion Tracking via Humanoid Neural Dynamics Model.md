---
title: "HumanoidNDM: Closing the Training-Deployment Gap of Motion Tracking via Humanoid Neural Dynamics Model"
aliases:
  - HumanoidNDM
authors:
  - "[[Chao Chen]]"
  - "[[Zhenrong Wang]]"
  - "[[Jilong Wang]]"
  - "[[Yusen Feng]]"
  - "[[Jiangran Lyu]]"
  - "[[Li Yi]]"
  - "[[He Wang]]"
corresponding:
  - "[[He Wang]]"
  - "[[Li Yi]]"
institutions:
  - "[[Peking University]]"
  - "[[Tsinghua University]]"
  - "[[Chinese Academy of Sciences]]"
  - "[[Beijing Academy of Artificial Intelligence]]"
labs:
  - "[[EPIC Lab]]"
  - "[[Galbot]]"
countries:
  - China
year: 2026
date: 2026-10-25
venue: CoRL 2026
venue_type: conference
arxiv_category: cs.RO
arxiv_id: "2610.34126"
url: https://arxiv.org/abs/2610.34126
pdf: https://arxiv.org/pdf/2610.34126
code: "No official repo yet (checked 2026-10-29)"
project_page: https://pku-epic.github.io/HumanoidNDM
status: to-read
relevance: high
added: 2026-10-29
tags:
  - paper
  - topic/humanoids
  - topic/motion-tracking
  - topic/whole-body-control
  - topic/sim-to-real
  - topic/world-models
  - topic/real-world-fine-tuning
  - method/model-based-rl
  - method/reinforcement-learning
  - method/transformer
  - robot/unitree-g1
  - sim/mujoco
  - eval/real-robot
---

# HumanoidNDM

> [!info] Quick view
> **Authors:** [[Chao Chen]], [[Zhenrong Wang]], [[Jilong Wang]], [[Yusen Feng]], [[Jiangran Lyu]], [[Li Yi]] (corresponding), [[He Wang]] (corresponding)
> **From:** [[Peking University]] · [[EPIC Lab]] · [[Galbot]] · [[Tsinghua University]] · [[Chinese Academy of Sciences]] · [[Beijing Academy of Artificial Intelligence]]
> **Published:** CoRL 2026 · Oct 2026 · cs.RO
> **Keywords:** training-deployment gap, neural dynamics model, online fine-tuning, MoCap-free, real-world adaptation, motion tracking, analytic policy gradients
> **Links:** [arXiv](https://arxiv.org/abs/2610.34126) · [PDF](https://arxiv.org/pdf/2610.34126) · [Project page](https://pku-epic.github.io/HumanoidNDM)
> **One line:** fine-tune a sim-trained humanoid tracking policy on the real robot by learning a neural dynamics model from real proprioceptive data and backpropagating through it, no motion capture needed

> [!warning] Partly incomplete
> The exact per-setting result tables were not captured. Check Table 2 in the PDF for the numbers.

## Summary

Motion tracking policies work great in simulation, but on the real robot they hit a dynamics gap, from extra payloads, worn motors or just a simulator that is not accurate. This paper argues that the last mile has to come from real-world data, so they propose HumanoidNDM, an online loop that keeps improving the policy directly on the robot. Each iteration has three stages. First they collect real data with a safe exploration method. Then they train a neural dynamics model on that data. Finally they fine-tune the policy by backpropagating gradients through the learned model.

The key point is that everything is MoCap-free. They only use proprioception and get Euclidean-space supervision through differentiable forward kinematics, so you dont need a motion capture room to adapt the robot. In MuJoCo it performs close to an oracle policy trained with PPO on the exact disturbance, and on a real G1 it adapts to payloads, elastic bands and even a damaged motor. Honestly this is one of the more practical sim-to-real papers I have seen, because the setup is something a small lab can actually reproduce.

## How it works

- Start from a tracking policy trained in simulation and deploy it on the real robot to collect data
- **FENCE** (Frequency-domain Exploration via Neighborhood Coverage Estimation) injects low-frequency noise into the reference trajectories instead of the actions. The policy's own closed-loop robustness keeps the robot stable while it explores, and the intensity adapts based on how much of the policy's neighborhood is already covered
- The zero-frequency component of the noise is set to zero so the perturbation never shifts the whole posture and makes it collapse
- **Neural dynamics model (NDM)** is a history-based Transformer ensemble that predicts the next state from a sequence of past states and actions, using proprioception only
- Differentiable forward kinematics turns the predicted joint states into link positions, so the loss is computed in Euclidean space and not only in joint space. Root height is taken from the reference to avoid propagating estimation errors
- **Policy fine-tuning** is treated as supervised learning, backpropagating analytic gradients through the differentiable NDM instead of running model-free RL, and that is where the data efficiency comes from
- They also test pretraining the NDM on domain-randomized simulation data before the real-world data
- The three stages repeat as an online loop: collect, train the model, fine-tune, then deploy again

## Results that matter

- Simulation is in MuJoCo with a G1 model. Five motions (walk, balance, squat, swing, stride) × three disturbance types = 15 settings, one type being inertial variations (added mass and shifted CoM on specific links)
- Baselines are Direct Transfer, ASAP (delta action model), RWM (model-based) and PPO w/ Disturb., which is an oracle trained directly on the exact disturbance
- HumanoidNDM is best in 40 of 60 metrics and performs on par with the oracle, with the largest gains in Euclidean-space metrics
- It shows a clear advantage on walk, squat and swing, but it is only comparable to the baselines on balance and stride
- On the real G1 the disturbances are 1.5 kg on each calf (swallow balance), 1.5 kg on the left hand (arm swing), 0.75 kg on the left hand (pen capping), elastic bands on both wrists (lateral raise) and a damaged arm motor (bowing), plus walking
- Real data per setting is small, between 50 and 200 trajectories

## Interesting stuff

- The damaged-motor experiment is the coolest part to me. That is a gap you cannot model in a simulator, and the learned dynamics just absorbs it
- Making it MoCap-free is a big deal practically, because ASAP style methods need motion capture to see global positions
- Perturbing the reference instead of the actions is a smart trick. The policy is already robust to reference changes, so exploration stays safe on real hardware
- Gradients through a learned model can exploit model errors, which is the classic problem of analytic policy gradients. They use an ensemble, but it is still something to watch
- The adaptation is per motion and per disturbance, not one general policy that adapts to everything

## Useful for me

- **As prior work.** Strong reference for arguing that real-world fine-tuning, not only domain randomization, is needed to close the sim-to-real gap for a custom humanoid whose simulation model is never going to be perfect.
- **Apply.** The MoCap-free trick, supervising a dynamics model in Euclidean space through differentiable forward kinematics using only proprioception. This makes real-world adaptation possible without a motion capture setup.
- **Apply.** FENCE style low-frequency reference perturbations for collecting real data safely on an expensive prototype, instead of random action noise.
- **Gap.** A humanoid that changes its morphology changes its own dynamics, so a learned dynamics model would need to be conditioned on the current body or retrained per configuration. Nobody here deals with a body that changes during deployment.
- **Watch out.** Tested only on the G1 and adapted per motion. For a robot with less accurate hardware the model error could be larger and the gradients less trustworthy.

## Key prior work

- [[DexNDM - Closing the Reality Gap for Dexterous In-Hand Rotation via Joint-Wise Neural Dynamics Model|DexNDM]] · same group, the neural dynamics model idea first used for dexterous hands, direct predecessor of this paper
- [[SuperTrack - Motion Tracking for Physically Simulated Characters using Supervised Learning|SuperTrack]] · the original idea of training a tracker with supervised gradients through a learned world model
- [[ASAP - Aligning Simulation and Real-World Physics for Learning Agile Humanoid Whole-Body Skills|ASAP]] · main competitor, learns a delta action model from real data but needs motion capture
- [[Robotic World Model - A Neural Network Simulator for Robust Policy Optimization in Robotics|RWM]] · model-based baseline, trains policies inside a learned neural simulator
- [[Track Any Motions under Any Disturbances|Any2Track]] · same group, robust tracking under disturbances trained only in sim, the "before" of this paper
- [[Accelerated Policy Learning with Parallel Differentiable Simulation|SHAC]] · analytic policy gradients through a differentiable simulator, same optimization idea
- [[Robot Trains Robot - Automatic Real-World Policy Adaptation and Learning for Humanoids|Robot Trains Robot]] · another way to do real-world humanoid adaptation, worth comparing
- [[Pink Noise Is All You Need - Colored Noise Exploration in Deep Reinforcement Learning|Pink Noise]] · colored noise exploration, the background for FENCE

## References

1. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
2. [[Soft Actor-Critic - Off-Policy Maximum Entropy Deep Reinforcement Learning with a Stochastic Actor|SAC]] · Haarnoja et al. · 2018 · ICML · [Scholar](https://scholar.google.com/scholar?q=Soft+Actor-Critic+Off-Policy+Maximum+Entropy+Deep+Reinforcement+Learning+with+a+Stochastic+Actor)
3. [[Proprioceptive Actuator Design in the MIT Cheetah - Impact Mitigation and High-Bandwidth Physical Interaction for Dynamic Legged Robots|MIT Cheetah Actuator]] · Wensing et al. · 2017 · IEEE T-RO · [DOI](https://doi.org/10.1109/TRO.2016.2640183)
4. [[Mini Cheetah - A Platform for Pushing the Limits of Dynamic Quadruped Control|Mini Cheetah]] · Katz et al. · 2019 · ICRA · [DOI](https://doi.org/10.1109/ICRA.2019.8793865)
5. [[ExBody2 - Advanced Expressive Humanoid Whole-Body Control|ExBody2]] · Ji et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2412.13196)
6. [[OmniH2O - Universal and Dexterous Human-to-Humanoid Whole-Body Teleoperation and Learning|OmniH2O]] · He et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2406.08858)
7. [[Track Any Motions under Any Disturbances|Any2Track]] · Zhang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.13833)
8. [[GMT - General Motion Tracking for Humanoid Whole-Body Control|GMT]] · Chen et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2506.14770)
9. [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · Liao et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2508.08241)
10. [[Smooth Exploration for Robotic Reinforcement Learning|gSDE]] · Raffin et al. · 2022 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Smooth+Exploration+for+Robotic+Reinforcement+Learning)
11. [[A Scoping Review of Energy Consumption in Industrial Robotics|Energy in Industrial Robotics]] · Muru & Rassõlkin · 2025 · Machines · [Scholar](https://scholar.google.com/scholar?q=A+Scoping+Review+of+Energy+Consumption+in+Industrial+Robotics)
12. [[ASAP - Aligning Simulation and Real-World Physics for Learning Agile Humanoid Whole-Body Skills|ASAP]] · He et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.01143)
13. [[SuperTrack - Motion Tracking for Physically Simulated Characters using Supervised Learning|SuperTrack]] · Fussell et al. · 2021 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=SuperTrack+Motion+Tracking+for+Physically+Simulated+Characters+using+Supervised+Learning)
14. [[ControlVAE - Model-Based Learning of Generative Controllers for Physics-Based Characters|ControlVAE]] · Yao et al. · 2022 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=ControlVAE+Model-Based+Learning+of+Generative+Controllers+for+Physics-Based+Characters)
15. [[Dream to Control - Learning Behaviors by Latent Imagination|Dreamer]] · Hafner et al. · 2019 · arXiv · [arXiv](https://arxiv.org/abs/1912.01603)
16. [[Mastering Atari with Discrete World Models|DreamerV2]] · Hafner et al. · 2020 · arXiv · [arXiv](https://arxiv.org/abs/2010.02193)
17. [[Mastering Diverse Domains through World Models|DreamerV3]] · Hafner et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2301.04104)
18. [[Accelerated Policy Learning with Parallel Differentiable Simulation|SHAC]] · Xu et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2204.07137)
19. [[Closing Sim-to-Real Gap for Heavy-loaded Humanoid Agile Motion Skills via Differentiable Simulation|HALO]] · Wang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2603.15084)
20. [[Sampling-Based System Identification with Active Exploration for Legged Robot Sim2Real Learning|SPI-Active]] · Sobanbabu et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2505.14266)
21. [[Bridging the Sim-to-Real Gap for Athletic Loco-Manipulation|Athletic Loco-Manipulation]] · Fey et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.10894)
22. [[DexNDM - Closing the Reality Gap for Dexterous In-Hand Rotation via Joint-Wise Neural Dynamics Model|DexNDM]] · Liu et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2510.08556)
23. [[A Walk in the Park - Learning to Walk in 20 Minutes With Model-Free Reinforcement Learning|A Walk in the Park]] · Smith et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2208.07860)
24. [[Legged Robots that Keep on Learning - Fine-Tuning Locomotion Policies in the Real World|Legged Robots that Keep on Learning]] · Smith et al. · 2022 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Legged+Robots+that+Keep+on+Learning+Fine-Tuning+Locomotion+Policies+in+the+Real+World)
25. [[Grow Your Limits - Continuous Improvement with Real-World RL for Robotic Locomotion|Grow Your Limits]] · Smith et al. · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Grow+Your+Limits+Continuous+Improvement+with+Real-World+RL+for+Robotic+Locomotion)
26. [[Safe Reinforcement Learning for Legged Locomotion|Safe RL for Legged Locomotion]] · Yang et al. · 2022 · IROS · [Scholar](https://scholar.google.com/scholar?q=Safe+Reinforcement+Learning+for+Legged+Locomotion)
27. [[Robot Trains Robot - Automatic Real-World Policy Adaptation and Learning for Humanoids|Robot Trains Robot]] · Hu et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2508.12252)
28. [[Towards Real Robot Learning in the Wild - A Case Study in Bipedal Locomotion|Real Robot Learning in the Wild]] · Bloesch et al. · 2022 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Towards+Real+Robot+Learning+in+the+Wild+A+Case+Study+in+Bipedal+Locomotion)
29. [[Simulation Distillation - Pretraining World Models in Simulation for Rapid Real-World Adaptation|Simulation Distillation]] · Levy et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2603.15759)
30. [[RISE - Self-Improving Robot Policy with Compositional World Model|RISE]] · Yang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.11075)
31. [[Robotic World Model - A Neural Network Simulator for Robust Policy Optimization in Robotics|RWM]] · Li et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2501.10100)
32. [[Uncertainty-Aware Robotic World Model Makes Offline Model-Based Reinforcement Learning Work on Real Robots|RWM-U]] · Li et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2504.16680)
33. [[Towards Bridging the Gap between Large-Scale Pretraining and Efficient Finetuning for Humanoid Control|Pretraining to Finetuning for Humanoids]] · Huang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2601.21363)
34. [[Learning to Walk from Three Minutes of Real-World Data with Semi-structured Dynamics Models|Three Minutes of Data]] · Levy et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2410.09163)
35. [[An Efficient Model-Based Approach on Learning Agile Motor Skills without Reinforcement|Agile Skills without RL]] · Shi et al. · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=An+Efficient+Model-Based+Approach+on+Learning+Agile+Motor+Skills+without+Reinforcement)
36. [[Off-Policy Reinforcement Learning with Model-Based Exploration Augmentation|Model-Based Exploration Augmentation]] · Wang et al. · 2026 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Off-Policy+Reinforcement+Learning+with+Model-Based+Exploration+Augmentation)
37. [[A Survey of Exploration Methods in Reinforcement Learning|Exploration Survey (Amin)]] · Amin et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2109.00157)
38. [[Exploration and Safety in Deep Reinforcement Learning|Achiam Thesis]] · Achiam · 2021 · PhD thesis, UC Berkeley · [Scholar](https://scholar.google.com/scholar?q=Exploration+and+Safety+in+Deep+Reinforcement+Learning+Achiam)
39. [[Continuous Control with Deep Reinforcement Learning|DDPG]] · Lillicrap et al. · 2016 · ICLR · [arXiv](https://arxiv.org/abs/1509.02971)
40. [[Pink Noise Is All You Need - Colored Noise Exploration in Deep Reinforcement Learning|Pink Noise]] · Eberhard et al. · 2023 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Pink+Noise+Is+All+You+Need+Colored+Noise+Exploration+in+Deep+Reinforcement+Learning)
41. [[Cola-Flow Policy - Temporally Coherent Imitation Learning via Continuous Latent Action Flow Matching for Robotic Manipulation|Cola-Flow Policy]] · Songwei et al. · 2026 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Cola-Flow+Policy+Temporally+Coherent+Imitation+Learning+via+Continuous+Latent+Action+Flow+Matching)
42. [[Exploration in Deep Reinforcement Learning - A Survey|Exploration Survey (Ladosz)]] · Ladosz et al. · 2022 · Information Fusion · [Scholar](https://scholar.google.com/scholar?q=Exploration+in+Deep+Reinforcement+Learning+A+Survey+Ladosz)
43. [[Rapidly Adapting Policies to the Real World via Simulation-Guided Fine-Tuning|SGFT]] · Yin et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.02705)
44. [[Grow Your Limits - Continuous Improvement with Real-World RL for Robotic Locomotion|Grow Your Limits]] · Smith et al. · 2024 · ICRA · duplicate of [25] in the paper · [Scholar](https://scholar.google.com/scholar?q=Grow+Your+Limits+Continuous+Improvement+with+Real-World+RL+for+Robotic+Locomotion)
45. [[Reinforcement Learning - An Introduction|Sutton & Barto]] · Sutton & Barto · book · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+Learning+An+Introduction+Sutton+Barto)
46. [[Simple Statistical Gradient-Following Algorithms for Connectionist Reinforcement Learning|REINFORCE]] · Williams · 1992 · Machine Learning · [Scholar](https://scholar.google.com/scholar?q=Simple+Statistical+Gradient-Following+Algorithms+for+Connectionist+Reinforcement+Learning)
47. [[Simple Noisy Environment Augmentation for Reinforcement Learning|Noisy Env Augmentation]] · Khraishi & Okhrati · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2305.02882)
48. [[Neighboring State-based Exploration for Reinforcement Learning|Neighboring State Exploration]] · Li et al. · 2025 · preprint · [Scholar](https://scholar.google.com/scholar?q=Neighboring+State-based+Exploration+for+Reinforcement+Learning)
49. [[MuJoCo Playground|MuJoCo Playground]] · Zakka et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.08844)
50. [[When to Trust Your Model - Model-Based Policy Optimization|MBPO]] · Janner et al. · 2019 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=When+to+Trust+Your+Model+Model-Based+Policy+Optimization)
51. [[A Unified View on Solving Objective Mismatch in Model-Based Reinforcement Learning|Objective Mismatch]] · Wei et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2310.06253)
52. [[Ready Policy One - World Building Through Active Learning|Ready Policy One]] · Ball et al. · 2020 · ICML · [Scholar](https://scholar.google.com/scholar?q=Ready+Policy+One+World+Building+Through+Active+Learning)
53. [[MOPO - Model-based Offline Policy Optimization|MOPO]] · Yu et al. · 2020 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=MOPO+Model-based+Offline+Policy+Optimization)
54. [[First Return, Then Explore|Go-Explore]] · Ecoffet et al. · 2021 · Nature · [Scholar](https://scholar.google.com/scholar?q=First+Return+Then+Explore+Ecoffet)
55. [[Neural Robot Dynamics|NeRD]] · Xu et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2508.15755)
56. [[Contact-Aware Neural Dynamics|Contact-Aware Neural Dynamics]] · Jing et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2601.12796)
57. [[DeepMimic - Example-Guided Deep Reinforcement Learning of Physics-Based Character Skills|DeepMimic]] · Peng et al. · 2018 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=DeepMimic+Example-Guided+Deep+Reinforcement+Learning+of+Physics-Based+Character+Skills)

## See also

- [[Scalable Humanoid Whole-Body Control via Differentiable Neural Network Dynamics|Differentiable NN Dynamics]] · neural dynamics model used as a differentiable simulator for humanoid control · [Scholar](https://scholar.google.com/scholar?q=Scalable+Humanoid+Whole-Body+Control+via+Differentiable+Neural+Network+Dynamics)
- [[SuperMimic - Scaling Humanoid Whole-Body Control with One Differentiable World Model|SuperMimic]] · same idea of backpropagating through a learned world model, but for training the tracker at scale · [arXiv](https://arxiv.org/abs/2610.28480)
- [[MimicAll - Structured Embodiment Conditioning for Cross-Humanoid Whole-Body Motion Tracking|MimicAll]] · body-aware tracking, combine with this for a morphing robot