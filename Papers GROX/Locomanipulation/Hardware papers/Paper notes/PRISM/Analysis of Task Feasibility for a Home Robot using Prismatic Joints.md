---
title: "Analysis of Task Feasibility for a Home Robot using Prismatic Joints"
aliases:
  - PRISM
  - PRISM prismatic arm
authors:
  - "[[Tomoaki Mashimo]]"
  - "[[Rosen Diankov]]"
  - "[[Takateru Urakubo]]"
  - "[[Takeo Kanade]]"
corresponding: "[[Tomoaki Mashimo]]"
institutions:
  - "[[Carnegie Mellon University]]"
  - "[[Kobe University]]"
labs:
  - "[[CMU Robotics Institute]]"
countries:
  - USA
  - Japan
year: 2010
date: 2010-10-18
venue: IEEE/RSJ IROS 2010
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://scholar.google.com/scholar?q=Analysis+of+Task+Feasibility+for+a+Home+Robot+using+Prismatic+Joints
pdf: ""
code: "No official repo (hardware paper, checked 2026-11-02)"
related_code: https://github.com/rdiankov/openrave
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/manipulation
  - topic/hardware-design
  - topic/variable-length-limbs
  - topic/morphology
  - topic/actuators
  - method/motion-planning
  - sim/openrave
  - eval/real-robot
---

# PRISM

