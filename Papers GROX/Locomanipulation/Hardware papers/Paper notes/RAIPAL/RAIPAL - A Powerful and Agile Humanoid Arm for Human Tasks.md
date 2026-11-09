---
title: "RAIPAL: A Powerful and Agile Humanoid Arm for Human Tasks"
aliases:
  - RAIPAL
authors:
  - "[[Donggeon Kim]]"
  - "[[Hyungho Chris Choi]]"
  - "[[Donghyuk Choi]]"
  - "[[Jeongsoo Park]]"
  - "[[Choongin Lee]]"
  - "[[Chan Park]]"
  - "[[Jemin Hwangbo]]"
corresponding: "[[Jemin Hwangbo]]"
institutions:
  - "[[Korea Advanced Institute of Science and Technology]]"
labs:
  - "[[RaiLab]]"
countries:
  - South Korea
year: 2026
date: 2026-11-03
venue: IEEE Robotics and Automation Letters (RA-L) 2026
venue_type: journal
arxiv_category: cs.RO
arxiv_id: "2611.04334"
url: https://arxiv.org/abs/2611.04334
pdf: https://arxiv.org/pdf/2611.04334
code: "No official repo yet (checked 2026-11-08)"
status: to-read
relevance: medium
added: 2026-11-08
tags:
  - paper
  - topic/humanoids
  - topic/hardware-design
  - topic/actuators
  - topic/manipulation
  - topic/biomechanics
  - topic/arm-design
  - method/variable-transmission
  - robot/raipal
  - eval/real-robot
---

# RAIPAL

