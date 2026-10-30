---
type: author
name: "Xue Bin Peng"
aliases:
  - Jason Peng
  - Xue Bin (Jason) Peng
position: Assistant Professor, School of Computing Science · Research Scientist at NVIDIA
institutions:
  - "[[Simon Fraser University]]"
  - "[[NVIDIA]]"
countries:
  - Canada
phd: "[[University of California, Berkeley]]"
phd_year: 2021
phd_advisors:
  - "[[Sergey Levine]]"
  - "[[Pieter Abbeel]]"
msc: "[[University of British Columbia]]"
msc_advisors:
  - "[[Michiel van de Panne]]"
homepage: https://xbpeng.github.io/
scholar: https://scholar.google.com/citations?view_op=search_authors&mauthors=Xue+Bin+Peng
github: https://github.com/xbpeng
advisor_candidate: true
relevance: high
added: 2026-10-30
tags:
  - author
  - topic/character-animation
  - topic/humanoids
  - topic/legged-locomotion
  - topic/motion-tracking
  - topic/sim-to-real
  - topic/domain-randomization
  - method/reinforcement-learning
  - method/adversarial-motion-priors
  - method/imitation-learning
---

# Xue Bin (Jason) Peng
![[Pasted image 20261030084834.png]]
> [!info] Quick view
> **Now:** Assistant Professor, Computing Science at [[Simon Fraser University]] · Research Scientist at [[NVIDIA]]
> **Trained at:** PhD [[University of California, Berkeley]] ([[Sergey Levine]], [[Pieter Abbeel]]) · BSc + MSc [[University of British Columbia]] ([[Michiel van de Panne]])
> **Known for:** DeepMimic, AMP, ASE, dynamics randomization, MaskedMimic
> **Links:** [Homepage](https://xbpeng.github.io/) · [SFU page](https://www.sfu.ca/fas/computing/people/faculty/faculty-members/xbpeng.html) · [NVIDIA page](https://research.nvidia.com/labs/dair/author/xue-bin-jason-peng/) · [Scholar](https://scholar.google.com/citations?view_op=search_authors&mauthors=Xue+Bin+Peng) · [GitHub](https://github.com/xbpeng)
> **One line:** Motion data plus deep RL in physics simulation to get natural, agile motor skills on simulated characters and real legged robots.

## Who he is

Jason Peng is basically one of the main people behind the idea of using motion data plus RL to get natural and agile skills, first on simulated characters and then on real robots. He did his BSc (2015) and MSc (2017) at UBC with Michiel van de Panne, then his PhD at UC Berkeley with Sergey Levine and Pieter Abbeel, finished in 2021. Now he is Assistant Professor in Computing Science at SFU and also a Research Scientist at NVIDIA, so he sit between academia and a company with huge simulation infrastructure. His thesis won the SIGGRAPH 2022 Outstanding Doctoral Dissertation Award, the animal imitation paper won Best Paper at RSS 2020, and he got the Graphics Interface 2024 Early Career Researcher Award. Amii also lists him as a Canada CIFAR AI Chair.

## Research line

- **2016 to 2018 · Motion imitation for characters.** Physics-based animation at UBC and early Berkeley. DeepMimic is the big one: track a reference clip with RL plus a task reward, and the same recipe works on a human, Atlas, a dinosaur and a dragon.
- **2018 · Sim-to-real.** The dynamics randomization paper (done at OpenAI) is one of the papers everybody cite when they randomize mass, friction and latency during training.
- **2020 · From characters to legged robots.** Imitating animals on a Laikago quadruped. Retarget dog mocap, imitate it in sim, then adapt in the real world with a latent-space adaptation step that only needs a few minutes of real data.
- **2021 to 2022 · Motion priors instead of tracking.** AMP replaces the hand-designed tracking reward with a discriminator trained on unstructured clips, so the style comes from data and the task comes from a simple reward. ASE scales this into a reusable skill embedding, pretrained in Isaac Gym with around a decade of simulated experience.
- **2024 to 2026 · Unified controllers and humanoid robots.** MaskedMimic (one controller conditioned on partial goals like keyframes, text or objects), versatile Cassie locomotion with Berkeley, TWIST whole-body teleop with Stanford, and NVIDIA work on generative motion and large-scale pretraining for motor control.

## Key works (start here)

- [[DeepMimic - Example-Guided Deep Reinforcement Learning of Physics-Based Character Skills|DeepMimic]] · 2018 · ACM TOG (SIGGRAPH 2018) · [arXiv](https://arxiv.org/abs/1804.02717) · the base recipe for RL motion tracking, most humanoid trackers today descend from this
- [[Sim-to-Real Transfer of Robotic Control with Dynamics Randomization|Dynamics Randomization]] · 2018 · ICRA 2018 · [arXiv](https://arxiv.org/abs/1710.06537) · where dynamics randomization as the default sim-to-real tool comes from
- [[Learning Agile Robotic Locomotion Skills by Imitating Animals|Imitating Animals]] · 2020 · RSS 2020 · [arXiv](https://arxiv.org/abs/2004.00784) · full pipeline retarget, imitate, adapt on a real legged robot
- [[AMP - Adversarial Motion Priors for Stylized Physics-Based Character Control|AMP]] · 2021 · ACM TOG (SIGGRAPH 2021) · [arXiv](https://arxiv.org/abs/2104.02180) · the motion prior as a learned style reward, no clip selection needed
- [[ASE - Large-Scale Reusable Adversarial Skill Embeddings for Physically Simulated Characters|ASE]] · 2022 · ACM TOG (SIGGRAPH 2022) · [arXiv](https://arxiv.org/abs/2205.01906) · pretrain a skill space once and reuse it for new tasks
- [[MaskedMimic - Unified Physics-Based Character Control Through Masked Motion Inpainting|MaskedMimic]] · 2024 · ACM TOG (SIGGRAPH Asia 2024) · [arXiv](https://arxiv.org/abs/2409.14393) · one controller with many conditioning modes, trained as motion inpainting
- [[Reinforcement Learning for Versatile, Dynamic, and Robust Bipedal Locomotion Control|Versatile Bipedal RL]] · 2024 · IJRR 2024 · [arXiv](https://arxiv.org/abs/2401.16889) · real biped (Cassie), dual-history architecture, task randomization on top of dynamics randomization
- [[TWIST - Teleoperated Whole-Body Imitation System|TWIST]] · 2025 · CoRL 2025 · [Link](https://yanjieze.com/TWIST/) · whole-body humanoid teleop through motion imitation, his closest work to real humanoids

## Useful for me

- **Template.** DeepMimic to AMP to ASE is basically the template for our controller: motion data gives the style prior, RL handles the task and the physics. For a humanoid that changes its body this is attractive, because a discriminator on motion features is easier to keep valid across body changes than a per-joint tracking reward.
- **Gap.** DeepMimic already reports policies adapting to morphology changes, but the body is fixed for each training run. A body that changes during the episode, with the policy aware of it, is still open, and that is exactly our problem.
- **Sim-to-real.** Dynamics randomization and the animal-imitation adaptation step are the two baselines to compare against when we move from sim to the real robot. Morphology change is kinda one more axis of randomization, with the difference that we control it on purpose.
- **Watch out.** Most of his flagship work is on simulated characters. The real-robot results are mostly quadruped and Cassie with collaborators, so for full-size humanoid sim-to-real the closer evidence come from TWIST and the newer NVIDIA work, not from AMP or ASE directly.
- **Tooling.** ASE was trained in Isaac Gym and his NVIDIA side build on the same sim stack, so his methods port pretty directly to an Isaac Lab pipeline.

## Advisor fit

- **Where.** School of Computing Science at SFU (Burnaby, BC), plus part-time Research Scientist at NVIDIA. So a PhD with him is a CS PhD, not mechanical or robotics engineering.
- **Graphics plus robotics.** His listed interests are character animation, RL and robotics, and Amii lists humanoid robotics as a focus. Expect a group that publishes at SIGGRAPH as much as at CoRL or RSS.
- **Recent humanoid work.** Coauthor on TWIST (CoRL 2025) with Stanford and the Cassie work with Berkeley. From what I found, a lot of his real-robot work happens through collaborations.
- **Prospective students.** His homepage has a prospective-students section. Read it before emailing, he put that info there for a reason.
- **Open questions.** Does he have humanoid hardware at SFU or only through collaborators? Is he taking PhD students for my cycle? How do students use the NVIDIA connection (compute, internships)? What are his current students working on?

## Papers in my vault

> [!note] This list fills itself. Every paper note that lists Xue Bin Peng in `authors` shows up in this note's Backlinks.