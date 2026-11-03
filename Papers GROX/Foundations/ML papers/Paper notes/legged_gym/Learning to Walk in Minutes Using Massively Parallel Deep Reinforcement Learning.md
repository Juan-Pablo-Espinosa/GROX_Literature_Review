---
title: "Learning to Walk in Minutes Using Massively Parallel Deep Reinforcement Learning"
aliases:
  - legged_gym
  - Learning to Walk in Minutes
authors:
  - "[[Nikita Rudin]]"
  - "[[David Hoeller]]"
  - "[[Philipp Reist]]"
  - "[[Marco Hutter]]"
corresponding: ""
institutions:
  - "[[ETH Zurich]]"
  - "[[NVIDIA]]"
labs:
  - "[[Robotic Systems Lab]]"
countries:
  - Switzerland
  - USA
year: 2022
date: ""
venue: CoRL 2021 (PMLR 164, 2022)
venue_type: conference
arxiv_category: cs.RO
arxiv_id: "2109.11978"
url: https://arxiv.org/abs/2109.11978
pdf: https://arxiv.org/pdf/2109.11978v3
code: https://leggedrobotics.github.io/legged_gym/
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/legged-locomotion
  - topic/sim-to-real
  - topic/domain-randomization
  - method/reinforcement-learning
  - method/ppo
  - method/curriculum-learning
  - sim/isaac-gym
  - robot/anymal
  - robot/cassie
  - eval/real-robot
---

# legged_gym

