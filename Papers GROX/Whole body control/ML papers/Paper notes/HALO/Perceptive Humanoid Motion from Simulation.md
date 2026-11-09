---
title: "Perceptive Humanoid Motion from Simulation"
aliases:
  - HALO
authors:
  - "[[Siyuan Hu]]"
  - "[[Abulikemu Abuduweili]]"
  - "[[Yue Meng]]"
  - "[[Shipeng Liu]]"
  - "[[Quentin Leboutet]]"
  - "[[Chen Chen]]"
  - "[[Matthias Müller]]"
  - "[[Vladlen Koltun]]"
corresponding: ""
institutions:
  - "[[Apple]]"
labs: []
countries:
  - USA
year: 2026
date: 2026-11-04
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2611.06908"
url: https://arxiv.org/abs/2611.06908
pdf: https://arxiv.org/pdf/2611.06908
code: "https://apple-aiml-research.github.io/ml-halo/ (project page says code is released there, repo URL not checked directly)"
project_page: https://apple-aiml-research.github.io/ml-halo/
status: to-read
relevance: high
added: 2026-11-08
tags:
  - paper
  - topic/humanoids
  - topic/whole-body-control
  - topic/motion-tracking
  - topic/loco-manipulation
  - topic/perception
  - topic/sim-to-real
  - topic/foundation-models
  - topic/synthetic-data
  - method/reinforcement-learning
  - method/distillation
  - method/flow-matching
  - robot/unitree-g1
  - sim/isaac-lab
  - eval/real-robot
---

# HALO

