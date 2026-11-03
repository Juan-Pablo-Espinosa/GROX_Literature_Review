---
title: "Combining Evolutionary and Adaptive Control Strategies for Quadruped Robotic Locomotion"
aliases:
  - Evolutionary Adaptive CPG
  - Evolutionary and adaptive control
authors:
  - "[[Elisa Massi]]"
  - "[[Lorenzo Vannucci]]"
  - "[[Ugo Albanese]]"
  - "[[Marie Claire Capolei]]"
  - "[[Alexander Vandesompele]]"
  - "[[Gabriel Urbain]]"
  - "[[Angelo Maria Sabatini]]"
  - "[[Joni Dambre]]"
  - "[[Cecilia Laschi]]"
  - "[[Silvia Tolu]]"
  - "[[Egidio Falotico]]"
corresponding: "[[Elisa Massi]]"
institutions:
  - "[[Scuola Superiore Sant'Anna]]"
  - "[[Technical University of Denmark]]"
  - "[[Ghent University]]"
  - "[[imec]]"
labs:
  - "[[The BioRobotics Institute]]"
  - "[[Automation and Control Group]]"
  - "[[AIRO]]"
countries:
  - Italy
  - Denmark
  - Belgium
year: 2019
date: 2019-08-29
venue: Frontiers in Neurorobotics 2019
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.3389/fnbot.2019.00071
pdf: ""
code: "No official repo yet (checked 2026-11-02)"
status: to-read
relevance: low
added: 2026-11-02
tags:
  - paper
  - topic/legged-locomotion
  - topic/bio-inspired-control
  - topic/sim-to-real
  - topic/domain-randomization
  - method/evolutionary-algorithms
  - method/cpg
  - method/cma-es
  - method/cerebellar-control
  - sim/gazebo
  - robot/fable
  - eval/sim-only
---

# Evolutionary Adaptive CPG