> [!info] Quick view
> **Authors:** [[Donggeon Kim]], [[Hyungho Chris Choi]], [[Donghyuk Choi]], [[Jeongsoo Park]], [[Choongin Lee]], [[Chan Park]], [[Jemin Hwangbo]] (corresponding)
> **From:** [[Korea Advanced Institute of Science and Technology]] · [[RaiLab]]
> **Published:** IEEE RA-L 2026 · Nov 2026 (arXiv) · cs.RO
> **Keywords:** humanoid arm, human-task-derived requirements, 3K compound planetary, posture-dependent elbow, crossed four-bar, thermal safe operating area, endpoint force, torque transparency
> **Links:** [arXiv](https://arxiv.org/abs/2611.04334) · [PDF](https://arxiv.org/pdf/2611.04334) · [Video](https://youtu.be/xVUWFICD5G0)
> **One line:** a 7-DoF, 8.9 kg humanoid arm sized from human upper-limb measurements instead of worst-case workspace specs, with a strong upper arm, a fast forearm and an elbow whose reduction changes with posture, reaching 20 kg payload and 20.6 m/s endpoint speed

## Summary

Humanoid arms usually end up either strong and slow (high reduction drives sized for worst-case workspace forces) or fast and weak (quasi-direct-drive). This paper argues the problem is how requirements are chosen, not only the actuator tech. They take published human data (joint torques, peak joint velocities in sports, segment lengths and masses, Rohmert's endurance curve) and use mean + one std of an 80 kg, 1.80 m male as targets. Torque targets are converted to endpoint force with the Jacobian, which avoids oversizing the proximal joints.

From that they get a force-oriented upper arm (four identical 22:1 3K compound planetary actuators), a speed-oriented forearm (7.33:1 single-stage planetary, very low inertia) and a crossed four-bar elbow that gives more torque when flexed and more speed when extended, like the human elbow. On the real arm they show 20 kg held for 60 s, 5 kg per arm for 25 min, 20.6 m/s pitching and 70 hammer strikes without damage. Honestly the design method is clean and easy to reuse, but all motions are hand designed joint trajectories, so it says nothing yet about how this arm behaves under a learned whole-body controller.

## How it works

- **Human prior.** Five task characteristics drive the design: compliant feedback-driven accuracy, contracted-posture strength in the upper arm, extended-posture speed in the forearm, the elbow mediating both, and load-dependent endurance
- **Targets.** Per joint, mean + σ of published measurements (about the top 16% of men). Endpoint force for each joint is the minimum gravity-compensated end-effector force across human worst-case poses, for example 129 N for shoulder flexion
- **Upper-arm actuator.** 1.5 kg, 22:1 3K (Wolfrom) compound planetary, 56 Nm rated for 30 min, 180 Nm peak, 12.8 rad/s. The gear preload was chosen empirically to trade backlash against static friction
- **Forearm actuator.** 0.36 kg, 7.33:1 planetary, 3.88 Nm rated, 12.61 Nm peak, 89.7 rad/s, reflected inertia about 100× lower than the upper-arm one
- **Elbow.** Crossed four-bar linkage with effective ratio going from 0.531 near extension to 0.875 near flexion. It also moves actuator mass toward the shoulder, cutting the effective inertia seen by the shoulder joints by 15%
- **Electronics and thermal.** Driver embedded in each actuator on a stacked dual board (power stage separated from sensing). Heat goes passively into the chassis and arm structure, no fans. A first-order RC thermal model per axis gives continuous torque limits
- **Control in the demos.** Joint impedance controller at 15 kHz with targets at 2 kHz and fixed PD gains, trajectories designed by hand

## Results that matter

- Arm is 8.9 kg vs an estimated 12.3 kg matched human reference. Peak payload 20 kg (2.25× own mass) for 60 s including an extended pose, and the elbow torque goes above 100 Nm, which a fixed low reduction tuned for pitching speed could not give
- Pitching reaches 20.6 m/s endpoint speed, about 130% of the average human reference. In the comparison table only AthenaZero is faster (31 m/s), and that uses the whole torso
- Endpoint force beats the targets on 6 of 7 axes, but wrist flexion is 78.6 N vs a 79 N target, so basically equal and slightly under
- Upper-arm actuator efficiency is about 88 to 93% forward and around 84% backward
- The thermal safe operating area encloses the human endurance curve over almost the whole load range. 5 kg per arm through worst-case poses for 25 min stays under the temperature limit
- Hammering: 70 strikes at 3.8 m/s average, about 32 J of arm kinetic energy dissipated at each impact, no damage or maintenance

## Interesting stuff

- Using endpoint force instead of matching joint torques directly is a simple but smart move. It's the reason the arm is lighter than the human reference and still exceeds the force targets
- The posture-dependent elbow is basically a mechanical gear shift tied to the configuration. Same idea as the variable ratio crossed four-bar legs, now used in an arm
- Comparing the thermal SOA with Rohmert's human endurance curve is a nice way to say "it can work as long as a person" in numbers
- **Weak point.** All demos use manually designed trajectories, there is no hand, and the arm is mounted on a fixed stand. No autonomy or learning yet, and it's not tested on a moving humanoid body
- **Weak point.** The comparison table mixes peer-reviewed papers, product pages and blog posts, and many cells are N/A, so the "best payload to mass ratio" claim is a bit loose

## Useful for me

- **Apply.** The requirement method (human data with mean + σ, then converting to endpoint force through the Jacobian over a set of human poses) is a clean way to justify actuator sizing in a design paper instead of picking numbers by hand
- **As prior work.** The posture-dependent elbow shows how a mechanism can change the torque-speed trade-off depending on configuration. That's a close relative of a humanoid that changes its body to fit the task, just done passively through the linkage
- **Apply.** The RC thermal model plus quasi-adiabatic short-overload limit is easy to reproduce and gives realistic torque and duration limits to put in simulation, so a learned policy does not exploit torques the real motors cannot hold
- **Watch out.** The arm is fast and torque transparent, but the paper only shows hand designed motions. Any learning-based whole-body control on this kind of arm would still need actuator modeling for the variable elbow ratio (they mention a polynomial surrogate for simulation)

## Key prior work

- [[Development of a Human Size and Strength Compliant Bi-Manual Platform for Realistic Heavy Manipulation Tasks|IIT Bi-Manual Platform]] · the closest strong humanoid arm in their comparison, 15 kg payload with similar mass
- [[A Low-Cost and Lightweight 6 DoF Bimanual Arm for Dynamic and Contact-Rich Manipulation|ARMADA]] · the fast and light quasi-direct-drive side of the trade-off
- [[Anthropomorphic low-inertia high-stiffness manipulator for high-speed safe interaction|LIMS]] · low-inertia anthropomorphic arm, the reference for fast safe arms
- [[Design of robot leg with variable reduction ratio crossed four-bar linkage mechanism|Variable-Ratio Crossed Four-Bar Leg]] · where the posture-dependent elbow idea comes from
- [[Bilateral Drive Gear - A Highly Backdrivable Reduction Gearbox for Robotic Actuators|Bilateral Drive Gear]] · background for efficiency and backdrivability of compound gear trains
- [[Raibo2 - Highly Efficient Quadruped Robot Completing Full Marathon with a Single Battery Charge|Raibo2]] · same lab, the actuator and thermal modeling base reused here
- [[Problems in Determining Rest Allowances - Part 1 - Use of Modern Methods to Evaluate Stress and Strain in Static Muscular Work|Rohmert Endurance Curve]] · the human endurance boundary they compare the thermal limits against

## References

1. [[Development of a Human Size and Strength Compliant Bi-Manual Platform for Realistic Heavy Manipulation Tasks|IIT Bi-Manual Platform]] · Baccelliere et al. · 2017 · IROS · [Scholar](https://scholar.google.com/scholar?q=Development+of+a+Human+Size+and+Strength+Compliant+Bi-Manual+Platform+for+Realistic+Heavy+Manipulation+Tasks)
2. [[AthenaZero - A Bimanual Robot for Dynamic Manipulation|AthenaZero]] · Robotics and AI Institute · 2026 · Blog post · [Link](https://rai-inst.com/resources/blog/)
3. [[Biomechanics and Motor Control of Human Movement|Winter Biomechanics]] · Winter · 2009 · Book (4th ed., Wiley) · [Scholar](https://scholar.google.com/scholar?q=Biomechanics+and+Motor+Control+of+Human+Movement)
4. [[Modelling and Control of Robot Manipulators|Sciavicco & Siciliano]] · Sciavicco & Siciliano · 2000 · Book (2nd ed., Springer) · [Scholar](https://scholar.google.com/scholar?q=Modelling+and+Control+of+Robot+Manipulators)
5. [[A Low-Cost and Lightweight 6 DoF Bimanual Arm for Dynamic and Contact-Rich Manipulation|ARMADA]] · Kim et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.16908)
6. [[The KUKA-DLR Lightweight Robot Arm - A New Reference Platform for Robotics Research and Manufacturing|KUKA-DLR LWR]] · Bischoff et al. · 2010 · ISR/ROBOTIK · [Scholar](https://scholar.google.com/scholar?q=The+KUKA-DLR+Lightweight+Robot+Arm+A+New+Reference+Platform+for+Robotics+Research+and+Manufacturing)
7. [[D3-Arm - High-Dynamic, Dexterous and Fully Decoupled Cable-Driven Robotic Arm|D3-Arm]] · Luo et al. · 2025 · ICRA · [Scholar](https://scholar.google.com/scholar?q=D3-Arm+High-Dynamic+Dexterous+and+Fully+Decoupled+Cable-Driven+Robotic+Arm)
8. [[Anthropomorphic low-inertia high-stiffness manipulator for high-speed safe interaction|LIMS]] · Kim · 2017 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Anthropomorphic+Low-Inertia+High-Stiffness+Manipulator+for+High-Speed+Safe+Interaction)
9. [[DynaArm - Ultra-Lightweight Robotic Arm|DynaArm]] · Duatic AG · 2026 · Product page · [Link](https://www.duatic.com/dynaarm)
10. [[The Reserve of Joint Torque Determines Movement Coordination|Reserve of Joint Torque]] · Faity et al. · 2021 · Scientific Reports · [Scholar](https://scholar.google.com/scholar?q=The+Reserve+of+Joint+Torque+Determines+Movement+Coordination)
11. [[Kinetic Chain of Overarm Throwing in Terms of Joint Rotations Revealed by Induced Acceleration Analysis|Overarm Throwing Kinetic Chain]] · Hirashima et al. · 2008 · J. Biomech. · [Scholar](https://scholar.google.com/scholar?q=Kinetic+Chain+of+Overarm+Throwing+in+Terms+of+Joint+Rotations+Revealed+by+Induced+Acceleration+Analysis)
12. [[Muscles Across the Elbow Joint - A Biomechanical Analysis|Elbow Muscles]] · An et al. · 1981 · J. Biomech. · [Scholar](https://scholar.google.com/scholar?q=Muscles+Across+the+Elbow+Joint+A+Biomechanical+Analysis)
13. [[Problems in Determining Rest Allowances - Part 1 - Use of Modern Methods to Evaluate Stress and Strain in Static Muscular Work|Rohmert Endurance Curve]] · Rohmert · 1973 · Applied Ergonomics · [Scholar](https://scholar.google.com/scholar?q=Problems+in+Determining+Rest+Allowances+Part+1+Use+of+Modern+Methods+to+Evaluate+Stress+and+Strain+in+Static+Muscular+Work)
14. [[Isokinetic Norms for Ankle, Knee, Shoulder and Forearm Muscles in Young South African Men|Isokinetic Norms]] · Lategan · 2011 · Isokinetics and Exercise Science · [Scholar](https://scholar.google.com/scholar?q=Isokinetic+Norms+for+Ankle+Knee+Shoulder+and+Forearm+Muscles+in+Young+South+African+Men)
15. [[Effect of Two Different Handgrip Positions on Elbow Peak Torque Values|Handgrip Elbow Torque]] · Lategan & Krüger · 2007 · S. Afr. J. Res. Sport Phys. Educ. Recreation · [Scholar](https://scholar.google.com/scholar?q=Effect+of+Two+Different+Handgrip+Positions+on+Elbow+Peak+Torque+Values)
16. [[Maximum Isometric Moments Generated by the Wrist Muscles in Flexion-Extension and Radial-Ulnar Deviation|Wrist Isometric Moments]] · Delp et al. · 1996 · J. Biomech. · [Scholar](https://scholar.google.com/scholar?q=Maximum+Isometric+Moments+Generated+by+the+Wrist+Muscles+in+Flexion-Extension+and+Radial-Ulnar+Deviation)
17. [[Maximum Velocities in Flexion and Extension Actions for Sport|Max Joint Velocities in Sport]] · Jessop & Pain · 2016 · J. Hum. Kinet. · [Scholar](https://scholar.google.com/scholar?q=Maximum+Velocities+in+Flexion+and+Extension+Actions+for+Sport)
18. [[Ergonomics and Work-Related Musculoskeletal Disorders|Ergonomics and MSDs]] · Keyserling & Armstrong · 2008 · Book chapter (McGraw-Hill) · [Scholar](https://scholar.google.com/scholar?q=Ergonomics+and+Work-Related+Musculoskeletal+Disorders)
19. [[Bilateral Drive Gear - A Highly Backdrivable Reduction Gearbox for Robotic Actuators|Bilateral Drive Gear]] · Matsuki et al. · 2019 · IEEE/ASME T-Mech · [Scholar](https://scholar.google.com/scholar?q=Bilateral+Drive+Gear+A+Highly+Backdrivable+Reduction+Gearbox+for+Robotic+Actuators)
20. [[Raibo2 - Highly Efficient Quadruped Robot Completing Full Marathon with a Single Battery Charge|Raibo2]] · Lee et al. · 2025 · Research Square preprint · [DOI](https://doi.org/10.21203/rs.3.rs-6040970/v1)
21. [[Design of robot leg with variable reduction ratio crossed four-bar linkage mechanism|Variable-Ratio Crossed Four-Bar Leg]] · Tomishiro et al. · 2019 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+of+robot+leg+with+variable+reduction+ratio+crossed+four-bar+linkage+mechanism)
22. [[A Robotic Model to Investigate Human Motor Control|Robotic Model of Human Motor Control]] · Lenzi et al. · 2011 · Biological Cybernetics · [Scholar](https://scholar.google.com/scholar?q=A+Robotic+Model+to+Investigate+Human+Motor+Control)
23. [[Upper Limb Muscle Volumes in Adult Subjects|Upper Limb Muscle Volumes]] · Holzbaur et al. · 2007 · J. Biomech. · [Scholar](https://scholar.google.com/scholar?q=Upper+Limb+Muscle+Volumes+in+Adult+Subjects)

## See also
