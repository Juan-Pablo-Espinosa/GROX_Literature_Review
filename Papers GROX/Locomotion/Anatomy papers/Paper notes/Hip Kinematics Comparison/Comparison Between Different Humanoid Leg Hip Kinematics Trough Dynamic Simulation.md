---
title: "Comparison Between Different Humanoid Leg Hip Kinematics Trough Dynamic Simulation"
aliases:
  - Hip Kinematics Comparison
authors:
  - "[[Maja Zorjan]]"
  - "[[Vincent Hugel]]"
  - "[[Pierre Blazevic]]"
corresponding: ""
institutions:
  - "[[University of Versailles Saint-Quentin-en-Yvelines]]"
labs:
  - "[[Versailles Engineering System Laboratory (LISV)]]"
countries:
  - France
year: 2011
date: 2011-12-01
venue: International Journal of Industrial Engineering and Management (IJIEM) 2011, Vol. 2 No. 4
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: http://www.iim.ftn.uns.ac.rs/ijiem_journal.php
pdf: ""
code: "No code (simulation study, checked 2026-11-02)"
status: to-read
relevance: low
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/leg-design
  - topic/hardware-design
  - topic/actuators
  - robot/romeo
  - sim/adams
  - eval/sim-only
---

# Hip Kinematics Comparison

> [!info] Quick view
> **Authors:** [[Maja Zorjan]], [[Vincent Hugel]], [[Pierre Blazevic]]
> **From:** [[University of Versailles Saint-Quentin-en-Yvelines]] · [[Versailles Engineering System Laboratory (LISV)]]
> **Published:** IJIEM · 2011 · journal
> **Keywords:** hip joint axes, inclined hip yaw, power distribution, torque-speed, ROMEO humanoid, Adams simulation
> **Links:** [Journal](http://www.iim.ftn.uns.ac.rs/ijiem_journal.php)
> **One line:** rotating the hip joint axes of the ROMEO humanoid by 15° or 30° does not help simple double support motions, it mostly increases hip power

## Summary

Most humanoids use orthogonal hip axes aligned with the body (yaw, roll, pitch). Robots like Johnnie (15° tilted yaw) and NAO (45° coupled yaw) break that rule. This paper, done inside the ROMEO project with Aldebaran, takes a 38 DoF, 1.38 m, 40.5 kg ROMEO model in Adams, rotates the orthogonal hip axes set by 15° and 30° around Z or around X, and compares joint power and torque-speed curves for three double support motions: knee flexion and extension, swaying hips sideways, and forward and backward pelvis motion.

The result is mostly negative. For knee flexion the rotated hips need about 3x more power at hip pitch and roll when rotated 30° about Z, and about 1.5x for 30° about X. Only swaying hips gets a benefit, around 20% lower hip pitch power with 30° about X. The pelvis motion is too small to see differences. Honestly this is a short workshop-level study with three slow motions, no walking, and a PID position controller, so the conclusions do not generalize much. The later Advanced Robotics paper from the same group is the full version.

## How it works

- ROMEO model generated automatically from Aldebaran Python files into Matlab and Adams, so new hip kinematics can be built fast
- Each leg: hip yaw, roll, pitch, knee, ankle pitch, ankle roll and toe, feet with 16 contact spheres and contact sensors
- Contact model is the Adams impact function with stiffness 1e10 N/m and friction 0.8/0.76
- Trajectories computed with inverse kinematics in Matlab and tracked with PID in Adams co-simulation
- Hip configurations: classical orthogonal, rotated 15° and 30° about Z, rotated 15° and 30° about X, axes stay orthogonal to each other
- Compared by power P = T·ω over time and torque vs speed per hip joint

## Results that matter

- Knee flexion: no benefit from any rotated hip, 3x more hip power for 30° about Z and 1.5x for 30° about X
- Rotation about X adds up to 10 Nm at hip roll during knee flexion where the classical hip needs 0
- Swaying hips: rotation about X reduces hip pitch power about 20% at 30°, rotation about Z reduces hip roll torque but raises hip pitch torque
- Pelvis forward/backward: too small amplitude to show any effect
- In general, more rotation means more torque and power

## Interesting stuff

- The automated model generation pipeline is the useful engineering idea, change kinematics and resimulate quickly
- Only quasi static double support motions are tested, walking (where the oblique hip should help) is left for future work
- The benefit of a tilted hip depends totally on which motion you look at, which is consistent with the later papers
- No statistics, no optimization over the angle, just two angles per axis

## Useful for me

- **As prior work.** Shows that joint axis orientation changes the actuator power distribution, and that the effect is task dependent. Good minor citation for task-dependent morphology.
- **Apply.** Same automated approach can be done much better today: generate URDF variants in a script and evaluate them with a trained policy in simulation.
- **Watch out.** A design that helps one motion can hurt another, so morphology evaluation must use a set of tasks and not only one motion.

## Key prior work

- [[Mechatronic design of NAO humanoid|NAO]] · the robot with the 45° coupled hip yaw, same lab
- [[Sensor and Control Design of a Dynamically Stable Biped Robot|Johnnie]] · biped with the 15° tilted hip yaw
- [[Influence of Hip Joint Axes Change of Orientation on Power Distribution in Humanoid Motion|ICARA 2011 hip axes]] · their companion paper on the same idea
- [[Humanoid robot HRP-2|HRP-2]] · classic orthogonal humanoid leg reference
- [[The intelligent ASIMO - System overview and integration|ASIMO]] · another classical hip layout example

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
10. [[Sensor and Control Design of a Dynamically Stable Biped Robot|Johnnie]] · Löffler et al. · 2003 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Sensor+and+Control+Design+of+a+Dynamically+Stable+Biped+Robot)
11. [[Mechatronic design of NAO humanoid|NAO]] · Gouaillier et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Mechatronic+design+of+NAO+humanoid)
12. [[MSC.Software Adams documentation|Adams docs]] · MSC Software · n.d. · software documentation · [Scholar](https://scholar.google.com/scholar?q=MSC+Adams+documentation+contact+impact+function)
13. [[Influence of Hip Joint Axes Change of Orientation on Power Distribution in Humanoid Motion|ICARA 2011 hip axes]] · Zorjan et al. · 2011 · ICARA · [Scholar](https://scholar.google.com/scholar?q=Influence+of+Hip+Joint+Axes+Change+of+Orientation+on+Power+Distribution+in+Humanoid+Motion)

## See also

- [[Influence of rotation of humanoid hip joint axes on joint power during locomotion|Hip axes rotation and joint power]] · the extended journal version with walking
- [[A Guideline for Humanoid Leg Design with Oblique Axes for Bipedal Locomotion|Oblique Axes Leg Guideline]] · biomechanics argument for rotated hips
