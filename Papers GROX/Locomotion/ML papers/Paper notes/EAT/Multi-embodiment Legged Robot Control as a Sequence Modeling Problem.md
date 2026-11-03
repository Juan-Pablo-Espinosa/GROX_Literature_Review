---
title: "Multi-embodiment Legged Robot Control as a Sequence Modeling Problem"
aliases:
  - EAT
  - Embodiment-aware Transformer
authors:
  - "[[Chen Yu]]"
  - "[[Weinan Zhang]]"
  - "[[Hang Lai]]"
  - "[[Zheng Tian]]"
  - "[[Laurent Kneip]]"
  - "[[Jun Wang]]"
corresponding: ""
institutions:
  - "[[ShanghaiTech University]]"
  - "[[Shanghai Jiao Tong University]]"
  - "[[University College London]]"
labs:
  - "[[Digital Brain Lab]]"
  - "[[UCL Centre for Artificial Intelligence]]"
countries:
  - China
  - UK
year: 2023
date: 2022-12-18
venue: IEEE ICRA 2023
venue_type: conference
arxiv_category: cs.RO
arxiv_id: "2212.09078"
url: https://arxiv.org/abs/2212.09078
pdf: https://arxiv.org/pdf/2212.09078v1
code: "No official repo found (checked 2026-11-02)"
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/legged-locomotion
  - topic/cross-embodiment
  - topic/morphology
  - topic/reconfigurable-robots
  - topic/co-design
  - topic/sim-to-real
  - method/transformer
  - method/behavior-cloning
  - method/ppo
  - method/bayesian-optimization
  - sim/isaac-gym
  - robot/mini-cheetah
  - eval/real-robot
---

# EAT