> [!info] Quick view
> **Authors:** [[Nikita Rudin]], [[David Hoeller]], [[Philipp Reist]], [[Marco Hutter]]
> **From:** [[ETH Zurich]] · [[Robotic Systems Lab]] · [[NVIDIA]]
> **Published:** CoRL 2021 · arXiv Sep 2021 · cs.RO
> **Keywords:** massively parallel RL, Isaac Gym, PPO on GPU, game-inspired terrain curriculum, time-out bootstrapping, ANYmal, sim-to-real
> **Links:** [arXiv](https://arxiv.org/abs/2109.11978) · [PDF](https://arxiv.org/pdf/2109.11978v3) · [Code](https://leggedrobotics.github.io/legged_gym/)
> **One line:** thousands of robots simulated on one GPU with an end-to-end GPU PPO pipeline train a rough-terrain ANYmal policy in under 20 minutes that transfers to the real robot

## Summary

This is the paper behind legged_gym and the base recipe of most locomotion RL today. Everything (simulation in Isaac Gym, observations, rewards, PPO) runs on the GPU, so they can run thousands of robots in parallel. They study what changes in PPO when you have, for example, 4096 robots: the batch is robots × steps, so steps per robot must be small, but below about 25 steps (0.5 s) GAE stops working. Very large mini-batches (tens of thousands) help. Time-out resets must be handled by bootstrapping the critic with its own value, which most Gym implementations ignore and gives 10 to 20% more reward.

They also propose a game-inspired curriculum: terrains (flat, slopes, rough, stairs, obstacles) are tiled in rows of increasing difficulty, a robot that walks out of its tile moves up a level, one that covers less than half the commanded distance moves down, and robots that solve the top level loop back to random levels. With 4096 robots and batch 98,304, a perceptive ANYmal C policy trains in under 20 minutes on one GPU (flat terrain under 4 minutes), climbs 0.2 m stairs in simulation, and transfers to the real robot with friction randomization, pushes, observation noise and an LSTM actuator network. The same setup trains ANYmal B, ANYmal with an arm, Unitree A1 and Cassie. Honestly the paper says it does not aim for the most robust policy, and on hardware the elevation map noise limits speed to 0.6 m/s, but the training recipe changed the field.

## How it works

- Single Isaac Gym simulation with all robots, all terrains in one mesh, robots moved between tiles to change difficulty
- PPO fully on GPU: batch 98,304 (4096 × 24 steps), mini-batch 24,576, 5 epochs, adaptive learning rate from KL
- Time-out bootstrapping: critic target uses its own prediction when an episode ends by time limit
- Observations: base linear and angular velocity, gravity, joint positions and velocities, last action, 108 terrain height samples
- Rewards: velocity tracking plus penalties on z velocity, roll/pitch rate, joint acceleration and torque, action rate, collisions, and a feet air time reward, no gait prior
- Sim-to-real: ground friction 0.5 to 1.25, pushes every 10 s, measured observation noise, LSTM actuator network for ANYmal's SEAs
- Policy at 50 Hz, simulation dt 0.005 s

## Results that matter

- Flat terrain in under 4 minutes, rough terrain in under 20 minutes on one RTX A6000
- Best trade-off 2048 to 4096 robots with batch about 100k to 200k, too many robots means too few steps per robot and performance drops sharply
- Time-out bootstrapping improves total reward about 10 to 20%
- Sim success near 100% on 0.2 m stairs, slopes fail above 25°
- Real ANYmal C climbs stairs and obstacles at up to 0.6 m/s
- Same setup trains ANYmal B, ANYmal with arm (20% heavier), A1 and Cassie with minor changes

## Interesting stuff

- The curriculum is simple, needs no tuning, and the robot distribution over levels doubles as a progress monitor
- Policies always converge to trotting but with artifacts (dragging legs, odd heights) that need reward tuning
- The time-out bootstrapping detail is something many implementations still get wrong
- Robustness on hardware is lower than teacher-student methods, they mention merging the two

## Useful for me

- **Apply.** This is my default training stack: thousands of parallel envs, GPU PPO, terrain curriculum. For a morphing humanoid, the same tiled curriculum idea could be used with body configurations as an extra curriculum axis.
- **Apply.** Keep at least about 25 steps per env per update and handle time-outs with bootstrapping, especially when episodes are long because the body changes during the episode.
- **As prior work.** Standard citation for the training pipeline and the curriculum.

## Key prior work

- [[Learning agile and dynamic motor skills for legged robots|ANYmal agile skills]] · actuator network for sim-to-real
- [[Learning quadrupedal locomotion over challenging terrain|Challenging terrain locomotion]] · the curriculum they replace
- [[Isaac Gym - High Performance GPU-Based Physics Simulation For Robot Learning|Isaac Gym]] · the simulator
- [[Proximal Policy Optimization Algorithms|PPO]] · the RL algorithm
- [[High-Dimensional Continuous Control Using Generalized Advantage Estimation|GAE]] · why short rollouts fail
- [[Time Limits in Reinforcement Learning|Time limits in RL]] · the time-out bootstrapping idea

## References

1. [[Learning agile and dynamic motor skills for legged robots|ANYmal agile skills]] · Hwangbo et al. · 2019 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+agile+and+dynamic+motor+skills+for+legged+robots)
2. [[Deep reinforcement learning for robotic manipulation with asynchronous off-policy updates|Asynchronous off-policy manipulation]] · Gu et al. · 2017 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Deep+reinforcement+learning+for+robotic+manipulation+with+asynchronous+off-policy+updates)
3. [[Self-supervised deep reinforcement learning with generalized computation graphs for robot navigation|Generalized computation graphs]] · Kahn et al. · 2018 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Self-supervised+deep+reinforcement+learning+with+generalized+computation+graphs+for+robot+navigation)
4. [[Solving Rubik's Cube with a Robot Hand|Rubik's cube hand]] · OpenAI et al. · 2019 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Solving+Rubik%27s+Cube+with+a+Robot+Hand)
5. [[MuJoCo - A physics engine for model-based control|MuJoCo]] · Todorov et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=MuJoCo+A+physics+engine+for+model-based+control)
6. [[PyBullet, a Python module for physics simulation for games, robotics and machine learning|PyBullet]] · Coumans & Bai · 2016-2021 · software · [Link](http://pybullet.org)
7. [[Per-Contact Iteration Method for Solving Contact Dynamics|RaiSim contact method]] · Hwangbo et al. · 2018 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Per-contact+iteration+method+for+solving+contact+dynamics)
8. [[Isaac Gym - High Performance GPU-Based Physics Simulation For Robot Learning|Isaac Gym]] · Makoviychuk et al. · 2021 · NeurIPS Datasets and Benchmarks · [arXiv](https://arxiv.org/abs/2108.10470)
9. [[Emergence of Locomotion Behaviours in Rich Environments|Emergence of locomotion]] · Heess et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.02286)
10. [[Accelerated Methods for Deep Reinforcement Learning|Accelerated methods for DRL]] · Stooke & Abbeel · 2018 · arXiv · [arXiv](https://arxiv.org/abs/1803.02811)
11. [[Large Batch Simulation for Deep Reinforcement Learning|Large batch simulation]] · Shacklett et al. · 2021 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Large+batch+simulation+for+deep+reinforcement+learning)
12. [[GPU-Accelerated Robotic Simulation for Distributed Reinforcement Learning|GPU-accelerated simulation]] · Liang et al. · 2018 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Gpu-accelerated+robotic+simulation+for+distributed+reinforcement+learning)
13. [[Brax - A Differentiable Physics Engine for Large Scale Rigid Body Simulation|Brax]] · Freeman et al. · 2021 · NeurIPS Datasets and Benchmarks · [Scholar](https://scholar.google.com/scholar?q=Brax+a+differentiable+physics+engine+for+large+scale+rigid+body+simulation)
14. [[Autonomous Spot - Long-Range Autonomous Exploration of Extreme Environments with Legged Locomotion|Autonomous Spot]] · Bouman et al. · 2020 · IROS · [Scholar](https://scholar.google.com/scholar?q=Autonomous+spot+Long-range+autonomous+exploration+of+extreme+environments+with+legged+locomotion)
15. [[ANYmal in the Field - Solving Industrial Inspection of an Offshore HVDC Platform with a Quadrupedal Robot|ANYmal in the field]] · Gehring et al. · 2021 · Field and Service Robotics · [Scholar](https://scholar.google.com/scholar?q=Anymal+in+the+field+Solving+industrial+inspection+of+an+offshore+hvdc+platform+with+a+quadrupedal+robot)
16. [[Learning quadrupedal locomotion over challenging terrain|Challenging terrain locomotion]] · Lee et al. · 2020 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+quadrupedal+locomotion+over+challenging+terrain)
17. [[DeepGait - Planning and Control of Quadrupedal Gaits using Deep Reinforcement Learning|DeepGait]] · Tsounis et al. · 2020 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Deepgait+Planning+and+control+of+quadrupedal+gaits+using+deep+reinforcement+learning)
18. [[Real-Time Trajectory Adaptation for Quadrupedal Locomotion using Deep Reinforcement Learning|Real-time trajectory adaptation]] · Gangapurwala et al. · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Real-time+trajectory+adaptation+for+quadrupedal+locomotion+using+deep+reinforcement+learning)
19. [[Learning robust perceptive locomotion for quadrupedal robots in the wild|Perceptive locomotion in the wild]] · Miki et al. · 2021 (submitted, as printed) · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+robust+perceptive+locomotion+for+quadrupedal+robots+in+the+wild)
20. [[Blind Bipedal Stair Traversal via Sim-to-Real Reinforcement Learning|Blind stair traversal]] · Siekmann et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2105.08328)
21. [[Where is the data? Why you cannot debate CPU vs. GPU performance without the answer|Where is the data]] · Gregg & Hazelwood · 2011 · ISPASS · [Scholar](https://scholar.google.com/scholar?q=Where+is+the+data+why+you+cannot+debate+cpu+vs+gpu+performance+without+the+answer)
22. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
23. [[High-Dimensional Continuous Control Using Generalized Advantage Estimation|GAE]] · Schulman et al. · 2016 · ICLR · [Scholar](https://scholar.google.com/scholar?q=High-dimensional+continuous+control+using+generalized+advantage+estimation)
24. [[Time Limits in Reinforcement Learning|Time limits in RL]] · Pardo et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1712.00378)
25. [[OpenAI Gym|OpenAI Gym]] · Brockman et al. · 2016 · arXiv · [Scholar](https://scholar.google.com/scholar?q=OpenAI+Gym+Brockman)
26. [[Stable Baselines|Stable Baselines]] · Hill et al. · 2018 · software · [GitHub](https://github.com/hill-a/stable-baselines)
27. [[Spinning Up in Deep Reinforcement Learning|Spinning Up]] · Achiam · 2018 · documentation · [Link](https://spinningup.openai.com/en/latest/)
28. [[Paired Open-Ended Trailblazer (POET) - Endlessly Generating Increasingly Complex and Diverse Learning Environments and Their Solutions|POET]] · Wang et al. · 2019 · arXiv · [arXiv](https://arxiv.org/abs/1901.01753)
29. [[ALLSTEPS - Curriculum-driven Learning of Stepping Stone Skills|ALLSTEPS]] · Xie et al. · 2020 · SCA · [Scholar](https://scholar.google.com/scholar?q=Allsteps+Curriculum-driven+learning+of+stepping+stone+skills)
30. [[Automatic Goal Generation for Reinforcement Learning Agents|Automatic goal generation]] · Florensa et al. · 2018 · ICML · [Scholar](https://scholar.google.com/scholar?q=Automatic+goal+generation+for+reinforcement+learning+agents)

## See also

- [[Berkeley Humanoid - A Research Platform for Learning-based Control|Berkeley Humanoid]] · applies this recipe to a humanoid with minimal randomization
- [[Multi-Embodiment Locomotion at Scale with extreme Embodiment Randomization|URMAv2]] · scales parallel training across 50 robot bodies
