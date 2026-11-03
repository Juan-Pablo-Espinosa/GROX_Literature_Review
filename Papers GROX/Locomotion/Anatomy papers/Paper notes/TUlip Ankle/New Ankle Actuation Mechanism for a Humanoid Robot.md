---
title: "New Ankle Actuation Mechanism for a Humanoid Robot"
aliases:
  - TUlip Ankle
authors:
  - "[[Gijs van Oort]]"
  - "[[Roelof Reinink]]"
  - "[[Stefano Stramigioli]]"
corresponding: ""
institutions:
  - "[[University of Twente]]"
labs: []
countries:
  - Netherlands
year: 2011
date: 2011-08-28
venue: IFAC World Congress 2011 (preprints)
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://scholar.google.com/scholar?q=New+Ankle+Actuation+Mechanism+for+a+Humanoid+Robot+van+Oort
pdf: ""
code: "No code (hardware paper, checked 2026-11-02)"
status: to-read
relevance: low
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/leg-design
  - topic/actuators
  - topic/hardware-design
  - topic/variable-stiffness
  - method/series-elastic-actuation
  - robot/tulip
  - eval/analytical
---

# TUlip Ankle

> [!info] Quick view
> **Authors:** [[Gijs van Oort]], [[Roelof Reinink]], [[Stefano Stramigioli]]
> **From:** [[University of Twente]]
> **Published:** IFAC World Congress · Aug 2011 · conference
> **Keywords:** series elastic actuation, differential ankle, cable drive, limit cycle walking, push-off, MIMO, nonlinearity, TUlip
> **Links:** [Scholar](https://scholar.google.com/scholar?q=New+Ankle+Actuation+Mechanism+for+a+Humanoid+Robot+van+Oort)
> **One line:** a 2-DoF ankle for the TUlip humanoid driven by two small motors through cables and springs in a differential setup, sized from a simplest-walker push-off model

## Summary

TUlip (1.37 m, 19 kg, 17 DoF, SEAs at hip and knee) is a Dutch limit cycle walking humanoid. Its old ankle had a passive spring roll joint and a single-sided bowden cable for pitch with a return spring, which needed so much preload that cable friction made control impossible, and the robot could not stand on one leg. The new ankle uses two Maxon RE25 motors on the shank connected to the foot through cables with series springs, in a differential setup: both motors together give pitch, opposite give roll. So both motors share the high pitch load, and two 20 W motors replace one 60 W plus one 20 W motor.

The motor and gear choice comes from a nice push-off model: a compass walker with feet, where the energy lost at impact must be injected by ankle push-off, giving the walking speed as a function of the max actuator acceleration and speed. The best is two RE25 with 1:117 total ratio for 1.62 m/s theoretical. The control section shows the system is MIMO and nonlinear: the input sensitivity changes up to 32% with foot angle, and because the rotation axes sit 1.6 cm off the cable attachment plane the springs create an unstable equilibrium with up to 2 Nm of torque that must be compensated. The ankle was not tested yet, so it is design plus analysis.

## How it works

- Two RE25 DC motors, pulleys, cables with pretensioned springs (500 N, 50 kN/m) to the front and rear of the foot, guided along wings on the shank
- Differential: same direction gives ankle pitch (θy), opposite gives roll (θx), ranges about [−32°, 35°] pitch and [−23°, 20°] roll
- Sizing: impact loss ½m(1−cos²2α)v² must equal push-off energy J·φ̈·Δφ, plus a speed limit from foot rotation, walking speed = min of both
- Motor choice by plotting each motor and gear ratio on the speed vs acceleration chart
- Linear model: coupled SEA transfer functions, input sensitivity r_x/r and r_y/r
- Nonlinear analysis: large angles and off-plane rotation axes, potential energy of the four springs shows an unstable equilibrium

## Results that matter

- Theoretical max walking speed 1.62 m/s with two RE25 at 1:117, about the same as one RE30 but lighter because no extra roll motor
- Input sensitivity varies up to 32% over the foot workspace
- With 1.6 cm offset of the axes the springs make the ankle unstable away from zero with up to 2 Nm motor-side torque
- No experiments yet

## Interesting stuff

- The push-off sizing method is simple and transferable: size actuators from the energy the gait needs to inject per step
- Differential coupling gives more pitch torque but reduces diagonal torque to that of a single motor
- The off-plane axis problem is a nice warning, a small geometric choice (keeping the foot thin) created an instability
- Very old school limit cycle walking context, no learning

## Useful for me

- **As prior work.** Minor reference for differential and cable-driven ankles with series elasticity.
- **Watch out.** Small geometry offsets in a linkage can create strong nonlinear and even unstable torques. If the limb geometry changes, these effects change too, so they should be in the simulation model.
- **Apply.** Sizing actuators from per-step energy balance is a quick sanity check for motors in a limb whose length changes, since push-off energy and required speed scale with leg length.

## Key prior work

- [[Series elastic actuators|Series elastic actuators]] · Williamson's original SEA thesis
- [[The simplest walking model - Stability, complexity, and scaling|Simplest walker]] · the model behind the sizing
- [[Energetics of actively powered locomotion using the simplest walking model|Kuo push-off energetics]] · why push-off timing matters
- [[Limit Cycle Walking|Limit cycle walking]] · the walking paradigm of TUlip

## References

1. [[Ankle actuation for planar bipedal robots|Franken MSc thesis]] · Franken · 2007 · MSc thesis, University of Twente · [Scholar](https://scholar.google.com/scholar?q=Franken+Ankle+actuation+for+planar+bipedal+robots)
2. [[The simplest walking model - Stability, complexity, and scaling|Simplest walker]] · Garcia et al. · 1998 · ASME J. Biomech. Eng. · [Scholar](https://scholar.google.com/scholar?q=The+simplest+walking+model+Stability+complexity+and+scaling)
3. [[Nonlinear controllers for non-integrable systems - the Acrobot example|Acrobot control]] · Hauser & Murray · 1990 · ACC · [Scholar](https://scholar.google.com/scholar?q=Nonlinear+controllers+for+non-integrable+systems+the+acrobot+example)
4. [[Limit Cycle Walking|Limit cycle walking]] · Hobbelen · 2008 · PhD thesis, TU Delft · [Scholar](https://scholar.google.com/scholar?q=Hobbelen+Limit+Cycle+Walking+thesis)
5. [[Energetics of actively powered locomotion using the simplest walking model|Kuo push-off energetics]] · Kuo · 2002 · ASME J. Biomech. Eng. · [DOI](https://doi.org/10.1115/1.1427703)
6. [[Design and analysis of series elasticity in closed-loop actuator force control|Robinson SEA thesis]] · Robinson · 2000 · PhD thesis, MIT · [Scholar](https://scholar.google.com/scholar?q=Design+and+analysis+of+series+elasticity+in+closed-loop+actuator+force+control)
7. [[Series elastic actuators|Series elastic actuators]] · Williamson · 1995 · MSc thesis, MIT · [Scholar](https://scholar.google.com/scholar?q=Williamson+Series+elastic+actuators+thesis)

## See also

- [[Conceptual Design of a Variable Stiffness Mechanism in a Humanoid Ankle using Parallel Redundant Actuation|Ankle VSM]] · another compliant multi-motor ankle
