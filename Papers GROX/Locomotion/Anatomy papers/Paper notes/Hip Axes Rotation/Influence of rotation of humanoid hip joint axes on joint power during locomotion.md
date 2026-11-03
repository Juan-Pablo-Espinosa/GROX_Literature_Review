---
title: "Influence of rotation of humanoid hip joint axes on joint power during locomotion"
aliases:
  - Hip Axes Rotation
  - Hip axes rotation and joint power
authors:
  - "[[Maja Zorjan]]"
  - "[[Vincent Hugel]]"
  - "[[Pierre Blazevic]]"
  - "[[Branislav Borovac]]"
corresponding: "[[Vincent Hugel]]"
institutions:
  - "[[University of Versailles Saint-Quentin-en-Yvelines]]"
  - "[[Université de Toulon]]"
  - "[[University of Novi Sad]]"
labs:
  - "[[ENDICAP - INSERM U1179]]"
  - "[[COSMER Lab]]"
countries:
  - France
  - Serbia
year: 2015
date: 2015-03-23
venue: Advanced Robotics 2015 (Vol. 29, No. 11)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1080/01691864.2015.1013057
pdf: https://doi.org/10.1080/01691864.2015.1013057
code: "No code (simulation study, checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/leg-design
  - topic/legged-locomotion
  - topic/actuators
  - topic/biomechanics
  - robot/romeo
  - sim/adams
  - eval/sim-only
---

# Hip Axes Rotation