> [!info] Quick view
> **Authors:** [[Siyuan Hu]], [[Abulikemu Abuduweili]], [[Yue Meng]], [[Shipeng Liu]], [[Quentin Leboutet]], [[Chen Chen]], [[Matthias Müller]], [[Vladlen Koltun]]
> **From:** [[Apple]]
> **Published:** arXiv preprint · Nov 2026 · cs.RO
> **Keywords:** synthetic vision-language-motion data, procedural homes, motion diffusion, flow matching DiT, perceptive motion tracker, multi-critic PPO, VLM orchestrator, zero-shot sim-to-real
> **Links:** [arXiv](https://arxiv.org/abs/2611.06908) · [PDF](https://arxiv.org/pdf/2611.06908) · [Project page](https://apple-aiml-research.github.io/ml-halo/)
> **One line:** generate all the vision-language-motion data in 1,000 procedural homes, train a VLM + flow matching motion generator and a depth-conditioned tracker on it, and a G1 sits, crawls, crouches, pushes and walks through doors in real unseen homes at 91% success

## Summary

The paper attacks the data bottleneck for humanoid autonomy. To map pixels and language to joint commands you need paired data (egocentric image, instruction, whole-body trajectory) and teleoperation costs at least one hour of human time per hour of data. HALO makes all of it in simulation instead. A kinematic motion diffusion model (Kimodo) generates whole-body motion from text, waypoints and keyframes, Infinigen generates 1,000 rooms, the motion gets placed collision-free in a room, and the egocentric camera is rendered with baked lighting. The result is about 296 h and 162k episodes over six skills, made with no human in the loop besides writing the task recipes.

The stack on the robot has three layers: a VLM orchestrator (~0.5 Hz) that splits a request into subtasks, a motion generator (~3 Hz, frozen Qwen3.5-2B encoder + flow matching DiT decoder) that outputs 5 s of whole-body motion from the egocentric RGB, and a 50 Hz depth-conditioned tracker trained with RL. Everything runs onboard a G1 with a Mac mini on its back. They get 91% over 360 real trials in six unscanned environments. Honestly this is a very complete systems paper and the appendix is full of practical sim-to-real details, but there is no head to head comparison with any other method, only feature tables and internal ablations.

## How it works

- **Data generation.** Each task is a hand written chain of Kimodo calls (for sit: walk with waypoints, turn, sit into a keyframe taken from a real G1 joint log). Candidates are filtered with forward kinematics checks (foot skating, floor penetration, flight phases, joint limits) and the best one is kept
- **Scene staging.** Motion + target object are placed in an Infinigen room by searching SE(2) transforms on a 10 cm occupancy grid until the pelvis stays 0.35 m away from obstacles. Every episode is just a seed + placement record, so they can re-render it with another camera or simulator without regenerating motion
- **Rendering.** Path tracing is too slow (132 s per frame), so they bake lighting into emissive textures once per room and then render in Isaac Sim with a fisheye model matching the real OAK-1W camera, plus image randomization (exposure, tint, blur, noise)
- **Motion generator.** Frozen Qwen3.5-2B reads a 224×224 image + command, a 49.3M param DiT decoder (8 blocks, width 512) does flow matching over 150 frames × 36 values (29 joints + pelvis translation + quaternion), 15 Euler steps at inference
- **Asynchronous generation.** The generator runs while the tracker executes. The first 15 frames (0.5 s) the robot is already committed to are given clean as a prefix during training, so the model learns to continue a plan without jumps (training-time RTC instead of inference-time inpainting)
- **Tracker phase 1.** Separate PPO experts per skill family with privileged state and a 17×11 local height map, trained in Isaac Lab
- **Tracker phase 2 and 3.** DAgger distillation (20% teacher rollouts) into one student that only sees proprioception, IMU, the plan and a 48×84 depth image, then multi-critic PPO fine-tuning with depth in the loop, one critic per skill with separately normalized advantages so the object skills dont dominate the gradient
- **Sim-to-real.** Sys-ID of stiffness, damping, armature, torque limits and delay from open-loop replays, PD gains from rotor inertia (ωn = 10 Hz), knee braking power penalty, two-tier domain randomization, contact compliance randomization for carpets. Real depth comes from raw stereo IR through LiteAnyStereo on the Jetson instead of the noisy RealSense output

## Results that matter

- **Single task.** Mean 91% in sim (6,000 trials in held-out rooms) and 91% real (360 trials, 6 environments never scanned). Real per task: Go-To 92, Sit 85, Door 93, Crouch 100, Crawl 90, Push 88
- **Multi task is weaker.** Mean 69% sim and 70% real, with Crouch→Go-To→Push at 50% real and Crouch→Sit at 60%. Real multi task is only 1 environment × 10 trials per chain, so these numbers are noisy. Failures come from the robot ending a subtask with the next target out of view
- **Scene diversity matters a lot.** Sit success goes 43% (1 room) → 64% (10) → 81% (100) → 84% (1,000). Most of the gain is before 100 rooms
- **Plan continuity is critical.** Without RTC 24%, inference-time RTC 66%, training-time conditioning 84%. A regression decoder instead of flow matching drops to 19%
- **Depth in the tracker.** Removing depth only drops Sit from 84% to 74%, so the perceptive tracker helps but the generator does most of the work
- **Sys-ID.** Fitting the actuator model cuts joint tracking error on held-out motions by 62% (26.9% to 10.1%). The real motors only give 78 to 90% of the commanded stiffness with 25 to 30 ms delay

## Interesting stuff

- The frozen VLM generalizes better than a fine-tuned one: trained with one human model, the frozen version still follows many different real people, the fine-tuned one only recognizes people who look like the training model. Fine-tuning also learns the shortcut "chair in view means sit", they fix it by blacking out the image in 30% of samples
- Action-rate penalty predicts real-world stability much better than sim reward, and without it plus latency randomization the ankles and wrists vibrate audibly. Very useful practical note
- Rigid-floor policies shuffle their feet on thick carpet because sinking 1 to 2 cm looks like a tilt. Randomizing ground stiffness and damping fixes it
- **Weak point.** No comparison against any other system on the same tasks. Table I is just checkmarks, and all ablations are on Sit only, in sim
- **Weak point.** Data generation is "no human labor" but each new skill still needs a person to write prompts and keyframes, and the compute is huge (48 h on 250 H100 for data, 64 B200 for the generator). Small labs cant reproduce this scale

## Useful for me

- **Apply.** The tracker recipe is the most transferable part: privileged experts per skill, DAgger into a depth student, then multi-critic PPO. Plus the practical sim-to-real list (rotor-inertia PD gains, sys-ID of armature and delay, knee braking penalty, ground compliance randomization, action-rate penalty)
- **Apply.** The prefix-conditioned flow matching trick for streaming plans to a tracker without jumps. Any kinematic planner feeding a low-level policy has the same seam problem
- **Gap.** Everything is built around one fixed G1 body, the motion generator, Kimodo keyframes and tracker all assume it. A humanoid that changes its morphology would need the generated motion and the tracker conditioned on the current body, and nothing here addresses that
- **As prior work.** Good reference for the argument that procedurally generated environments plus synthetic data can replace teleoperation for whole-body skills, and the room scaling ablation is a nice number to cite
- **Watch out.** 91% looks great but the real multi task evaluation is tiny and there are no baselines, so I would not cite it as proof that this beats teleop-based methods

## Key prior work

- [[Kimodo - Scaling Controllable Human Motion Generation|Kimodo]] · the motion diffusion model that generates all the training motion, the whole pipeline depends on it
- [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · source of the tracking reward structure and sys-ID approach
- [[OmniXtreme - Breaking the Generality Barrier in High-Dynamic Humanoid Control|OmniXtreme]] · actuator modeling, PD gains from rotor inertia and the braking power penalty come from here
- [[RPL - Learning Robust Humanoid Perceptive Locomotion on Challenging Terrains|RPL]] · depth rendering with self-occlusion, height maps relative to the feet and depth noise models
- [[VLK - Learning Humanoid Loco-Manipulation from Synthetic Interactions in Reconstructed Scenes|VLK]] · closest competitor but trains in scanned rooms, HALO's main argument is against this
- [[TANGO - Humanoid Navigation in Cluttered Environments with a Whole-Body Vision-Language-Action Model|TANGO]] · other generated-plan + blind tracker system, tested in 3 rooms
- [[Training-Time Action Conditioning for Efficient Real-Time Chunking|Training-Time RTC]] · the prefix conditioning idea that makes asynchronous plans smooth
- [[Infinigen Indoors - Photorealistic Indoor Scenes using Procedural Generation|Infinigen Indoors]] · the procedural room generator behind the 1,000 homes

## References

1. [[Language Models are Few-Shot Learners|GPT-3]] · Brown et al. · 2020 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Language+Models+are+Few-Shot+Learners)
2. [[Learning Fine-Grained Bimanual Manipulation with Low-Cost Hardware|ALOHA]] · Zhao et al. · 2023 · RSS · [Scholar](https://scholar.google.com/scholar?q=Learning+Fine-Grained+Bimanual+Manipulation+with+Low-Cost+Hardware)
3. [[TWIST - Teleoperated Whole-Body Imitation System|TWIST]] · Ze et al. · 2025 · CoRL · [Scholar](https://scholar.google.com/scholar?q=TWIST+Teleoperated+Whole-Body+Imitation+System)
4. [[Universal Manipulation Interface - In-The-Wild Robot Teaching Without In-The-Wild Robots|UMI]] · Chi et al. · 2024 · RSS · [Scholar](https://scholar.google.com/scholar?q=Universal+Manipulation+Interface+In-The-Wild+Robot+Teaching+Without+In-The-Wild+Robots)
5. [[AMP - Adversarial Motion Priors for Stylized Physics-Based Character Control|AMP]] · Peng et al. · 2021 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=AMP+Adversarial+Motion+Priors+for+Stylized+Physics-Based+Character+Control)
6. [[ASAP - Aligning Simulation and Real-World Physics for Learning Agile Humanoid Whole-Body Skills|ASAP]] · He et al. · 2025 · RSS · [Scholar](https://scholar.google.com/scholar?q=ASAP+Aligning+Simulation+and+Real-World+Physics+for+Learning+Agile+Humanoid+Whole-Body+Skills)
7. [[OmniRetarget - Interaction-Preserving Data Generation for Humanoid Whole-Body Loco-Manipulation and Scene Interaction|OmniRetarget]] · Yang et al. · 2026 · ICRA · [Scholar](https://scholar.google.com/scholar?q=OmniRetarget+Interaction-Preserving+Data+Generation+for+Humanoid+Whole-Body+Loco-Manipulation+and+Scene+Interaction)
8. [[HDMI - Learning Interactive Humanoid Whole-Body Control from Human Videos|HDMI]] · Weng et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.16757)
9. [[Kimodo - Scaling Controllable Human Motion Generation|Kimodo]] · Rempe et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2603.15546)
10. [[PhysDiff - Physics-Guided Human Motion Diffusion Model|PhysDiff]] · Yuan et al. · 2023 · ICCV · [Scholar](https://scholar.google.com/scholar?q=PhysDiff+Physics-Guided+Human+Motion+Diffusion+Model)
11. [[TANGO - Humanoid Navigation in Cluttered Environments with a Whole-Body Vision-Language-Action Model|TANGO]] · Li et al. · 2026 · RSS Workshops · [Scholar](https://scholar.google.com/scholar?q=TANGO+Humanoid+Navigation+in+Cluttered+Environments+with+a+Whole-Body+Vision-Language-Action+Model)
12. [[PARC - Physics-based Augmentation with Reinforcement Learning for Character Controllers|PARC]] · Xu et al. · 2025 · SIGGRAPH · [Scholar](https://scholar.google.com/scholar?q=PARC+Physics-based+Augmentation+with+Reinforcement+Learning+for+Character+Controllers)
13. [[LEGS - Fine-tuning Teleop-free VLAs for Humanoid Loco-manipulation in an Embodied Gaussian Splatting World|LEGS]] · Kim et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2606.01458)
14. [[Phone2Proc - Bringing Robust Robots Into Our Chaotic World|Phone2Proc]] · Deitke et al. · 2023 · CVPR · [Scholar](https://scholar.google.com/scholar?q=Phone2Proc+Bringing+Robust+Robots+Into+Our+Chaotic+World)
15. [[VLK - Learning Humanoid Loco-Manipulation from Synthetic Interactions in Reconstructed Scenes|VLK]] · Wang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2606.30645)
16. [[ProcTHOR - Large-Scale Embodied AI Using Procedural Generation|ProcTHOR]] · Deitke et al. · 2022 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=ProcTHOR+Large-Scale+Embodied+AI+Using+Procedural+Generation)
17. [[Infinigen Indoors - Photorealistic Indoor Scenes using Procedural Generation|Infinigen Indoors]] · Raistrick et al. · 2024 · CVPR · [Scholar](https://scholar.google.com/scholar?q=Infinigen+Indoors+Photorealistic+Indoor+Scenes+using+Procedural+Generation)
18. [[GRAIL - Generating Humanoid Loco-Manipulation from 3D Assets and Video Priors|GRAIL]] · Xie et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2606.05160)
19. [[FetchMan - Learning Visual Humanoid Loco-Manipulation Policies from Simulated Experiences|FetchMan]] · Rayyan et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2608.17027)
20. [[ZeroWBC - Learning Natural Whole-Body Humanoid Interaction from Human Egocentric Data|ZeroWBC]] · Yang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2603.09170)
21. [[HumanVLA - Towards Vision-Language Directed Object Rearrangement by Physical Humanoid|HumanVLA]] · Xu et al. · 2024 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=HumanVLA+Towards+Vision-Language+Directed+Object+Rearrangement+by+Physical+Humanoid)
22. [[VIRAL - Visual Sim-to-Real at Scale for Humanoid Loco-Manipulation|VIRAL]] · He et al. · 2026 · CVPR · [Scholar](https://scholar.google.com/scholar?q=VIRAL+Visual+Sim-to-Real+at+Scale+for+Humanoid+Loco-Manipulation)
23. [[DreamControl - Human-Inspired Whole-Body Humanoid Control for Scene Interaction via Guided Diffusion|DreamControl]] · Kalaria et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.14353)
24. [[HumanoidMimicGen - Data Generation for Loco-Manipulation via Whole-Body Planning|HumanoidMimicGen]] · Lin et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2605.27724)
25. [[VisualMimic - Visual Humanoid Loco-Manipulation via Motion Tracking and Generation|VisualMimic]] · Yin et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.20322)
26. [[LeVERB - Humanoid Whole-Body Control with Latent Vision-Language Instruction|LeVERB]] · Xue et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2506.13751)
27. [[OmniH2O - Universal and Dexterous Human-to-Humanoid Whole-Body Teleoperation and Learning|OmniH2O]] · He et al. · 2024 · CoRL · [Scholar](https://scholar.google.com/scholar?q=OmniH2O+Universal+and+Dexterous+Human-to-Humanoid+Whole-Body+Teleoperation+and+Learning)
28. [[Humanoid Parkour Learning|Humanoid Parkour Learning]] · Zhuang et al. · 2024 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Humanoid+Parkour+Learning)
29. [[Learning Humanoid Locomotion with Perceptive Internal Model|PIM]] · Long et al. · 2025 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Learning+Humanoid+Locomotion+with+Perceptive+Internal+Model)
30. [[BeamDojo - Learning Agile Humanoid Locomotion on Sparse Footholds|BeamDojo]] · Wang et al. · 2025 · RSS · [Scholar](https://scholar.google.com/scholar?q=BeamDojo+Learning+Agile+Humanoid+Locomotion+on+Sparse+Footholds)
31. [[HumanoidBench - Simulated Humanoid Benchmark for Whole-Body Locomotion and Manipulation|HumanoidBench]] · Sferrazza et al. · 2024 · RSS · [Scholar](https://scholar.google.com/scholar?q=HumanoidBench+Simulated+Humanoid+Benchmark+for+Whole-Body+Locomotion+and+Manipulation)
32. [[Humanoid Locomotion and Manipulation - Current Progress and Challenges in Control, Planning, and Learning|Humanoid Loco-Manipulation Survey]] · Gu et al. · 2026 · IEEE/ASME T-Mech · [Scholar](https://scholar.google.com/scholar?q=Humanoid+Locomotion+and+Manipulation+Current+Progress+and+Challenges+in+Control+Planning+and+Learning)
33. [[What Matters in Humanoid General Motion Tracking An Empirical Study|What Matters in Motion Tracking]] · Amadio & Hoffman · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2607.19903)
34. [[Collision-Free Humanoid Traversal in Cluttered Indoor Scenes|Collision-Free Traversal]] · Xue et al. · 2026 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Collision-Free+Humanoid+Traversal+in+Cluttered+Indoor+Scenes)
35. [[Light-Loco-Parkour - Versatile Perceptive Whole-Body Locomotion via Multi-Skill Distillation|Light-Loco-Parkour]] · Chen et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2608.02653)
36. [[LessMimic - Long-Horizon Humanoid Interaction with Unified Distance Field Representations|LessMimic]] · Lin et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.21723)
37. [[Scalable Diffusion Models with Transformers|DiT]] · Peebles & Xie · 2023 · ICCV · [Scholar](https://scholar.google.com/scholar?q=Scalable+Diffusion+Models+with+Transformers)
38. [[Flow Matching for Generative Modeling|Flow Matching]] · Lipman et al. · 2023 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Flow+Matching+for+Generative+Modeling)
39. [[Real-Time Execution of Action Chunking Flow Policies|RTC]] · Black et al. · 2025 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Real-Time+Execution+of+Action+Chunking+Flow+Policies)
40. [[Training-Time Action Conditioning for Efficient Real-Time Chunking|Training-Time RTC]] · Black et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2512.05964)
41. [[Robust Motion In-betweening|LAFAN1]] · Harvey et al. · 2020 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=Robust+Motion+In-betweening)
42. [[Retargeting Matters - General Motion Retargeting for Humanoid Motion Tracking|GMR]] · Araújo et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2510.02252)
43. [[A Reduction of Imitation Learning and Structured Prediction to No-Regret Online Learning|DAgger]] · Ross et al. · 2011 · AISTATS · [Scholar](https://scholar.google.com/scholar?q=A+Reduction+of+Imitation+Learning+and+Structured+Prediction+to+No-Regret+Online+Learning)
44. [[OmniXtreme - Breaking the Generality Barrier in High-Dynamic Humanoid Control|OmniXtreme]] · Wang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.23843)
45. [[Lite Any Stereo - Efficient Zero-Shot Stereo Matching|LiteAnyStereo]] · Jing et al. · 2026 · CVPR · [Scholar](https://scholar.google.com/scholar?q=Lite+Any+Stereo+Efficient+Zero-Shot+Stereo+Matching)
46. [[NVIDIA Isaac Sim - Enabling Scalable, GPU-Accelerated Simulation for Robotics|Isaac Sim]] · Gao et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2606.03551)
47. [[MuJoCo - A physics engine for model-based control|MuJoCo]] · Todorov et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=MuJoCo+A+physics+engine+for+model-based+control)
48. [[World-Grounded Human Motion Recovery via Gravity-View Coordinates|GVHMR]] · Shen et al. · 2024 · SIGGRAPH Asia · [Scholar](https://scholar.google.com/scholar?q=World-Grounded+Human+Motion+Recovery+via+Gravity-View+Coordinates)
49. [[SimBa - Simplicity Bias for Scaling Up Parameters in Deep Reinforcement Learning|SimBa]] · Lee et al. · 2025 · ICLR · [Scholar](https://scholar.google.com/scholar?q=SimBa+Simplicity+Bias+for+Scaling+Up+Parameters+in+Deep+Reinforcement+Learning)
50. [[Isaac Lab - A GPU-Accelerated Simulation Framework for Multi-Modal Robot Learning|Isaac Lab]] · NVIDIA et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2511.04831)
51. [[Perceptive Humanoid Parkour - Chaining Dynamic Human Skills via Motion Matching|Perceptive Humanoid Parkour]] · Wu et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.15827)
52. [[RPL - Learning Robust Humanoid Perceptive Locomotion on Challenging Terrains|RPL]] · Zhang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.03002)
53. [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · Liao et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2508.08241)
54. [[MaskedMimic - Unified Physics-Based Character Control Through Masked Motion Inpainting|MaskedMimic]] · Tessler et al. · 2024 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=MaskedMimic+Unified+Physics-Based+Character+Control+Through+Masked+Motion+Inpainting)
55. [[ResMimic - From General Motion Tracking to Humanoid Whole-Body Loco-Manipulation via Residual Learning|ResMimic]] · Zhao et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2510.05070)
56. [[Hunyuan3D 2.1 - From Images to High-Fidelity 3D Assets with Production-Ready PBR Material|Hunyuan3D 2.1]] · Tencent Hunyuan3D Team · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2506.15442)

## See also
