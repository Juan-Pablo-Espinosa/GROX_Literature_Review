---
title: "The Design of Stretch: A Compact, Lightweight Mobile Manipulator for Indoor Human Environments"
aliases:
  - Stretch
  - Stretch RE1
authors:
  - "[[Charles C. Kemp]]"
  - "[[Aaron Edsinger]]"
  - "[[Henry M. Clever]]"
  - "[[Blaine Matulevich]]"
corresponding: ""
institutions:
  - "[[Georgia Institute of Technology]]"
  - "[[Hello Robot Inc.]]"
labs:
  - "[[Healthcare Robotics Lab]]"
countries:
  - USA
year: 2022
date: 2022-05-23
venue: IEEE ICRA 2022
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://scholar.google.com/scholar?q=The+Design+of+Stretch+A+Compact+Lightweight+Mobile+Manipulator+for+Indoor+Human+Environments
pdf: ""
code: https://github.com/hello-robot
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/manipulation
  - topic/loco-manipulation
  - topic/hardware-design
  - topic/variable-length-limbs
  - topic/teleoperation
  - robot/stretch
  - eval/real-robot
---

# Stretch

> [!info] Quick view
> **Authors:** [[Charles C. Kemp]], [[Aaron Edsinger]], [[Henry M. Clever]], [[Blaine Matulevich]]
> **From:** [[Georgia Institute of Technology]] · [[Healthcare Robotics Lab]] · [[Hello Robot Inc.]]
> **Published:** IEEE ICRA 2022 · May 2022 · conference
> **Keywords:** mobile manipulator, telescoping arm, lift, Cartesian manipulation, static stability, anthropometry, home robot, compliant gripper
> **Links:** [Scholar](https://scholar.google.com/scholar?q=The+Design+of+Stretch+A+Compact+Lightweight+Mobile+Manipulator+for+Indoor+Human+Environments) · [Code](https://github.com/hello-robot)
> **One line:** a 23 kg, 34 cm wide home robot made of a differential base, a vertical lift and a horizontal telescoping arm, so four actuators give Cartesian end-effector motion

## Summary

The authors argue that mobile manipulators are not adopted because they are too big, heavy and expensive, and that indoor environments are Cartesian (floors, counters, shelves). So Stretch is minimal: a two-wheel differential base, a mast with a lift, and a carbon-fiber telescoping arm perpendicular to the driving direction. Driving the base moves the gripper left and right, the lift up and down, the arm in and out, so the Jacobian is the identity over the whole workspace. A wrist yaw stows the tool inside the footprint, an optional Dex Wrist adds pitch and roll, and the gripper is adapted from a cheap commercial reacher-grabber. Dimensions come from anthropometry: 34 cm wide (less than median hip width), 71 cm reach (median arm length), camera at 131 cm, reach from floor to 110 cm.

The trade-off between being small and light and still being stable is modeled with simple tipping equations over a triangular support polygon: payload ∝ robot mass / reach, push/pull force ∝ mass / arm height. The models predict 3.47 kg max payload at full reach and the measurement is 3.46 kg, and pull force measurements also match. They show 19 tasks in real homes, mostly teleoperated and some autonomous. Honestly it is a design paper with very simple models, but the core idea, use prismatic joints that match the structure of the environment, is strong and the robot clearly worked commercially.

## How it works

- Four main actuators: two drive wheels, lift, telescoping arm, all low gear ratio so contact is sensed through motor current
- Two modes: navigation (arm stowed, drive forward) and manipulation (arm extends sideways, base gives the third Cartesian axis)
- Wrist yaw with 330° range, optional Dex Wrist (pitch and roll) making it 7 DoF
- Sensing: RealSense D435i rotated 90° on a pan-tilt head, laser range finder, IMU, microphone array, wrist accelerometers
- Stability model with a triangular support (two drive wheels and a passive omni wheel): payload = m_r c / (2l d_p/w + t), pull/push = m_r g c w / (2 l d_F)
- Telescoping arm with n segments: widening the base by i cm can extend reach by up to n·i cm

## Results that matter

- 23 kg, 34 cm wide, about 20k USD, vs Fetch 113 kg, TIAGo 70 kg, PR2 220 kg
- Predicted payload at 0.693 m reach 3.47 kg vs measured 3.46 kg
- Pull force model matches measurements at different heights, with arm retracted and extended
- Hardware can pull > 70 N and lift > 6 kg when the base is anchored, so static stability is the real limit
- 19 tasks in homes: laundry, watering plants, opening drawers with haptic sensing, opening doors with the Dex Wrist, pouring

## Interesting stuff

- "Robotic cubism": deconstruct the human body and keep only the dimensions that matter for the environment
- Prismatic joints make contact detection trivial because non-contact loads do not change much with configuration
- Static stability, not actuator strength, limits the robot, a direct consequence of being light
- Most tasks are teleoperated, so the autonomy claims are limited

## Useful for me

- **As prior work.** Reference for telescoping limbs in a successful product, and for matching body dimensions to human anthropometry and environment structure.
- **Apply.** The tipping models are a quick way to check payload and force limits of a humanoid with variable limb length: extending a limb moves the CoM and changes the support margin, exactly like extending Stretch's arm.
- **Watch out.** A light robot with long reach is stability limited, so for a growing humanoid, extending limbs to reach farther reduces the force it can apply.

## Key prior work

- [[Analysis of Task Feasibility for a Home Robot using Prismatic Joints|PRISM]] · earlier prismatic arm for home tasks
- [[Towards a personal robotics development platform - Rationale and design of an intrinsically safe personal robot|PR1]] · personal robot design reference
- [[Development of human support robot as the research platform of a domestic mobile manipulator|HSR]] · Toyota home mobile manipulator
- [[Fetch and Freight - Standard Platforms for Service Robot Applications|Fetch]] · comparison mobile manipulator
- [[Maximum load-carrying capacity of autonomous mobile manipulator in an environment with obstacle considering tip over stability|Tip-over load capacity]] · stability vs load trade-off
- [[A list of household objects for robotic retrieval prioritized by people with ALS|ALS object list]] · task requirements for assistive retrieval
- [[On Growth and Form|On Growth and Form]] · scaling arguments for size

## References

1. [[EL-E - An assistive robot that fetches objects from flat surfaces|EL-E HRI]] · Nguyen et al. · 2008 · HRI Workshop · [Scholar](https://scholar.google.com/scholar?q=EL-E+An+assistive+robot+that+fetches+objects+from+flat+surfaces)
2. [[Towards a personal robotics development platform - Rationale and design of an intrinsically safe personal robot|PR1]] · Wyrobek et al. · 2008 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Towards+a+personal+robotics+development+platform+Rationale+and+design+of+an+intrinsically+safe+personal+robot)
3. [[EL-E - an assistive mobile manipulator that autonomously fetches objects from flat surfaces|EL-E]] · Jain & Kemp · 2010 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=EL-E+an+assistive+mobile+manipulator+that+autonomously+fetches+objects+from+flat+surfaces)
4. [[A field study of the human support robot in the home environment|HSR field study]] · Hashimoto et al. · 2013 · ARSO · [Scholar](https://scholar.google.com/scholar?q=A+field+study+of+the+human+support+robot+in+the+home+environment)
5. [[Robots for humanity - using assistive robotics to empower people with disabilities|Robots for humanity]] · Chen et al. · 2013 · IEEE RAM · [Scholar](https://scholar.google.com/scholar?q=Robots+for+humanity+using+assistive+robotics+to+empower+people+with+disabilities)
6. [[In-home and remote use of robotic body surrogates by people with profound motor deficits|Robotic body surrogates]] · Grice & Kemp · 2019 · PLOS ONE · [Scholar](https://scholar.google.com/scholar?q=In-home+and+remote+use+of+robotic+body+surrogates+by+people+with+profound+motor+deficits)
7. [[Towards autonomous robotic butlers - Lessons learned with the PR2|PR2 butlers]] · Bohren et al. · 2011 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Towards+autonomous+robotic+butlers+Lessons+learned+with+the+PR2)
8. [[Searching objects in large-scale indoor environments - A decision-theoretic approach|Object search indoors]] · Kunze et al. · 2012 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Searching+objects+in+large-scale+indoor+environments+A+decision-theoretic+approach)
9. [[Failure recovery with shared autonomy|Shared autonomy recovery]] · Sankaran et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=Failure+recovery+with+shared+autonomy)
10. [[Integrated task and motion planning in belief space|Belief-space TAMP]] · Kaelbling & Lozano-Pérez · 2013 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Integrated+task+and+motion+planning+in+belief+space)
11. [[Older adults' medication management in the home - How can robots help|Medication management robots]] · Prakash et al. · 2013 · HRI · [Scholar](https://scholar.google.com/scholar?q=Older+adults+medication+management+in+the+home+How+can+robots+help)
12. [[Replacing the office intern - An autonomous coffee run with a mobile manipulator|Coffee run]] · Pratkanis et al. · 2013 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Replacing+the+office+intern+An+autonomous+coffee+run+with+a+mobile+manipulator)
13. [[Charlie rides the elevator - integrating vision, navigation and manipulation towards multi-floor robot locomotion|Charlie elevator]] · Troniak et al. · 2013 · CRV · [Scholar](https://scholar.google.com/scholar?q=Charlie+rides+the+elevator+integrating+vision+navigation+and+manipulation+towards+multi-floor+robot+locomotion)
14. [[POOP SCOOP - Perception Of Offensive Products and Sensorized Control Of Object Pickup|POOP SCOOP]] · Cohen et al. · 2011 · IROS PR2 Workshop · [Scholar](https://scholar.google.com/scholar?q=POOP+SCOOP+Perception+Of+Offensive+Products+and+Sensorized+Control+Of+Object+Pickup)
15. [[Learning generalizable surface cleaning actions from demonstration|Surface cleaning LfD]] · Elliott et al. · 2017 · RO-MAN · [Scholar](https://scholar.google.com/scholar?q=Learning+generalizable+surface+cleaning+actions+from+demonstration)
16. [[Multi-task hierarchical imitation learning for home automation|Hierarchical IL home]] · Fox et al. · 2019 · CASE · [Scholar](https://scholar.google.com/scholar?q=Multi-task+hierarchical+imitation+learning+for+home+automation)
17. [[HERB - a home exploring robotic butler|HERB]] · Srinivasa et al. · 2010 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Herb+a+home+exploring+robotic+butler)
18. [[Robot, organize my shelves! Tidying up objects by predicting user preferences|Organize my shelves]] · Abdo et al. · 2015 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Robot+organize+my+shelves+tidying+up+objects+by+predicting+user+preferences)
19. [[The robot household marathon experiment|Household marathon]] · Kazhoyan et al. · 2021 · ICRA · [arXiv](https://arxiv.org/abs/2011.09792)
20. [[Cloth grasp point detection based on multiple-view geometric cues with application to robotic towel folding|Towel folding]] · Maitin-Shepard et al. · 2010 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Cloth+grasp+point+detection+based+on+multiple-view+geometric+cues+with+application+to+robotic+towel+folding)
21. [[A geometric approach to robotic laundry folding|Laundry folding]] · Miller et al. · 2012 · IJRR · [Scholar](https://scholar.google.com/scholar?q=A+geometric+approach+to+robotic+laundry+folding)
22. [[Robotics for occupational therapy - Learning upper-limb exercises from demonstrations|Occupational therapy robotics]] · Hu et al. · 2021 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Robotics+for+occupational+therapy+Learning+upper-limb+exercises+from+demonstrations)
23. [[Skill learning and task outcome prediction for manipulation|Skill learning and outcome prediction]] · Pastor et al. · 2011 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Skill+learning+and+task+outcome+prediction+for+manipulation)
24. [[Roboscopie - A theatre performance for a human and a robot|Roboscopie]] · Lemaignan et al. · 2012 · HRI · [DOI](https://doi.org/10.1145/2157689.2157831)
25. [[Exploring low-cost mobile manipulation for elder care within a community based setting|Low-cost elder care manipulation]] · Mucchiani et al. · 2020 · J. Intell. Robot. Syst. · [Scholar](https://scholar.google.com/scholar?q=Exploring+low-cost+mobile+manipulation+for+elder+care+within+a+community+based+setting)
26. [[Maximum load-carrying capacity of autonomous mobile manipulator in an environment with obstacle considering tip over stability|Tip-over load capacity]] · Korayem et al. · 2010 · Int. J. Adv. Manuf. Technol. · [Scholar](https://scholar.google.com/scholar?q=Maximum+load-carrying+capacity+of+autonomous+mobile+manipulator+in+an+environment+with+obstacle+considering+tip+over+stability)
27. [[KUKA youBot - a mobile manipulator for research and education|youBot]] · Bischoff et al. · 2011 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Kuka+youbot+a+mobile+manipulator+for+research+and+education)
28. [[Dusty - an assistive mobile manipulator that retrieves dropped objects for people with motor impairments|Dusty]] · King et al. · 2012 · Disabil. Rehabil. Assist. Technol. · [DOI](https://doi.org/10.3109/17483107.2011.615374)
29. [[Teaching robotics software with the open hardware mobile manipulator|Open hardware mobile manipulator]] · Vona & Shekar · 2013 · IEEE Trans. Education · [Scholar](https://scholar.google.com/scholar?q=Teaching+robotics+software+with+the+open+hardware+mobile+manipulator)
30. [[Affordable modular mobile manipulator for domestic applications|Affordable modular manipulator]] · Mersha et al. · 2018 · REM · [Scholar](https://scholar.google.com/scholar?q=Affordable+modular+mobile+manipulator+for+domestic+applications)
31. [[Robot learning in homes - Improving generalization and reducing dataset bias|Robot learning in homes]] · Gupta et al. · 2018 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Robot+learning+in+homes+Improving+generalization+and+reducing+dataset+bias)
32. [[PyRobot - An Open-source Robotics Framework for Research and Benchmarking|PyRobot]] · Murali et al. · 2019 · arXiv · [arXiv](https://arxiv.org/abs/1906.08236)
33. [[On Growth and Form|On Growth and Form]] · Thompson · 1942 · Cambridge University Press · [Scholar](https://scholar.google.com/scholar?q=Thompson+On+growth+and+form)
34. [[On Size and Life|On Size and Life]] · McMahon & Bonner · 1983 · Scientific American Books · [Scholar](https://scholar.google.com/scholar?q=McMahon+Bonner+On+size+and+life)
35. [[Multimodal tactile perception of objects in a real home|Tactile perception in a home]] · Bhattacharjee et al. · 2018 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Multimodal+tactile+perception+of+objects+in+a+real+home)
36. [[Human support robot (HSR)|HSR SIGGRAPH]] · Yamamoto et al. · 2018 · SIGGRAPH Emerging Technologies · [Scholar](https://scholar.google.com/scholar?q=Human+support+robot+HSR+SIGGRAPH+2018)
37. [[Development of human support robot as the research platform of a domestic mobile manipulator|HSR]] · Yamamoto et al. · 2019 · ROBOMECH Journal · [Scholar](https://scholar.google.com/scholar?q=Development+of+human+support+robot+as+the+research+platform+of+a+domestic+mobile+manipulator)
38. [[Fetch mobile manipulator specifications|Fetch specs]] · Fetch Robotics · 2021 · web page · [Link](https://fetchrobotics.com/fetch-mobile-manipulator/)
39. [[Fetch and Freight - Standard Platforms for Service Robot Applications|Fetch]] · Wise et al. · 2016 · Workshop on Autonomous Mobile Service Robots · [Scholar](https://scholar.google.com/scholar?q=Fetch+and+freight+Standard+platforms+for+service+robot+applications)
40. [[Private communication with academic customer about Fetch price|Fetch price note]] · private communication · 2018 · email · n/a
41. [[TIAGo technical specifications|TIAGo specs]] · PAL Robotics · 2021 · datasheet · [Link](https://pal-robotics.com/wp-content/uploads/2021/07/Datasheet-complete_TIAGo-2021.pdf)
42. [[Call for loan of TIAGo Steel robots|TIAGo price note]] · euRobotics · 2017 · web page · [Scholar](https://scholar.google.com/scholar?q=Call+for+loan+of+TIAGo+Steel+robots)
43. [[Willow Garage retrospective (ROS Topics)|Willow Garage retrospective]] · Cousins · 2014 · IEEE RAM · [Scholar](https://scholar.google.com/scholar?q=Willow+garage+retrospective+ROS+Topics)
44. [[PR2 user manual|PR2 manual]] · Willow Garage · 2012 · manual · [Scholar](https://scholar.google.com/scholar?q=PR2+user+manual+Willow+Garage)
45. [[PR2 - Hardware specs|PR2 specs]] · Willow Garage · 2011 · web archive · [Link](https://web.archive.org/web/20110807162103/http://www.willowgarage.com/pages/pr2/specs)
46. [[iRobot's Roomba Robotic Floorvac Surpasses 1 Million In Sales|Roomba 1 million]] · iRobot · 2004 · press release · [Scholar](https://scholar.google.com/scholar?q=iRobot+Roomba+Robotic+Floorvac+Surpasses+1+Million+In+Sales)
47. [[How to build complete creatures rather than isolated cognitive simulators|Complete creatures]] · Brooks · 1989 · Architectures for Intelligence · [Scholar](https://scholar.google.com/scholar?q=How+to+build+complete+creatures+rather+than+isolated+cognitive+simulators)
48. [[NIOSH Work Practices Guide for Manual Lifting|NIOSH lifting guide]] · NIOSH · 1981 · guide · [Link](https://www.cdc.gov/niosh/docs/81-122/pdfs/81-122.pdf)
49. [[Arms akimbo male (silhouette clipart)|Clipart arms akimbo]] · algotruneman · 2016 · Openclipart · [Link](https://openclipart.org/detail/260142/arms-akimbo-male)
50. [[Outstretched arm with grey background|Outstretched arm image]] · Gunnarsson · 2016 · Wikimedia Commons · [Link](https://commons.wikimedia.org/wiki/File:Extended_arm.jpg)
51. [[Cubism (Wikipedia)|Cubism]] · Wikipedia · 2022 · web page · [Link](http://en.wikipedia.org/w/index.php?title=Cubism&oldid=1068295802)
52. [[The Measure of Man and Woman - Human Factors in Design|Measure of Man and Woman]] · Tilley · 2002 · Wiley · [Scholar](https://scholar.google.com/scholar?q=The+measure+of+man+and+woman+human+factors+in+design)
53. [[iRobot Introduces Roomba Intelligent FloorVac|Roomba launch]] · iRobot · 2002 · press release · [Link](https://media.irobot.com/)
54. [[An anthropometric study of manual and powered wheelchair users|Wheelchair anthropometry]] · Paquet & Feathers · 2004 · Int. J. Ind. Ergonomics · [Scholar](https://scholar.google.com/scholar?q=An+anthropometric+study+of+manual+and+powered+wheelchair+users)
55. [[Modern Robotics - Mechanics, Planning, and Control|Modern Robotics]] · Lynch & Park · 2017 · Cambridge University Press · [Scholar](https://scholar.google.com/scholar?q=Modern+Robotics+Mechanics+Planning+and+Control)
56. [[Introduction to Robotics - Mechanics and Control|Craig Introduction to Robotics]] · Craig · 2005 · Pearson · [Scholar](https://scholar.google.com/scholar?q=Craig+Introduction+to+Robotics+Mechanics+and+Control)
57. [[Analysis of Task Feasibility for a Home Robot using Prismatic Joints|PRISM]] · Mashimo et al. · 2010 · IROS · [Scholar](https://scholar.google.com/scholar?q=Analysis+of+task+feasibility+for+a+home+robot+using+prismatic+joints)
58. [[User evaluation to apply the robotic arm RAPUDA for an upper-limb disabilities patient's daily life|RAPUDA]] · Wakita et al. · 2012 · ROBIO · [Scholar](https://scholar.google.com/scholar?q=User+evaluation+to+apply+the+robotic+arm+RAPUDA+for+an+upper-limb+disabilities+patient%27s+daily+life)
59. [[Design of a spherical robot arm with the spiral zipper prismatic joint|Spiral zipper arm]] · Collins & Yim · 2016 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+of+a+spherical+robot+arm+with+the+spiral+zipper+prismatic+joint)
60. [[Development and deployment of a mobile manipulator for assisting and entertaining elders living in supportive apartment living facilities|Elder-care mobile manipulator]] · Mucchiani et al. · 2018 · RO-MAN · [Scholar](https://scholar.google.com/scholar?q=Development+and+deployment+of+a+mobile+manipulator+for+assisting+and+entertaining+elders+living+in+supportive+apartment+living+facilities)
61. [[A list of household objects for robotic retrieval prioritized by people with ALS|ALS object list]] · Choi et al. · 2009 · ICORR · [Scholar](https://scholar.google.com/scholar?q=A+list+of+household+objects+for+robotic+retrieval+prioritized+by+people+with+ALS)
62. [[Grasping in the Wild - Learning 6DoF Closed-Loop Grasping from Low-Cost Demonstrations|Grasping in the wild]] · Song et al. · 2020 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Grasping+in+the+wild+Learning+6DoF+closed-loop+grasping+from+low-cost+demonstrations)
63. [[ArUco - a minimal library for augmented reality applications based on OpenCV|ArUco]] · Munoz-Salinas · 2012 · Universidad de Córdoba · [Scholar](https://scholar.google.com/scholar?q=ArUco+a+minimal+library+for+augmented+reality+applications+based+on+opencv)
64. [[The complex structure of simple devices - A survey of trajectories and forces that open doors and drawers|Doors and drawers forces]] · Jain et al. · 2010 · BioRob · [Scholar](https://scholar.google.com/scholar?q=The+complex+structure+of+simple+devices+A+survey+of+trajectories+and+forces+that+open+doors+and+drawers)
65. [[Improving robot manipulation with data-driven object-centric models of everyday forces|Object-centric force models]] · Jain & Kemp · 2013 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Improving+robot+manipulation+with+data-driven+object-centric+models+of+everyday+forces)
66. [[Informing assistive robots with models of contact forces from able-bodied face wiping and shaving|Face wiping forces]] · Hawkins et al. · 2012 · RO-MAN · [Scholar](https://scholar.google.com/scholar?q=Informing+assistive+robots+with+models+of+contact+forces+from+able-bodied+face+wiping+and+shaving)
67. [[An exploration of accessible remote tele-operation for assistive mobile manipulators in the home|Accessible remote teleop]] · Cabrera et al. · 2021 · RO-MAN · [Scholar](https://scholar.google.com/scholar?q=An+exploration+of+accessible+remote+tele-operation+for+assistive+mobile+manipulators+in+the+home)

## See also

- [[Triple Scissor Extender Robot Arm - A Solution to the Last One Foot Problem of Manipulation|Triple Scissor Extender]] · another way to get a long, stiff extension for reaching