> [!info] Quick view
> **Authors:** [[Elisa Massi]] (corresponding), [[Lorenzo Vannucci]], [[Ugo Albanese]], [[Marie Claire Capolei]], [[Alexander Vandesompele]], [[Gabriel Urbain]], [[Angelo Maria Sabatini]], [[Joni Dambre]], [[Cecilia Laschi]], [[Silvia Tolu]], [[Egidio Falotico]]
> **From:** [[Scuola Superiore Sant'Anna]] · [[The BioRobotics Institute]] · [[Technical University of Denmark]] · [[Ghent University]] · [[imec]]
> **Published:** Frontiers in Neurorobotics 13:71 · August 2019
> **Keywords:** central pattern generator, CMA-ES, cerebellar model, LWPR, feedback error learning, Neurorobotics Platform, Fable quadruped, Baldwin effect
> **Links:** [DOI](https://doi.org/10.3389/fnbot.2019.00071)
> **One line:** evolving CPG gait parameters while a cerebellar-like adaptive controller is already running gives faster and more robust quadruped gaits than evolving first and plugging the adaptive part in afterwards

## Summary

The question is simple and kinda biological: does it help to have learning active during evolution, or is it the same to evolve the controller and add learning later? They use a simulated Fable quadruped with only the 4 hip joints actuated. A CPG of coupled oscillators (Gay et al.) generates the hip trajectories, a PID tracks them, and a cerebellum-inspired module (LWPR as granular layer plus a Purkinje layer trained with feedback error learning) adds a corrective torque. CMA-ES evolves the CPG parameters in two setups: adapt-after-evo (CPG plus PID gains evolved, cerebellum added later) and adapt-in-evo (CPG evolved with the cerebellum already learning, PID fixed).

The adapt-in-evo gait walks further and tracks better, and the gap gets bigger when they randomize damping, encoder noise and ground friction, or when friction changes suddenly during the run. Honestly the setup is small: 16 generations of 10 individuals, one simulated robot with 4 actuated DoF, no real robot test and distance in the order of 1.5 m per minute. But the idea that the optimizer should see the adaptive controller, not the bare one, is a useful one.

## How it works

- Fable quadruped in the Neurorobotics Platform (Gazebo), 2 DoF per leg but only the hip actuated
- CPG: 4 coupled Hopf-like oscillators with amplitude, frequency, phase shifts, duty cycle and offsets, 14 evolved parameters
- Low level: PID on joint position gives torques, plus cerebellar corrective torque u_im = u_lwpr + u_pl
- Cerebellum: one unit learning machine per leg, LWPR receptive fields as granular cells, Purkinje layer learned with the PID output as teaching signal
- CMA-ES, 16 generations by 10 individuals, 2 min simulation each, fitness is distance walked
- Robustness test: 15 robots with random damping (0.08 to 0.25) and 0 to 10% encoder noise, friction 0.3, 0.5 and 0.95, plus friction changed during the run (1 to 0 to 0.5)

## Results that matter

- Nominal: 1.72 m vs 1.48 m distance in the 20 s window, joint RMSE 0.035 vs 0.056 rad for adapt-in-evo vs adapt-after-evo
- Low friction: 1.69 ± 0.71 m vs 0.80 ± 0.27 m, p < 0.0005
- Medium and high friction: 1.66 vs 1.34 m and 1.68 vs 1.28 m, both significant
- Dynamic friction change: 10.28 m vs 6.18 m from 50 to 120 s, and adapt-after-evo do not fully recover its CoM limit cycle
- The adapt-after-evo individual was chosen as the best of the top 3 after adding the cerebellum, so the comparison is a bit asymmetric

## Interesting stuff

- Basically a robotic version of the Baldwin effect: evolution finds patterns that exploit the learner
- Robustness is tested after evolution, not with randomization during evolution, which is a cleaner test of generalization
- Weak points: tiny search budget, one seed of evolution per setup, sim only, very slow gaits, and the PID gains are evolved in one setup but fixed in the other
- They claim it "infers the crossing of the reality gap" without any real robot experiment, which is overclaimed

## Useful for me

- **As prior work.** Good citation for the argument that the outer optimizer (morphology or gait) should be run together with the adaptive inner controller, not before it.
- **Apply.** For morphology search with an RL policy, evaluate each body with a policy that can adapt online, not a frozen one, otherwise the search picks bodies that only work for a rigid controller.
- **Watch out.** The results are on a 4-DoF quadruped with CPG. It says nothing about dynamic humanoid locomotion.
- **Gap.** Only the controller parameters evolve, the body stays fixed.

## Key prior work

- [[Central pattern generators for locomotion control in animals and robots - a review|CPG review]] · Ijspeert, the classic CPG reference
- [[Learning robot gait stability using neural networks as sensory feedback function for central pattern generators|Gay CPG]] · the oscillator model they use
- [[Bio-inspired adaptive feedback error learning architecture for motor control|Tolu 2012]] · the cerebellar controller
- [[Calibration method to improve transfer from simulation to quadruped robots|Urbain 2018]] · previous CPG evolution on a quadruped from Ghent
- [[Body randomization reduces the sim-to-real gap for compliant quadruped locomotion|Body randomization]] · randomizing the body for transfer, close to my sim-to-real interest
- [[The CMA Evolution Strategy - A Comparing Review|CMA-ES]] · the optimizer

## References

1. [[The Greenhouse-Geisser Correction|Greenhouse-Geisser]] · Abdi · 2010 · Encyclopedia of Research Design · [Scholar](https://scholar.google.com/scholar?q=The+Greenhouse-Geisser+Correction+Abdi)
2. [[Evolving swimming soft-bodied creatures|Evolving swimmers]] · Corucci et al. · 2016 · ALIFE XV · [Scholar](https://scholar.google.com/scholar?q=Evolving+swimming+soft-bodied+creatures)
3. [[The cerebellar microcircuit as an adaptive filter - experimental and computational evidence|Cerebellum adaptive filter]] · Dean et al. · 2010 · Nat. Rev. Neurosci. · [DOI](https://doi.org/10.1038/nrn2756)
4. [[Connecting Artificial Brains to Robots in a Comprehensive Simulation Framework - The Neurorobotics Platform|Neurorobotics Platform]] · Falotico et al. · 2017 · Front. Neurorobotics · [DOI](https://doi.org/10.3389/fnbot.2017.00002)
5. [[Neuroevolution - from architectures to learning|Neuroevolution]] · Floreano et al. · 2008 · Evol. Intell. · [DOI](https://doi.org/10.1007/s12065-007-0002-4)
6. [[Adaptation mechanism of interlimb coordination in human split-belt treadmill walking through learning of foot contact timing - a robotics study|Split-belt adaptation]] · Fujiki et al. · 2015 · J. R. Soc. Interface · [DOI](https://doi.org/10.1098/rsif.2015.0542)
7. [[Templates and anchors - neuromechanical hypotheses of legged locomotion on land|Templates and anchors]] · Full & Koditschek · 1999 · J. Exp. Biol. · [Scholar](https://scholar.google.com/scholar?q=Templates+and+anchors+neuromechanical+hypotheses+of+legged+locomotion+on+land)
8. [[Distributed cerebellar plasticity implements adaptable gain control in a manipulation task - a closed-loop robotic simulation|Cerebellar gain control]] · Garrido Alcazar et al. · 2013 · Front. Neural Circuits · [DOI](https://doi.org/10.3389/fncir.2013.00159)
9. [[Learning robot gait stability using neural networks as sensory feedback function for central pattern generators|Gay CPG]] · Gay et al. · 2013 · IROS · [DOI](https://doi.org/10.1109/IROS.2013.6696353)
10. [[A note on the sphericity test|Sphericity test]] · Gleser · 1966 · Ann. Math. Stat. · [DOI](https://doi.org/10.1214/aoms/1177699529)
11. [[The CMA Evolution Strategy - A Comparing Review|CMA-ES]] · Hansen · 2006 · Towards a New Evolutionary Computation · [Scholar](https://scholar.google.com/scholar?q=The+CMA+Evolution+Strategy+A+Comparing+Review)
12. [[Evolutionary robotics - a new scientific tool for studying cognition|ER for cognition]] · Harvey et al. · 2005 · Artificial Life · [DOI](https://doi.org/10.1162/1064546053278991)
13. [[A Framework for Coupled Simulations of Robots and Spiking Neuronal Networks|NRP coupled simulation]] · Hinkel et al. · 2017 · J. Intell. Robot. Syst. · [Scholar](https://scholar.google.com/scholar?q=A+Framework+for+Coupled+Simulations+of+Robots+and+Spiking+Neuronal+Networks)
14. [[The co-ordination of swimmeret movements in the crayfish, Procambarus clarkii (Girard)|Crayfish swimmerets]] · Hughes & Wiersma · 1960 · J. Exp. Biol. · [Scholar](https://scholar.google.com/scholar?q=The+co-ordination+of+swimmeret+movements+in+the+crayfish+Procambarus+clarkii)
15. [[Central pattern generators for locomotion control in animals and robots - a review|CPG review]] · Ijspeert · 2008 · Neural Networks · [DOI](https://doi.org/10.1016/j.neunet.2008.03.014)
16. [[From swimming to walking with a salamander robot driven by a spinal cord model|Salamander robot]] · Ijspeert et al. · 2007 · Science · [Scholar](https://scholar.google.com/scholar?q=From+swimming+to+walking+with+a+salamander+robot+driven+by+a+spinal+cord+model)
17. [[Mechanisms of motor learning in the cerebellum|Cerebellar motor learning]] · Ito · 2000 · Brain Research · [DOI](https://doi.org/10.1016/S0006-8993(00)03142-5)
18. [[Control of mental activities by internal models in the cerebellum|Cerebellar internal models]] · Ito · 2008 · Nat. Rev. Neurosci. · [DOI](https://doi.org/10.1038/nrn2332)
19. [[A computational model of four regions of the cerebellum based on feedback-error learning|Feedback-error learning]] · Kawato & Gomi · 1992 · Biol. Cybern. · [DOI](https://doi.org/10.1007/BF00201431)
20. [[CPG-based control of a simulated snake-like robot adaptable to changing ground friction|Snake CPG friction]] · Kousuke et al. · 2007 · IROS · [Scholar](https://scholar.google.com/scholar?q=CPG-based+control+of+a+simulated+snake-like+robot+adaptable+to+changing+ground+friction)
21. [[Automatic design and manufacture of robotic lifeforms|GOLEM]] · Lipson & Pollack · 2000 · Nature · [DOI](https://doi.org/10.1038/35023115)
22. [[Evolutionary Robotics - The Biology, Intelligence, and Technology of Self-Organizing Machines|Evolutionary Robotics book]] · Nolfi & Floreano · 2000 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Evolutionary+Robotics+The+Biology+Intelligence+and+Technology+of+Self-Organizing+Machines)
23. [[A scalable neuro-inspired robot controller integrating a machine learning algorithm and a spiking cerebellar-like network|Ojeda 2017]] · Ojeda et al. · 2017 · Living Machines · [Scholar](https://scholar.google.com/scholar?q=A+scalable+neuro-inspired+robot+controller+integrating+a+machine+learning+algorithm+and+a+spiking+cerebellar-like+network)
24. [[Fable - a modular robot for students, makers and researchers|Fable]] · Pacheco et al. · 2014 · IROS Workshop · [Scholar](https://scholar.google.com/scholar?q=Fable+a+modular+robot+for+students+makers+and+researchers)
25. [[Statistical power for the two-factor repeated measures ANOVA|Repeated measures ANOVA]] · Potvin & Schutz · 2000 · Behav. Res. Methods · [DOI](https://doi.org/10.3758/BF03207805)
26. [[Evolutionary robotics - a review|ER review]] · Pratihar · 2003 · Sadhana · [DOI](https://doi.org/10.1007/BF02703810)
27. [[Power comparisons of Shapiro-Wilk, Kolmogorov-Smirnov, Lilliefors and Anderson-Darling tests|Normality tests]] · Razali & Wah · 2011 · J. Stat. Model. Anal. · [Scholar](https://scholar.google.com/scholar?q=Power+comparisons+of+Shapiro-Wilk+Kolmogorov-Smirnov+Lilliefors+and+Anderson-Darling+tests)
28. [[Locomotion of snake-like robots using adaptive neural oscillators|Snake adaptive oscillators]] · Ryu et al. · 2010 · Intell. Serv. Robot. · [DOI](https://doi.org/10.1007/s11370-009-0049-4)
29. [[Motivation in Embodied Intelligence|Embodied intelligence motivation]] · Starzyk · 2008 · Frontiers in Robotics, Automation and Control · [Scholar](https://scholar.google.com/scholar?q=Motivation+in+Embodied+Intelligence+Starzyk)
30. [[Neurophysiology of gait - from the spinal cord to the frontal lobe|Neurophysiology of gait]] · Takakusaki · 2013 · Movement Disorders · [DOI](https://doi.org/10.1002/mds.25669)
31. [[Adaptive and predictive control of a simulated robot arm|Tolu 2013]] · Tolu et al. · 2013 · Int. J. Neural Syst. · [DOI](https://doi.org/10.1142/S012906571350010X)
32. [[Bio-inspired adaptive feedback error learning architecture for motor control|Tolu 2012]] · Tolu et al. · 2012 · Biol. Cybern. · [DOI](https://doi.org/10.1007/s00422-012-0515-5)
33. [[Calibration method to improve transfer from simulation to quadruped robots|Urbain 2018]] · Urbain et al. · 2018 · SAB · [Scholar](https://scholar.google.com/scholar?q=Calibration+method+to+improve+transfer+from+simulation+to+quadruped+robots)
34. [[Body randomization reduces the sim-to-real gap for compliant quadruped locomotion|Body randomization]] · Vandesompele et al. · 2019 · Front. Neurorobotics · [DOI](https://doi.org/10.3389/fnbot.2019.00009)
35. [[A visual tracking model implemented on the iCub robot as a use case for a novel neurorobotic toolkit integrating brain and physics simulation|iCub visual tracking]] · Vannucci et al. · 2015 · Humanoids · [DOI](https://doi.org/10.1109/HUMANOIDS.2015.7363512)
36. [[A comprehensive gaze stabilization controller based on cerebellar internal models|Gaze stabilization 2017]] · Vannucci et al. · 2017 · Bioinspir. Biomim. · [DOI](https://doi.org/10.1088/1748-3190/aa8581)
37. [[Adaptive gaze stabilization through cerebellar internal models in a humanoid robot|Gaze stabilization 2016]] · Vannucci et al. · 2016 · BioRob · [DOI](https://doi.org/10.1109/BIOROB.2016.7523593)
38. [[Locally weighted projection regression - An O(n) algorithm for incremental real time learning in high dimensional space|LWPR]] · Vijayakumar & Schaal · 2000 · ICML · [Scholar](https://scholar.google.com/scholar?q=Locally+weighted+projection+regression+An+O(n)+algorithm+for+incremental+real+time+learning+in+high+dimensional+space)
39. [[Internal models in the cerebellum|Cerebellar internal models 1998]] · Wolpert et al. · 1998 · Trends Cogn. Sci. · [Scholar](https://scholar.google.com/scholar?q=Internal+models+in+the+cerebellum+Wolpert)

## See also

- [[Evolutionary robotics as a modelling tool in evolutionary biology|ER in evolutionary biology]] · Winfield on using evolutionary robotics for biology questions