> [!info] Quick view
> **Authors:** [[Tomoaki Mashimo]] (corresponding), [[Rosen Diankov]], [[Takateru Urakubo]], [[Takeo Kanade]]
> **From:** [[Carnegie Mellon University]] · [[CMU Robotics Institute]] · [[Kobe University]]
> **Published:** IEEE/RSJ IROS 2010 · Oct 2010 · conference
> **Keywords:** prismatic arm, multi-parallel link, lazy tongs, differential drive, home robot, reachability, OpenRAVE, ikfast
> **Links:** [Scholar](https://scholar.google.com/scholar?q=Analysis+of+Task+Feasibility+for+a+Home+Robot+using+Prismatic+Joints) · [OpenRAVE (planning framework used)](https://github.com/rdiankov/openrave)
> **One line:** an arm built from lazy-tong prismatic links instead of a human-like shoulder-elbow-wrist chain is lighter, uses less energy for straight reaching and reaches into tighter spaces

## Summary

The idea starts from an observation: when humans reach for objects the hand follows a roughly straight line. So instead of copying the human arm with seven revolute joints, they build PRISM, an arm whose upper arm and forearm are lazy-tong (scissor) multi-parallel links driven by differential gears, so each segment can both extend and rotate. The arm has 7 DoF (3 prismatic, 4 revolute), reaches about 1.1 m, weighs 13 kg total and 2 kg beyond the shoulder, compared with 27 kg and 5.8 kg for the Barrett WAM.

They compare a scissor model to a two-link model in a simple inverse dynamics simulation (about 25% less mechanical energy for a straight line motion), and they test reachability in OpenRAVE kitchen scenes. In cluttered shelves PRISM can grasp the target from 3x more of the base locations than the WAM, but on an open table the WAM is better. The real prototype works but the scissor links deflect and sometimes hit obstacles. Honestly the simulation is very simple (gravity and friction ignored), so the energy claim is weak, but the concept of a telescoping arm that moves its elbow freely is interesting.

## How it works

- Each arm segment is a multi-parallel link of 36 links (16 rhombi), the "lazy tong" mechanism
- A differential gear at shoulder and elbow: both gears same direction gives rotation, opposite directions gives extension, mixing gives both
- Wrist has only 1 rotation DoF to keep the end effector light, plus a parallel gripper
- Links are carbon fiber reinforced plastic, peak 6.28 rad/s per joint and 500 mm/s translation, 1.5 kg payload fully extended
- Dynamics comparison: same total mass, sinusoidal joint trajectories that make the wrist go in a straight line, torque by inverse dynamics
- Task evaluation: grasp sets generated in OpenRAVE, analytic IK from ikfast (100,000 lines of C++, 50 µs per solve), BiRRT for motion planning
- Real robot uses Maxon motors with EPOS controllers in a 250 Hz PID loop

## Results that matter

- Mechanical energy about 25% lower for the scissor model vs the two-link model on the same straight line task
- In the two-link model the elbow needs about 2x the speed of the shoulder, so it needs much more peak power
- Graspability: table surface WAM 53.2% vs PRISM 50.0%, lower shelf 8.5% vs 26% (3.05x), upper shelf 17.2% vs 31.2% (1.81x)
- Real prototype reaches into a drawer, but the scissor links bend under load during trajectories

## Interesting stuff

- The swept volume argument is nice, a two-link arm sweeps a big area to change its reach, a prismatic arm almost none, that is why it wins in narrow places
- The dynamic comparison ignores gravity and friction, and friction is exactly the weak point of scissor mechanisms
- Torque is low when extended and high when contracted, like a slider-crank, so the arm can push hard at full reach
- Stiffness of long extending links is the main failure, same problem as any telescopic limb
- Old paper (2010) but Rosen Diankov's OpenRAVE and ikfast came from this same group

## Useful for me

- **As prior work.** A clean example of changing the link length instead of copying the human structure. Good to cite when arguing that limb length is a design variable and not only a fixed parameter.
- **Apply.** The graspability region metric (percentage of base positions from which a task is feasible) is a simple way to compare morphologies for a task, it could compare different limb lengths of a humanoid arm.
- **Watch out.** Extending mechanisms deflect under load. If the length can change, the simulation model must include the compliance at long extensions or the policy will be overconfident.
- **Gap.** No learning and no locomotion. The arm only changes length to reach, there is no notion of choosing the length for the task.

## Key prior work

- [[OpenRAVE - A Planning Architecture for Autonomous Robotics|OpenRAVE]] · the planning and ikfast framework used for all task tests
- [[Development of a Flying Robot With a Pantograph-Based Variable Wing Mechanism|Pantograph wing]] · the multi-parallel link mechanism reference
- [[1000 Trials - An Empirically Validated End Effector that Robustly Grasps Objects from the Floor|1000 Trials]] · other use of a scissor type mechanism, from Kemp's lab
- [[Kinematic Rules for Upper and Lower Arm Contributions to Grasp Orientation|Kinematic rules of grasping]] · the human reaching observation that motivates straight line motion
- [[Dynamics of Elastic Manipulators With Prismatic Joints|Elastic prismatic manipulators]] · prior prismatic manipulator dynamics
- [[Towards a personal robotics development platform - Rationale and design of an intrinsically safe personal robot|PR1]] · personal robot design they compare their goal to

## References

1. [[Devices for assisting manipulation - a summary of user task priorities|Assistive manipulation priorities]] · Stanger et al. · 1994 · IEEE T-RE · [Scholar](https://scholar.google.com/scholar?q=Devices+for+assisting+manipulation+a+summary+of+user+task+priorities)
2. [[A point-and-click interface for the real world - laser designation of objects for mobile manipulation|Point-and-click interface]] · Kemp et al. · 2008 · HRI · [Scholar](https://scholar.google.com/scholar?q=A+point-and-click+interface+for+the+real+world+laser+designation+of+objects+for+mobile+manipulation)
3. [[Preliminary design of a whole-arm manipulation system (WAMS)|WAMS]] · Salisbury et al. · 1988 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Preliminary+design+of+a+whole-arm+manipulation+system+WAMS)
4. [[The development of Honda humanoid robot|Honda humanoid]] · Hirai et al. · 1998 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+development+of+Honda+humanoid+robot)
5. [[Design and development of research platform for perception-action integration in humanoid robot - H6|H6]] · Nishiwaki et al. · 2000 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+development+of+research+platform+for+perception-action+integration+in+humanoid+robot+H6)
6. [[MANUS - a wheelchair-mounted rehabilitation robot|MANUS]] · Driessen et al. · 2001 · Proc. IMechE Part H · [Scholar](https://scholar.google.com/scholar?q=MANUS+a+wheelchair-mounted+rehabilitation+robot)
7. [[Sensing and Manipulating Built-for-human Environments|Built-for-human environments]] · Brooks et al. · 2004 · IJHR · [Scholar](https://scholar.google.com/scholar?q=Sensing+and+Manipulating+Built-for-human+Environments)
8. [[Humanoid robot HRP-2|HRP-2]] · Kaneko et al. · 2004 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+HRP-2)
9. [[Mechanical design of humanoid robot platform KHR-3 (KAIST Humanoid Robot 3 - HUBO)|KHR-3 HUBO]] · Park et al. · 2005 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Mechanical+design+of+humanoid+robot+platform+KHR-3+KAIST+Humanoid+Robot+3+HUBO)
10. [[ARMAR-III - An Integrated Humanoid Platform for Sensory-Motor Control|ARMAR-III]] · Asfour et al. · 2006 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=ARMAR-III+An+Integrated+Humanoid+Platform+for+Sensory-Motor+Control)
11. [[Mobile manipulators for assisted living in residential settings|Mobile manipulators for assisted living]] · Deegan et al. · 2008 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Mobile+manipulators+for+assisted+living+in+residential+settings)
12. [[On a new generation of torque controlled light-weight robots|DLR LWR]] · Hirzinger et al. · 2001 · ICRA · [Scholar](https://scholar.google.com/scholar?q=On+a+new+generation+of+torque+controlled+light-weight+robots)
13. [[Fast and soft-arm tactics (robot arm design)|Fast and soft arm tactics]] · Bicchi & Tonietti · 2004 · IEEE RAM · [Scholar](https://scholar.google.com/scholar?q=Fast+and+soft-arm+tactics+robot+arm+design)
14. [[Towards a personal robotics development platform - Rationale and design of an intrinsically safe personal robot|PR1]] · Wyrobek et al. · 2008 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Towards+a+personal+robotics+development+platform+Rationale+and+design+of+an+intrinsically+safe+personal+robot)
15. [[A new actuation approach for human friendly robot design|DM2 actuation]] · Zinn et al. · 2004 · ICRA · [Scholar](https://scholar.google.com/scholar?q=A+new+actuation+approach+for+human+friendly+robot+design)
16. [[Design and Control of a Bio-inspired Human-Friendly Robot|Bio-inspired human-friendly robot]] · Shin et al. · 2008 · ISER · [Scholar](https://scholar.google.com/scholar?q=Design+and+Control+of+a+Bio-inspired+Human-Friendly+Robot)
17. [[Kinematic Rules for Upper and Lower Arm Contributions to Grasp Orientation|Kinematic rules of grasping]] · Marotta et al. · 2003 · J Neurophysiol · [Scholar](https://scholar.google.com/scholar?q=Kinematic+Rules+for+Upper+and+Lower+Arm+Contributions+to+Grasp+Orientation)
18. [[Dynamics of Elastic Manipulators With Prismatic Joints|Elastic prismatic manipulators]] · Buffinton · 1992 · J Dyn Syst Meas Control · [Scholar](https://scholar.google.com/scholar?q=Dynamics+of+Elastic+Manipulators+With+Prismatic+Joints)
19. [[A linear solution to the kinematic parameter identification of robot manipulators|Kinematic parameter identification]] · Zhuang & Roth · 1993 · IEEE T-RA · [Scholar](https://scholar.google.com/scholar?q=A+linear+solution+to+the+kinematic+parameter+identification+of+robot+manipulators)
20. [[Nonholonomic control of 3 link planar manipulator with a free joint|Nonholonomic 3-link]] · Imura et al. · 1996 · CDC · [Scholar](https://scholar.google.com/scholar?q=Nonholonomic+control+of+3+link+planar+manipulator+with+a+free+joint)
21. [[Minimum effort inverse kinematics for redundant manipulators|Minimum effort IK]] · Deo & Walker · 1997 · IEEE T-RA · [Scholar](https://scholar.google.com/scholar?q=Minimum+effort+inverse+kinematics+for+redundant+manipulators)
22. [[Development of a Flying Robot With a Pantograph-Based Variable Wing Mechanism|Pantograph wing]] · Hara et al. · 2009 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Development+of+a+Flying+Robot+With+a+Pantograph-Based+Variable+Wing+Mechanism)
23. [[1000 Trials - An Empirically Validated End Effector that Robustly Grasps Objects from the Floor|1000 Trials]] · Xu et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=1000+Trials+An+Empirically+Validated+End+Effector+that+Robustly+Grasps+Objects+from+the+Floor)
24. [[BiSpace Planning - Concurrent Multi-Space Exploration|BiSpace Planning]] · Diankov et al. · 2008 · RSS · [Scholar](https://scholar.google.com/scholar?q=BiSpace+Planning+Concurrent+Multi-Space+Exploration)
25. [[OpenRAVE - A Planning Architecture for Autonomous Robotics|OpenRAVE]] · Diankov & Kuffner · 2008 · CMU tech report CMU-RI-TR-08-34 · [GitHub](https://github.com/rdiankov/openrave)
26. [[RRT-Connect - An Efficient Approach to Single-Query Path Planning|RRT-Connect]] · Kuffner & LaValle · 2000 · ICRA · [Scholar](https://scholar.google.com/scholar?q=RRT-Connect+An+Efficient+Approach+to+Single-Query+Path+Planning)

## See also

- [[Triple Scissor Extender Robot Arm - A Solution to the Last One Foot Problem of Manipulation|Triple Scissor Extender]] · a later and stiffer take on scissor extension for arms