> [!info] Quick view
> **Authors:** [[Maja Zorjan]], [[Vincent Hugel]] (corresponding), [[Pierre Blazevic]], [[Branislav Borovac]]
> **From:** [[University of Versailles Saint-Quentin-en-Yvelines]] · [[ENDICAP - INSERM U1179]] · [[Université de Toulon]] · [[COSMER Lab]] · [[University of Novi Sad]]
> **Published:** Advanced Robotics · Mar 2015 · journal
> **Keywords:** hip kinematics, rotated hip joint frame, joint mechanical power, ROMEO, forward walk, turn in place, LIP-3D
> **Links:** [DOI](https://doi.org/10.1080/01691864.2015.1013057)
> **One line:** rotating the whole orthogonal hip joint frame by 45° about the forward axis cuts hip pitch power by up to 57% in forward walking and spreads the load between yaw and pitch motors

## Summary

This is the full journal version of the Versailles hip kinematics study on the ROMEO model (38 DoF, 1.38 m, 35.5 kg in Adams). Instead of slow double support motions, they now simulate dynamic walking: a 4-step forward walk of 0.6 m in 2.4 s and a 90° turn in place, with LIP-3D and ZMP patterns tracked by PID. The hip yaw-roll-pitch frame is kept orthogonal and rotated as a whole about the longitudinal x axis, inward or outward by 15°, 30°, 45° and 60°, and they compare average joint mechanical power (|τ·ω|).

In forward walking the hip uses about half of the leg power. At 45° and 60° the pitch power drops a lot, 57% at 45° outward, at the cost of more yaw power, so the load is shared and the yaw and pitch motors can be the same. Outward is better than inward for the stance leg during acceleration and regular steps, because the upper body plus swing leg CoM ends up in front of the rotated pitch axis, so gravity and acceleration torques partially cancel. Turning in place shows no gain and no loss. Only tilting the yaw axis like Johnnie or NAO gives no benefit, and the ARMAR-4 layout is not better than 45° outward. Honestly the gait is a worst case LIP gait with knee flexion, and the benefit depends on that flexion, so it would shrink for straighter human-like walking, which they say clearly.

## How it works

- ROMEO model generated automatically from a description file into Adams, with a custom IK that supports arbitrary hip and ankle axis orientations
- Contact with eight semi-spheres per foot, impact model with stiffness and penetration-dependent damping
- LIP-3D at 0.69 m with predefined ZMPs, hyperbolic CoM segments, no double support phase, knees flexed to 52°
- Forward walk: 4 steps of 0.2 m in 2.4 s, turn in place: six 30° rotary steps
- Metric: average absolute joint mechanical power per joint, plus torque and speed decomposition over the gait
- Configurations: hip frame rotated 15° to 60° inward and outward about x, plus Johnnie, NAO and ARMAR-4 layouts

## Results that matter

- Hip joints use nearly half of total leg power in forward walking, knee and ankle power barely change with hip layout
- 45° outward reduces hip pitch power by 57% vs the straight hip, swing leg pitch power reduced 50 to 64% in the regular steps
- Yaw power goes up from zero, so the gain is a redistribution more than a total saving
- 45° outward beats 45° inward on the stance leg during acceleration and regular steps, but is worse in the deceleration step
- Turn in place: legs use about half the power of forward walking, no benefit from rotation
- Tilting only the yaw axis (Johnnie, NAO) brings no benefit, ARMAR-4 reduces pitch power but has higher yaw power than 45° outward

## Interesting stuff

- The torque explanation is nice: τ_P = |L̇_H − M_g|, and the rotated pitch axis changes the sign relation between angular momentum change and gravity moment
- The advantage depends on knee flexion, and so on the gait style, so a different controller could erase it
- The human femoral neck is at about 60°, close to their 45°, but they are careful not to oversell the bio-inspiration
- Simulation only, PID position control, no actuator model, so "power" is mechanical not electrical

## Useful for me

- **As prior work.** Good quantitative evidence that joint axis orientation is a real design variable for actuator load. Pairs well with the oblique axes guideline from DLR.
- **Watch out.** The optimal hip rotation depends on the gait (knee flexion, step length). A learned policy may walk differently than LIP, so design conclusions should be checked with the actual policy.
- **Apply.** The joint power decomposition (torque, speed, power per joint over the gait cycle) is a simple tool to compare morphologies with a trained policy in sim.

## Key prior work

- [[Comparison Between Different Humanoid Leg Hip Kinematics Trough Dynamic Simulation|Hip Kinematics Comparison]] · their earlier version with slow double support motions
- [[Influence of Hip Joint Axes Change of Orientation on Power Distribution in Humanoid Motion|ICARA 2011 hip axes]] · companion conference paper
- [[Generalized humanoid leg inverse kinematics to deal with singularities|Generalized leg IK]] · the IK that makes arbitrary axes possible
- [[ARMAR-4 - A 63 DOF torque controlled humanoid robot|ARMAR-4]] · humanoid with a double-rotated hip
- [[Mechatronic design of NAO humanoid|NAO]] · 45° coupled hip yaw
- [[Sensor and Control Design of a Dynamically Stable Biped Robot|Johnnie]] · 15° tilted hip yaw

## References

1. [[The development of Honda humanoid robot|Honda humanoid]] · Hirai et al. · 1998 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+development+of+Honda+humanoid+robot)
2. [[The intelligent ASIMO - System overview and integration|ASIMO]] · Sakagami et al. · 2002 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+intelligent+ASIMO+System+overview+and+integration)
3. [[Humanoid robot HRP-2|HRP-2]] · Kaneko et al. · 2004 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+HRP-2)
4. [[The first human-size humanoid that can fall over safely and stand-up again|HRP-2P falling]] · Fujiwara et al. · 2003 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+first+human-size+humanoid+that+can+fall+over+safely+and+stand-up+again)
5. [[Design and development of research platform for perception-action integration in humanoid robot - H6|H6]] · Nishiwaki et al. · 2000 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+development+of+research+platform+for+perception-action+integration+in+humanoid+robot+H6)
6. [[Online generation of humanoid walking motion based on a fast generation method of motion pattern that follows desired ZMP|Online ZMP walking]] · Nishiwaki et al. · 2002 · IROS · [Scholar](https://scholar.google.com/scholar?q=Online+generation+of+humanoid+walking+motion+based+on+a+fast+generation+method+of+motion+pattern+that+follows+desired+ZMP)
7. [[Development of a new humanoid robot WABIAN-2|WABIAN-2]] · Ogura et al. · 2006 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Development+of+a+new+humanoid+robot+WABIAN-2)
8. [[Online walking pattern generation for biped humanoid robot with trunk|Walking with trunk]] · Lim et al. · 2002 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Online+walking+pattern+generation+for+biped+humanoid+robot+with+trunk)
9. [[System design and dynamic walking of humanoid robot KHR-2|KHR-2]] · Kim et al. · 2005 · ICRA · [Scholar](https://scholar.google.com/scholar?q=System+design+and+dynamic+walking+of+humanoid+robot+KHR-2)
10. [[Double spherical joint and backlash clutch for lower limbs of humanoids|Double spherical joint]] · Okada et al. · 2003 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Double+spherical+joint+and+backlash+clutch+for+lower+limbs+of+humanoids)
11. [[Sensor and Control Design of a Dynamically Stable Biped Robot|Johnnie]] · Löffler et al. · 2003 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Sensor+and+control+design+of+a+dynamically+stable+biped+robot)
12. [[Entwurf und Realisierung einer zweibeinigen Laufmaschine|Gienger thesis]] · Gienger · 2005 · VDI-Verlag book · [Scholar](https://scholar.google.com/scholar?q=Gienger+Entwurf+und+Realisierung+einer+zweibeinigen+Laufmaschine)
13. [[Mechatronic design of NAO humanoid|NAO]] · Gouaillier et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Mechatronic+design+of+NAO+humanoid)
14. [[ARMAR-4 - A 63 DOF torque controlled humanoid robot|ARMAR-4]] · Asfour et al. · 2013 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=ARMAR-4+A+63+DOF+torque+controlled+humanoid+robot)
15. [[Comparison Between Different Humanoid Leg Hip Kinematics Trough Dynamic Simulation|Hip Kinematics Comparison]] · Zorjan et al. · 2011 · IJIEM · [Scholar](https://scholar.google.com/scholar?q=Comparison+between+different+humanoid+leg+hip+kinematics+through+dynamic+simulation)
16. [[Influence of Hip Joint Axes Change of Orientation on Power Distribution in Humanoid Motion|ICARA 2011 hip axes]] · Zorjan et al. · 2011 · ICARA · [Scholar](https://scholar.google.com/scholar?q=Influence+of+hip+joint+axes+change+of+orientation+on+power+distribution+in+humanoid+motion)
17. [[MSC.Software Adams documentation|Adams docs]] · MSC Software · n.d. · software documentation · [Link](https://www.mscsoftware.com)
18. [[Real-time 3D walking pattern generation for a biped robot with telescopic legs|Meltran V]] · Kajita et al. · 2001 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Real-time+3D+walking+pattern+generation+for+a+biped+robot+with+telescopic+legs)
19. [[On the stability of anthropomorphic systems|Stability of anthropomorphic systems]] · Vukobratovic & Stepanenko · 1972 · Mathematical Biosciences · [Scholar](https://scholar.google.com/scholar?q=On+the+stability+of+anthropomorphic+systems)
20. [[Biped Locomotion - Dynamics, Stability, Control and Application|Biped Locomotion book]] · Vukobratovic et al. · 1990 · Springer · [Scholar](https://scholar.google.com/scholar?q=Vukobratovic+Borovac+Surla+Stokic+Biped+locomotion)
21. [[Zero-moment point - Thirty five years of its life|ZMP 35 years]] · Vukobratovic & Borovac · 2004 · IJHR · [Scholar](https://scholar.google.com/scholar?q=Zero-moment+point+thirty+five+years+of+its+life)
22. [[Walking patterns for real time path planning simulation of humanoids|Real-time walking patterns]] · Hugel & Jouandeau · 2012 · RO-MAN · [Scholar](https://scholar.google.com/scholar?q=Walking+patterns+for+real+time+path+planning+simulation+of+humanoids)
23. [[Generalized humanoid leg inverse kinematics to deal with singularities|Generalized leg IK]] · Zorjan & Hugel · 2013 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Generalized+humanoid+leg+inverse+kinematics+singularities+Zorjan+Hugel)
24. [[On the implementation of model predictive control for on-line walking pattern generation|MPC walking implementation]] · Dimitrov et al. · 2008 · ICRA · [Scholar](https://scholar.google.com/scholar?q=On+the+implementation+of+model+predictive+control+for+online+walking+pattern+generation)
25. [[Anatomie fonctionnelle - tome 2 - membre inférieur|Kapandji lower limb]] · Kapandji · 2009 · Maloine book · [Scholar](https://scholar.google.com/scholar?q=Kapandji+Anatomie+fonctionnelle+membre+inf%C3%A9rieur)

## See also

- [[A Guideline for Humanoid Leg Design with Oblique Axes for Bipedal Locomotion|Oblique Axes Leg Guideline]] · the biomechanics version of the same rotated hip argument
