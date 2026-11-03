---
type: author
name: "Guanya Shi"
aliases:
  - Prof. Guanya Shi
position: Assistant Professor, Robotics Institute and School of Computer Science, Carnegie Mellon University · Amazon Scholar
institutions:
  - "[[Carnegie Mellon University]]"
  - "[[Amazon]]"
labs:
  - "[[LeCAR Lab]]"
countries:
  - USA
phd: "[[California Institute of Technology]]"
phd_year: 2022
phd_advisors:
  - "[[Soon-Jo Chung]]"
  - "[[Yisong Yue]]"
bachelor: "[[Tsinghua University]]"
postdoc: "[[University of Washington]]"
homepage: http://www.gshi.me/
scholar: https://scholar.google.com/citations?user=joR1Z4UAAAAJ
github: https://github.com/LeCAR-Lab
advisor_candidate: true
relevance: high
added: 2026-11-02
tags:
  - author
  - topic/humanoids
  - topic/legged-locomotion
  - topic/whole-body-control
  - topic/sim-to-real
  - topic/teleoperation
  - topic/loco-manipulation
  - method/reinforcement-learning
  - method/mpc
  - robot/unitree-g1
---

# Guanya Shi
![[Pasted image 20261102084648.png]]

> [!info] Quick view
> **Now:** Assistant Professor at the Robotics Institute, [[Carnegie Mellon University]] · head of [[LeCAR Lab]] · Amazon Scholar at [[Amazon]] (Frontier AI & Robotics)
> **Trained at:** PhD [[California Institute of Technology]] 2022 ([[Soon-Jo Chung]], [[Yisong Yue]]) · Postdoc [[University of Washington]] · BE [[Tsinghua University]] 2017
> **Known for:** Neural Lander and Neural-Fly, H2O and OmniH2O teleoperation, ASAP delta-action sim-to-real, DIAL-MPC, FALCON force-adaptive loco-manipulation
> **Links:** [Homepage](http://www.gshi.me/) · [Scholar](https://scholar.google.com/citations?user=joR1Z4UAAAAJ) · [GitHub](https://github.com/LeCAR-Lab) · [Lab](https://lecar-lab.github.io/)
> **One line:** Learning and control together with guarantees, first for agile drones and now for humanoids and legged robots where RL policies have to survive the real hardware.

## Who he is

Guanya Shi did his BE at Tsinghua (2017) and then his PhD at Caltech (2022) with Soon-Jo Chung and Yisong Yue, so he comes from a mix of control theory and ML from the start. At Caltech he won the Simoudis Discovery Prize and the Ben P.C. Chou Doctoral Prize. After a postdoc at the University of Washington he joined the CMU Robotics Institute in September 2023, where he runs the LeCAR Lab (Learning and Control for Agile Robotics). He is also an Amazon Scholar in Amazon's Frontier AI & Robotics team. In only three years his lab became one of the main groups for humanoid whole-body control, with papers like OmniRetarget winning the ICRA 2026 best conference paper.

## Research line

- **2018 to 2022 · Learned dynamics with stability guarantees on drones.** At Caltech, Neural Lander used a spectrally normalized network to learn the ground effect and still prove exponential stability. Neural-Fly then meta-learned a wind representation offline and adapted it online, published in Science Robotics.
- **2021 to 2023 · Learning-theory side of control.** Online control, perturbation-based MPC and exploration papers at NeurIPS. This is the "theory to applications" part, he dont only do robots.
- **2024 · Legged agility and the first humanoid teleop.** ABS (RSS 2024) gives a quadruped high speed locomotion with a learned reach-avoid safety shield. H2O and OmniH2O start the humanoid line: retarget human motion, train a whole-body tracking policy with RL in sim, teleoperate a full-size humanoid from VR or an RGB camera.
- **2025 · Closing the sim-to-real gap on purpose.** ASAP learns a delta action model from real data so the simulator matches the real G1, then fine-tunes the policy. HOVER (with NVIDIA) distills many control modes into one whole-body controller. DIAL-MPC and SPI-Active bring sampling-based optimization back for torque-level control and system identification.
- **2025 to 2026 · Humanoid loco-manipulation and data at scale.** FALCON for force-adaptive humanoid loco-manipulation, OmniRetarget for interaction-preserving data generation, VIRAL for visual sim-to-real, perceptive parkour, plus a Science Robotics survey on the evolution of humanoid locomotion control.

## Key works (start here)

- [[Neural Lander - Stable Drone Landing Control using Learned Dynamics|Neural Lander]] · 2019 · ICRA 2019 · [arXiv](https://arxiv.org/abs/1811.08027) · the root of his style, a learned residual on top of a nominal model with a stability proof
- [[Neural-Fly Enables Rapid Learning for Agile Flight in Strong Winds|Neural-Fly]] · 2022 · Science Robotics · [arXiv](https://arxiv.org/abs/2205.06908) · offline meta-learning plus online adaptation, the same pattern you would want when the dynamics keep changing
- [[Agile But Safe - Learning Collision-Free High-Speed Legged Locomotion|ABS]] · 2024 · RSS 2024 · [arXiv](https://arxiv.org/abs/2401.17583) · agile policy plus a learned safety policy that takes over, very clean separation
- [[Learning Human-to-Humanoid Real-Time Whole-Body Teleoperation|H2O]] · 2024 · IROS 2024 · [arXiv](https://arxiv.org/abs/2403.04436) · the first full pipeline from human motion to humanoid tracking policy on hardware
- [[OmniH2O - Universal and Dexterous Human-to-Humanoid Whole-Body Teleoperation and Learning|OmniH2O]] · 2024 · CoRL 2024 · [arXiv](https://arxiv.org/abs/2406.08858) · teacher-student version of H2O with sparse inputs, also used to collect data for autonomous policies
- [[HOVER - Versatile Neural Whole-Body Controller for Humanoid Robots|HOVER]] · 2025 · ICRA 2025 · [arXiv](https://arxiv.org/abs/2410.21229) · one policy for many command modes through multi-mode distillation
- [[ASAP - Aligning Simulation and Real-World Physics for Learning Agile Humanoid Whole-Body Skills|ASAP]] · 2025 · RSS 2025 · [arXiv](https://arxiv.org/abs/2502.01143) · delta action model trained on real G1 data, honestly the paper to read first for sim-to-real
- [[DIAL-MPC - Full-Order Sampling-Based MPC for Torque-Level Locomotion Control via Diffusion-Style Annealing|DIAL-MPC]] · 2025 · ICRA 2025 · [arXiv](https://arxiv.org/abs/2409.15610) · training-free full-order MPC, a good model-based baseline next to the RL policies

## Useful for me

- **As prior work.** H2O, OmniH2O, HOVER and ASAP are basically the reference stack for RL whole-body control on real humanoids. Any whole-body controller for a humanoid that changes its body needs to be compared against this line.
- **Apply.** ASAP's delta action model is a natural tool for a body that changes. If the morphology change makes the sim mismatch worse in some configurations, a residual model learned from real data per configuration (or conditioned on the morphology) is kinda the direct extension.
- **Apply.** Neural-Fly's offline representation plus online adaptation fits very well with a morphology that changes during the task. The policy could adapt a few coefficients online instead of retraining for every new body.
- **For the IROS 2027 locomotion idea.** Read ASAP, SPI-Active and the Science Robotics survey on humanoid locomotion control before writing anything, so the sim-to-real and system identification part is positioned against what his lab already did.
- **Gap.** I didn't find any work from LeCAR on morphology change or co-design. Their robots keep the same body, so a humanoid that changes its own body would be a new direction for the lab, not a copy of what they do.

## Advisor fit

- **Where.** Robotics Institute, School of Computer Science at CMU, Pittsburgh. His students are mostly RI PhDs, with some through ECE and one co-advised from ETH Zürich.
- **Real robots by default.** The work ends on hardware: drones at Caltech, quadrupeds (ABS) and full-size humanoids now (ASAP on the Unitree G1). He also keeps a theory side (online control, sampling-based optimization convergence), so expect both.
- **Humanoids are the core right now.** Most 2025 to 2026 papers are humanoid whole-body control, loco-manipulation or humanoid data. Alumni went to OpenAI (Tairan He) and Amazon FAR (Xiaofeng Guo), and the lab collaborates with NVIDIA.
- **How to apply.** The lab's join page says to apply to the related SCS programs at CMU and mention his name as a faculty of interest. It says there's no need to email before the deadline, and after applying or being admitted the email subject should be "Prospective Ph.D./MS Student".
- **Open questions.** How many RI PhD students does he take per year now that the lab is big? Is there room for a project with custom hardware that changes shape, or does the lab stay on off-the-shelf humanoids? Would he co-advise with someone on the hardware side at CMU?

## Papers in my vault

> [!note] This list fills itself. Every paper note that lists Guanya Shi in `authors` shows up in this note's Backlinks.