> [!info] Quick view
> **Authors:** [[Chen Yu]], [[Weinan Zhang]], [[Hang Lai]], [[Zheng Tian]], [[Laurent Kneip]], [[Jun Wang]]
> **From:** [[ShanghaiTech University]] · [[Digital Brain Lab]] · [[Shanghai Jiao Tong University]] · [[University College London]] · [[UCL Centre for Artificial Intelligence]]
> **Published:** IEEE ICRA 2023 · arXiv Dec 2022 · cs.RO
> **Keywords:** embodiment-aware transformer, decision transformer, offline RL, morphology conditioning, Mini Cheetah, variable leg length, Bayesian optimization, real-world evolution
> **Links:** [arXiv](https://arxiv.org/abs/2212.09078) · [PDF](https://arxiv.org/pdf/2212.09078v1) · [DOI](https://doi.org/10.1109/ICRA48891.2023.10161034)
> **One line:** a GPT-style transformer trained offline on PPO expert data from 27 Mini Cheetah body variants, conditioned on tibia and torso lengths, controls changed bodies on hardware and lets BO pick a body to step down a stair

## Summary

The idea is to have one controller that already knows how to handle many bodies, so morphology can be optimized in the real world without retraining. They train 27 PPO experts in Isaac Gym, one per Mini Cheetah variant (front tibia, hind tibia and torso lengths in {0.2, 0.25, 0.3} and {0.2, 0.3, 0.4} m), collect 1000 trajectories of 1000 steps each, and train the Embodiment-aware Transformer by behavior cloning on sequences of (embodiment, state, action) tokens. The embodiment vector is fed at every timestep, instead of returns-to-go like Decision Transformer.

In simulation EAT gets the best return on 57 of 80 body variants and an average score of 18.87 vs 16.53 for a single PPO policy, 13.32 for a transformer without the body vector and 3.8 for an MLP with the body vector. On a real Mini Cheetah with swappable tibias and 3D-printed torso extensions it walks with three bodies, including asymmetric ones where only EAT works. Then BO searches the body (with EAT fixed) to step down a 10 cm stair, and after about 10 real trials it finds a longer torso and different front and hind tibias that work. Honestly, the bodies are changed by hand between episodes and evaluation is small, but it is one of the few papers that actually changes leg length on a real dynamic robot and adapts with one policy.

## How it works

- Embodiment-aware MDP: body vector e sampled per episode, dynamics depend on e
- Data: 27 PPO experts with domain randomization (legged_gym style rewards), 1000 trajectories each
- Tokens: linear embeddings of e, s, a with layer norm plus timestep embeddings, causal GPT, MSE on the predicted action, context length H
- State: base linear and angular velocity, gravity, joint positions and velocities, last action, actions are PD joint targets
- Real robot: swappable steel tibia tubes and 3D printed torso parts with 1 cm accuracy, no fine-tuning
- Real-world evolution: BO over torso (0.27 to 0.35 m) and tibias (0.15 to 0.25 m), fitness = distance walked after a 10 cm step, max 20 iterations

## Results that matter

- Sim average score: EAT 18.87, PPO single policy 16.53, vanilla transformer 13.32, EABC 3.8, also best in a noisier test env (18.61)
- EAT best on 57/80 bodies and above 10 return in 70/80, including unseen lengths
- Context length 1 drops the score to 15.06, so history matters for body adaptation
- Less diverse data (8 bodies) hurts even with 10x more trajectories per body (15.32), body diversity matters more than data volume
- Real: on the default body EAT and the vanilla transformer walk while PPO fails from the sim-to-real gap, on asymmetric bodies only EAT walks
- BO with EAT solves the stair in about 10 generations, with PPO or vanilla transformer the optimizer gets noisy fitness and fails

## Interesting stuff

- Body diversity in the training set beats more data from fewer bodies, nice and simple result
- PPO failing on the real robot is probably a weak baseline (no history, no tuned sim-to-real), so part of the gain is just the transformer context
- The body is fixed during the episode, they say changing body shape inside an episode is impractical for now
- The description of the stair solution is contradictory in the text (front/hind tibia values swap between figure caption and paragraph)
- Only flat terrain in training, the stair is solved only by changing morphology, which is a cool demonstration of morphology as an action

## Useful for me

- **As prior work.** Very close to my work: one policy conditioned on limb lengths, deployed on a real legged robot whose limb lengths change. Must be in the related work.
- **Apply.** Distilling many per-body experts into one body-conditioned policy is an easy baseline before training a single RL policy over a continuous body range.
- **Apply.** Using the fixed body-aware policy inside a morphology optimizer (BO) is a clean way to choose the body for a task, the same can be done for a humanoid choosing its limb lengths.
- **Gap.** Body changes between episodes only. A humanoid that changes its limbs during the task needs a policy robust to the transition itself.

## Key prior work

- [[Decision Transformer - Reinforcement Learning via Sequence Modeling|Decision Transformer]] · the architecture they adapt
- [[Hardware Conditioned Policies for Multi-Robot Transfer Learning|Hardware Conditioned Policies]] · conditioning on a hardware vector
- [[GenLoco - Generalized Locomotion Controllers for Quadrupedal Robots|GenLoco]] · concurrent morphology randomization for quadrupeds
- [[Real-world embodied AI through a morphologically adaptive quadruped robot|DyRET in the wild]] · real-world morphology adaptation without learning
- [[Embodied intelligence via learning and evolution|DERL]] · the co-design via evolution view
- [[Jointly learning to construct and control agents using deep reinforcement learning|Joint construct and control]] · RL over a design distribution
- [[One Policy to Control Them All - Shared Modular Policies for Agent-Agnostic Control|SMP]] · modular policies across morphologies

## References

1. [[The Shape of Life - Genes, Development, and the Evolution of Animal Form|The Shape of Life]] · Raff · 2012 · University of Chicago Press · [Scholar](https://scholar.google.com/scholar?q=The+shape+of+life+genes+development+and+the+evolution+of+animal+form)
2. [[Legged Robots that Keep on Learning - Fine-Tuning Locomotion Policies in the Real World|Keep on learning]] · Smith et al. · 2022 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Legged+robots+that+keep+on+learning+Fine-tuning+locomotion+policies+in+the+real+world)
3. [[Learning to Climb - Constrained Contextual Bayesian Optimisation on a Multi-Modal Legged Robot|Learning to climb]] · Yu et al. · 2022 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Learning+to+climb+Constrained+contextual+bayesian+optimisation+on+a+multi-modal+legged+robot)
4. [[Learning to Walk in the Real World with Minimal Human Effort|Walk with minimal human effort]] · Ha et al. · 2020 · arXiv · [arXiv](https://arxiv.org/abs/2002.08550)
5. [[Combining Evolutionary and Adaptive Control Strategies for Quadruped Robotic Locomotion|Evolutionary and adaptive control]] · Massi et al. · 2019 · Frontiers in Neurorobotics · [Scholar](https://scholar.google.com/scholar?q=Combining+evolutionary+and+adaptive+control+strategies+for+quadruped+robotic+locomotion)
6. [[Real-World Robot Evolution - Why Would it (not) Work|Real-world robot evolution]] · Eiben · 2021 · Frontiers in Robotics and AI · [Scholar](https://scholar.google.com/scholar?q=Real-world+robot+evolution+why+would+it+not+work)
7. [[Real-world embodied AI through a morphologically adaptive quadruped robot|DyRET in the wild]] · Nygaard et al. · 2021 · Nature Machine Intelligence · [Scholar](https://scholar.google.com/scholar?q=Real-world+embodied+AI+through+a+morphologically+adaptive+quadruped+robot)
8. [[Environmental Adaptation of Robot Morphology and Control through Real-world Evolution|DyRET environmental adaptation]] · Nygaard et al. · 2021 · Evolutionary Computation · [Scholar](https://scholar.google.com/scholar?q=Environmental+adaptation+of+robot+morphology+and+control+through+real-world+evolution)
9. [[Real World Morphological Evolution is Feasible|Real-world morphological evolution]] · Nygaard et al. · 2020 · GECCO Companion · [Scholar](https://scholar.google.com/scholar?q=Real+world+morphological+evolution+is+feasible)
10. [[Real-World Evolution Adapts Robot Morphology and Control to Hardware Limitations|DyRET hardware limitations]] · Nygaard et al. · 2018 · GECCO · [Scholar](https://scholar.google.com/scholar?q=Real-world+evolution+adapts+robot+morphology+and+control+to+hardware+limitations)
11. [[The trade-off between morphology and control in the co-optimized design of robots|Morphology-control trade-off]] · Rosendo et al. · 2017 · PLOS ONE · [Scholar](https://scholar.google.com/scholar?q=The+trade-off+between+morphology+and+control+in+the+co-optimized+design+of+robots)
12. [[Evolutionary Developmental Robotics - Improving Morphology and Control of Physical Robots|Evo-devo robotics]] · Vujovic et al. · 2017 · Artificial Life · [Scholar](https://scholar.google.com/scholar?q=Evolutionary+developmental+robotics+Improving+morphology+and+control+of+physical+robots)
13. [[Artificial Metamorphosis - Evolutionary Design of Transforming, Soft-Bodied Robots|Artificial metamorphosis]] · Joachimczak et al. · 2016 · Artificial Life · [Scholar](https://scholar.google.com/scholar?q=Artificial+metamorphosis+Evolutionary+design+of+transforming+soft-bodied+robots)
14. [[Morpho-evolution with learning using a controller archive as an inheritance mechanism|Controller archive inheritance]] · Le Goff et al. · 2022 · IEEE TCDS · [Scholar](https://scholar.google.com/scholar?q=Morpho-evolution+with+learning+using+a+controller+archive+as+an+inheritance+mechanism)
15. [[On the challenges of jointly optimising robot morphology and control using a hierarchical optimisation scheme|Hierarchical co-optimisation challenges]] · Le Goff & Hart · 2021 · GECCO Companion · [Scholar](https://scholar.google.com/scholar?q=On+the+challenges+of+jointly+optimising+robot+morphology+and+control+using+a+hierarchical+optimisation+scheme)
16. [[RoboGrammar - Graph Grammar for Terrain-Optimized Robot Design|RoboGrammar]] · Zhao et al. · 2020 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=Robogrammar+graph+grammar+for+terrain-optimized+robot+design)
17. [[Data-efficient Co-Adaptation of Morphology and Behaviour with Deep Reinforcement Learning|Data-efficient co-adaptation]] · Luck et al. · 2020 · CoRL · [arXiv](https://arxiv.org/abs/1911.06832)
18. [[Lamarckian Evolution of Simulated Modular Robots|Lamarckian modular robots]] · Jelisavcic et al. · 2019 · Frontiers in Robotics and AI · [Scholar](https://scholar.google.com/scholar?q=Lamarckian+evolution+of+simulated+modular+robots)
19. [[Embodied intelligence via learning and evolution|DERL]] · Gupta et al. · 2021 · Nature Communications · [arXiv](https://arxiv.org/abs/2102.02202)
20. [[Morphologically induced stability on an underwater legged robot with a deformable body|Underwater legged robot]] · Picardi et al. · 2021 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Morphologically+induced+stability+on+an+underwater+legged+robot+with+a+deformable+body)
21. [[Dynamic Locomotion in the MIT Cheetah 3 Through Convex Model-Predictive Control|Cheetah 3 convex MPC]] · Di Carlo et al. · 2018 · IROS · [Scholar](https://scholar.google.com/scholar?q=Dynamic+locomotion+in+the+mit+cheetah+3+through+convex+model-predictive+control)
22. [[Adaptive CLF-MPC With Application to Quadrupedal Robots|Adaptive CLF-MPC]] · Minniti et al. · 2021 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Adaptive+clf-mpc+with+application+to+quadrupedal+robots)
23. [[Feedback MPC for Torque-Controlled Legged Robots|Feedback MPC]] · Grandia et al. · 2019 · IROS · [Scholar](https://scholar.google.com/scholar?q=Feedback+mpc+for+torque-controlled+legged+robots)
24. [[System identification and modeling for MABEL, a bipedal robot with a cable-differential-based compliant drivetrain|MABEL system ID]] · Park et al. · 2010 · Dynamic Walking · [Scholar](https://scholar.google.com/scholar?q=System+identification+and+modeling+for+mabel+a+bipedal+robot+with+a+cable-differential-based+compliant+drivetrain)
25. [[State transition, balancing, station keeping, and yaw control for a dynamically stable single spherical wheel mobile robot|Ballbot control]] · Nagarajan et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=State+transition+balancing+station+keeping+and+yaw+control+for+a+dynamically+stable+single+spherical+wheel+mobile+robot)
26. [[Learning quadrupedal locomotion over challenging terrain|Challenging terrain locomotion]] · Lee et al. · 2020 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+quadrupedal+locomotion+over+challenging+terrain)
27. [[RMA - Rapid Motor Adaptation for Legged Robots|RMA]] · Kumar et al. · 2021 · RSS · [arXiv](https://arxiv.org/abs/2107.04034)
28. [[Learning robust perceptive locomotion for quadrupedal robots in the wild|Perceptive locomotion in the wild]] · Miki et al. · 2022 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+robust+perceptive+locomotion+for+quadrupedal+robots+in+the+wild)
29. [[Hardware Conditioned Policies for Multi-Robot Transfer Learning|Hardware Conditioned Policies]] · Chen et al. · 2018 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Hardware+Conditioned+Policies+for+Multi-Robot+Transfer+Learning)
30. [[Jointly learning to construct and control agents using deep reinforcement learning|Joint construct and control]] · Schaff et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Jointly+learning+to+construct+and+control+agents+using+deep+reinforcement+learning)
31. [[One Policy to Control Them All - Shared Modular Policies for Agent-Agnostic Control|SMP]] · Huang et al. · 2020 · ICML · [arXiv](https://arxiv.org/abs/2007.04976)
32. [[Learning to Control Self-Assembling Morphologies - A Study of Generalization via Modularity|Self-assembling morphologies]] · Pathak et al. · 2019 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Learning+to+control+self-assembling+morphologies+a+study+of+generalization+via+modularity)
33. [[NerveNet - Learning Structured Policy with Graph Neural Networks|NerveNet]] · Wang et al. · 2018 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Nervenet+Learning+structured+policy+with+graph+neural+networks)
34. [[GenLoco - Generalized Locomotion Controllers for Quadrupedal Robots|GenLoco]] · Feng et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2209.05309)
35. [[Training Agents using Upside-Down Reinforcement Learning|Upside-down RL]] · Srivastava et al. · 2019 · arXiv · [arXiv](https://arxiv.org/abs/1912.02877)
36. [[A Generalist Agent|Gato]] · Reed et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2205.06175)
37. [[Decision Transformer - Reinforcement Learning via Sequence Modeling|Decision Transformer]] · Chen et al. · 2021 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Decision+transformer+Reinforcement+learning+via+sequence+modeling)
38. [[Can Wikipedia Help Offline Reinforcement Learning|Wikipedia for offline RL]] · Reid et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2201.12122)
39. [[Online Decision Transformer|Online Decision Transformer]] · Zheng et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2202.05607)
40. [[Offline Reinforcement Learning as One Big Sequence Modeling Problem|Trajectory Transformer]] · Janner et al. · 2021 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Offline+reinforcement+learning+as+one+big+sequence+modeling+problem)
41. [[Speech understanding systems - A summary of results of the five-year research effort at Carnegie Mellon University|Beam search origin]] · Reddy · 1977 · CMU tech report · [Scholar](https://scholar.google.com/scholar?q=Speech+understanding+systems+summary+of+results+of+the+five-year+research+effort+at+carnegie+mellon+university)
42. [[Mini Cheetah - A Platform for Pushing the Limits of Dynamic Quadruped Control|Mini Cheetah]] · Katz et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Mini+cheetah+A+platform+for+pushing+the+limits+of+dynamic+quadruped+control)
43. [[Practical Bayesian Optimization of Machine Learning Algorithms|Practical BO]] · Snoek et al. · 2012 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Practical+bayesian+optimization+of+machine+learning+algorithms)
44. [[Attention Is All You Need|Transformer]] · Vaswani et al. · 2017 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Attention+Is+All+You+Need)
45. [[Constituency Parsing with a Self-Attentive Encoder|Self-attentive parser]] · Kitaev & Klein · 2018 · arXiv · [arXiv](https://arxiv.org/abs/1805.01052)
46. [[Generating Wikipedia by Summarizing Long Sequences|Wikipedia generation]] · Liu et al. · 2018 · arXiv · [arXiv](https://arxiv.org/abs/1801.10198)
47. [[Video Swin Transformer|Video Swin]] · Liu et al. · 2022 · CVPR · [Scholar](https://scholar.google.com/scholar?q=Video+swin+transformer)
48. [[TrackFormer - Multi-Object Tracking with Transformers|TrackFormer]] · Meinhardt et al. · 2022 · CVPR · [Scholar](https://scholar.google.com/scholar?q=Trackformer+Multi-object+tracking+with+transformers)
49. [[Tree-Based Batch Mode Reinforcement Learning|Fitted Q iteration]] · Ernst et al. · 2005 · JMLR · [Scholar](https://scholar.google.com/scholar?q=Tree-based+batch+mode+reinforcement+learning)
50. [[Off-Policy Deep Reinforcement Learning without Exploration|BCQ]] · Fujimoto et al. · 2019 · ICML · [Scholar](https://scholar.google.com/scholar?q=Off-policy+deep+reinforcement+learning+without+exploration)
51. [[Stabilizing Off-Policy Q-Learning via Bootstrapping Error Reduction|BEAR]] · Kumar et al. · 2019 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Stabilizing+off-policy+q-learning+via+bootstrapping+error+reduction)
52. [[Layer Normalization|Layer norm]] · Ba et al. · 2016 · arXiv · [arXiv](https://arxiv.org/abs/1607.06450)
53. [[Improving Language Understanding by Generative Pre-Training|GPT]] · Radford et al. · 2018 · OpenAI report · [Scholar](https://scholar.google.com/scholar?q=Improving+language+understanding+by+generative+pre-training)
54. [[Learning to Walk in Minutes Using Massively Parallel Deep Reinforcement Learning|legged_gym]] · Rudin et al. · 2022 · CoRL · [arXiv](https://arxiv.org/abs/2109.11978)
55. [[Isaac Gym - High Performance GPU-Based Physics Simulation For Robot Learning|Isaac Gym]] · Makoviychuk et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2108.10470)
56. [[Sim-to-Real - Learning Agile Locomotion For Quadruped Robots|Sim-to-real agile locomotion]] · Tan et al. · 2018 · arXiv · [arXiv](https://arxiv.org/abs/1804.10332)
57. [[Introduction to Evolvable Hardware - A Practical Guide for Designing Self-Adaptive Systems|Evolvable hardware]] · Greenwood & Tyrrell · 2006 · Wiley · [Scholar](https://scholar.google.com/scholar?q=Introduction+to+evolvable+hardware+a+practical+guide+for+designing+self-adaptive+systems)
58. [[Reinforcement Learning With Evolutionary Trajectory Generator - A General Approach for Quadrupedal Locomotion|RL with ETG]] · Shi et al. · 2022 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+learning+with+evolutionary+trajectory+generator+A+general+approach+for+quadrupedal+locomotion)
59. [[Blind Bipedal Stair Traversal via Sim-to-Real Reinforcement Learning|Blind stair traversal]] · Siekmann et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2105.08328)

## See also

- [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · online RL alternative for multi-embodiment locomotion
- [[Self-Modifying Morphology Experiments with DyRET - Dynamic Robot for Embodied Testing|DyRET self-modifying]] · a quadruped that changes its leg length by itself
