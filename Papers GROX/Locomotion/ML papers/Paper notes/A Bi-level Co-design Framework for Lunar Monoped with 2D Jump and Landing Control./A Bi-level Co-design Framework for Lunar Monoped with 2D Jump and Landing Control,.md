---
title: "A Bi-level Co-design Framework for Lunar Monoped with 2D Jump and Landing Control"
aliases:
  - Lunar Monoped Co-design
authors:
  - "[[Sanjay Ramkumar]]"
  - "[[Suraj Kumar]]"
corresponding: ""
institutions: []
countries: []
year: 2026
date: ""
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2610.37199"
url: https://arxiv.org/abs/2610.37199
pdf: https://arxiv.org/pdf/2610.37199
code: "No official repo found yet (checked 2026-11-01)"
status: to-read
relevance: medium
added: 2026-11-01
tags:
  - paper
  - topic/co-design
  - topic/morphology
  - topic/legged-locomotion
  - topic/hardware-design
  - topic/space-robotics
  - method/evolutionary-algorithms
  - method/trajectory-optimization
  - method/qp-control
  - eval/sim-only
---

# Lunar Monoped Co-design

> [!info] Quick view
> **Authors:** [[Sanjay Ramkumar]], [[Suraj Kumar]]
> **From:** affiliation not confirmed yet
> **Published:** arXiv preprint · October 2026 · cs.RO, eess.SY
> **Keywords:** co-design, bi-level optimization, differential evolution, task-space QP control, direct collocation, cost of transport, lunar gravity, monoped jumping
> **Links:** [arXiv](https://arxiv.org/abs/2610.37199) · [PDF](https://arxiv.org/pdf/2610.37199) · [HTML](https://arxiv.org/html/2610.37199)
> **One line:** Differential evolution searches the leg link lengths of a planar monoped in lunar gravity, the inner controller is swapped between a task-space QP and trajectory optimization, and the optimal morphology changes depending on which controller you use.

## Summary

This paper try to answer a question that matters a lot for co-design: does the "optimal body" depend on the controller you evaluate it with? They take a planar monoped in lunar gravity (1.625 m/s²), where long flight phases and low contact forces make leg design and control strongly coupled. A differential evolution outer loop searches over link lengths, and each candidate does a 0.2 m vertical jump with one of two inner controllers: a QP-based task-space controller (QP-TSC) or direct-collocation trajectory optimization. The objective is the most energy-efficient morphology for that jump.

The answer is yes. The two controllers give different optimal link lengths, and trajectory optimization beats the QP-TSC over most of the design space, with 17% lower cost of transport at their respective optima. Because a vertical jump hides pitch, horizontal motion and landing, they also build a 2D jump and landing controller. It's a small, clean study and the main message is honestly the useful part: co-design results are only valid for the controller inside the loop.

## How it works

- **System.** A planar monoped (link lengths are the design variables) simulated in lunar gravity, 1.625 m/s².
- **Bi-level co-design.** The outer loop is differential evolution over link lengths. The inner loop evaluates each candidate on the jump task with a fixed controller, and the objective is energy efficiency (cost of transport) for a 0.2 m jump.
- **Inner controller A.** A quadratic-program task-space controller (QP-TSC), the typical reactive whole-body-control style.
- **Inner controller B.** Direct-collocation trajectory optimization over the whole jump.
- **Controlled comparison.** They fix the task and the outer loop and only swap the inner controller, so any difference in the optimal morphology comes from the controller.
- **2D jump and landing.** A planar controller that handles takeoff, a flight phase that prepares for landing and holds the landing configuration, and the landing itself.

## Results that matter

> [!warning] Full text not read yet. These are only the abstract-level results. Add the optimal link lengths, the COT values and Table III (2D jump metrics) from the PDF.

- QP-TSC and trajectory optimization give different optimal link lengths for the same task and outer loop.
- Trajectory optimization outperforms QP-TSC across most of the design space.
- At their respective optima, trajectory optimization has 17% lower cost of transport.
- The 2D controller achieves a stable planar jump and landing (Table III has the metrics).

## Interesting stuff

- Swapping only the inner controller is a clean way to show that the optimal morphology is controller-dependent. A lot of co-design papers quietly assume the controller doesn't matter.
- Lunar gravity makes the coupling more obvious. Flight time is long and landing impacts are a big part of the problem, so leg geometry changes a lot about what the controller can do.
- **Weak points:** only planar, only a monoped, only simulation, and the co-design itself is done on the vertical 1D jump. The 2D jump and landing controller isn't inside the co-design loop, as far as I can tell from the structure.
- No learned controller in the comparison. With an RL policy in the inner loop the optimal morphology could move again, and that's the case I care about.

## Useful for me

- **Watch out.** The main lesson transfers directly: if we pick limb lengths or ranges by optimizing with one controller (say MPC) and then deploy an RL policy, the "optimal" design may not be optimal anymore. The controller has to be part of the evaluation.
- **Apply.** An evolutionary outer loop over morphology with a fixed inner evaluation is a simple baseline for any design search, before trying anything fancier like morphology-conditioned RL.
- **Gap.** Their morphology is fixed after design. For a robot that changes its body during the task, the question becomes which morphology is best per phase (takeoff vs flight vs landing), and that's not studied here.
- **As prior work.** Mostly a co-design reference and a space-robotics one. It's not humanoid and not learning-based, so I'd keep it as background.

## References

1. [[SpaceBok - A Dynamic Legged Robot for Space Exploration|SpaceBok]] · Arm et al. · 2019 · ICRA 2019 · [Scholar](https://scholar.google.com/scholar?q=Spacebok+A+dynamic+legged+robot+for+space+exploration)
2. [[Towards Jumping Locomotion for Quadruped Robots on the Moon|Jumping on the Moon]] · Kolvenbach et al. · 2019 · IROS 2019 · [Scholar](https://scholar.google.com/scholar?q=Towards+jumping+locomotion+for+quadruped+robots+on+the+moon)
3. [[Cat-like Jumping and Landing of Legged Robots in Low Gravity Using Deep Reinforcement Learning|Cat-like Jumping]] · Rudin et al. · 2022 · IEEE T-RO · [DOI](http://dx.doi.org/10.1109/TRO.2021.3084374)
4. [[Computation of Regions of Attraction for Hybrid Limit Cycles Using Reachability - An Application to Walking Robots|Hybrid Limit Cycle ROA]] · Choi et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2201.08538)
5. [[SpaceHopper - A Small-Scale Legged Robot for Exploring Low-Gravity Celestial Bodies|SpaceHopper]] · Spiridonov et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2403.02831)
6. [[Olympus - A Jumping Quadruped for Planetary Exploration Utilizing Reinforcement Learning for In-Flight Attitude Control|Olympus]] · Olsen et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2503.03574)
7. [[Dynamic Modeling and Attitude Control of a Reaction-Wheel-Based Low-Gravity Bipedal Hopper|Reaction-Wheel Biped Hopper]] · Hari et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2603.10670)
8. [[Joint Optimization of Robot Design and Motion Parameters using the Implicit Function Theorem|Implicit Function Co-Design]] · Ha et al. · 2017 · RSS 2017 · [Scholar](https://scholar.google.com/scholar?q=Joint+optimization+of+robot+design+and+motion+parameters+using+the+implicit+function+theorem)
9. [[Jointly Learning to Construct and Control Agents using Deep Reinforcement Learning|Joint Construct and Control RL]] · Schaff et al. · 2018 · arXiv · [arXiv](https://arxiv.org/abs/1801.01432)
10. [[A Versatile Co-Design Approach for Dynamic Legged Robots|Versatile Co-Design]] · Dinev et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2103.04660)
11. [[Robust Co-Design - Coupling Morphology and Feedback Design Through Stochastic Programming|Robust Co-Design]] · Bravo-Palacios et al. · 2021 · ASME Journal of Dynamic Systems, Measurement, and Control · [DOI](https://doi.org/10.1115/1.4052463)
12. [[Computational Design of Energy-Efficient Legged Robots - Optimizing for Size and Actuators|Energy-Efficient Computational Design]] · Fadini et al. · 2021 · ICRA 2021 · [Scholar](https://scholar.google.com/scholar?q=Computational+design+of+energy-efficient+legged+robots+Optimizing+for+size+and+actuators)
13. [[A Co-Design Framework for Energy-Aware Monoped Jumping with Detailed Actuator Modeling|Energy-Aware Monoped Co-Design]] · Singh et al. · 2025 · Humanoids 2025 · [arXiv](https://arxiv.org/abs/2510.05923)
14. [[Martian Lava Tube Exploration Using Jumping Legged Robots - A Concept Study|Martian Lava Tubes]] · Olsen et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2310.14876)
15. [[Control Policies for a Large Region of Attraction for Dynamically Balancing Legged Robots - A Sampling-Based Approach|Large ROA Policies]] · Bhounsule et al. · Robotica · [Link](https://par.nsf.gov/biblio/10182618)
16. [[Balancing and Step Recovery Capturability via Sums-of-Squares Optimization|SOS Capturability]] · Posa et al. · 2017 · RSS 2017 · [Scholar](https://scholar.google.com/scholar?q=Balancing+and+step+recovery+capturability+via+sums-of-squares+optimization)
## See also

These papers are close to this one, but I couldn't check whether it cites them. If they show up in the bibliography, move them to References.

- [[A Co-Design Framework for Energy-Aware Monoped Jumping with Detailed Actuator Modeling|Energy-Aware Monoped Co-Design]] · co-design of monoped mechanics, gearbox and control for jump height vs energy, with hardware · [arXiv](https://arxiv.org/abs/2510.05923)
- [[A Co-Design Framework for High-Performance Jumping of a Five-Bar Monoped with Actuator Optimization|Five-Bar Monoped Co-Design]] · same group as above, closed-chain leg plus actuator optimization · [arXiv](https://arxiv.org/abs/2604.06025)
- [[Simulation Aided Co-Design for Robust Robot Optimization|Robust Bi-level Co-Design]] · bi-level co-design with a genetic algorithm outer loop and DDP inner loop, also tested on a jumping monoped · [Scholar](https://scholar.google.com/scholar?q=Simulation+Aided+Co-Design+for+Robust+Robot+Optimization)