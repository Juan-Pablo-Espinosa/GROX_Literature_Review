---
title: "Developmental robotics: a survey"
aliases:
  - Developmental robotics survey
  - Lungarella 2003
authors:
  - "[[Max Lungarella]]"
  - "[[Giorgio Metta]]"
  - "[[Rolf Pfeifer]]"
  - "[[Giulio Sandini]]"
corresponding: "[[Max Lungarella]]"
institutions:
  - "[[National Institute of Advanced Industrial Science and Technology]]"
  - "[[University of Genova]]"
  - "[[University of Zurich]]"
labs:
  - "[[Neuroscience Research Institute]]"
  - "[[LIRA-Lab]]"
  - "[[Artificial Intelligence Laboratory Zurich]]"
countries:
  - Japan
  - Italy
  - Switzerland
year: 2003
date: ""
venue: Connection Science 15(4), 2003
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1080/09540090310001655110
pdf: ""
code: "No official repo (survey paper, checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/survey
  - topic/developmental-robotics
  - topic/embodied-cognition
  - topic/morphology
  - topic/reconfigurable-robots
  - topic/humanoids
  - method/curriculum-learning
  - eval/real-robot
---

# Developmental robotics survey

> [!info] Quick view
> **Authors:** [[Max Lungarella]] (corresponding), [[Giorgio Metta]], [[Rolf Pfeifer]], [[Giulio Sandini]]
> **From:** [[National Institute of Advanced Industrial Science and Technology]] · [[Neuroscience Research Institute]] · [[University of Genova]] · [[LIRA-Lab]] · [[University of Zurich]] · [[Artificial Intelligence Laboratory Zurich]]
> **Published:** Connection Science 15(4), pp. 151–190 · December 2003
> **Keywords:** developmental robotics, embodiment, ontogenetic development, freezing and freeing degrees of freedom, self-exploration, value systems, scaffolding, morphological change
> **Links:** [DOI](https://doi.org/10.1080/09540090310001655110)
> **One line:** foundational survey that defines developmental robotics, lists the facets of infant development a robot can exploit, reviews 29 real-robot studies and points out that morphology change and locomotion were basically untouched

## Summary

This is the reference survey that kind of founded developmental robotics as a field. The authors argue against the mind-as-computer view and for embodiment, then define development as an incremental, constrained and self-organizing process. They go through the facets: development as incremental, early constraints that are lifted over time, Bernstein's freezing and freeing of degrees of freedom, self-exploration, spontaneous movements, anticipatory control, sensorimotor categorization, value systems and neuromodulation, and social interaction and scaffolding. Then they review 29 real-robot studies that explicitly link to a developmental theory, grouped in social interaction, non-social interaction, agent-related control and mechanisms, and summarize the existing frameworks (developmental engineering, cognitive developmental robotics, autonomous mental development).

The two most relevant observations for me are in the discussion. First, there were no developmental studies on robot locomotion at all. Second, the field ignored morphology and materials, and they explicitly ask for robots with physical growth and change of shape, mentioning modular reconfigurable robots and self-repair. It's a 2003 paper so nothing about deep RL, but the conceptual argument that a body which changes over time can make learning easier is still the best framing for a growing robot.

## How it works

- Synthetic methodology: use robots to test developmental models, and use development as a design principle for robots
- Facets of development: incremental process, constraints, self-organization, degrees of freedom, self-exploration, spontaneous activity, anticipation, categorization, value and plasticity, social interaction
- Inclusion criteria for the review: real robot experiments only (no simulation) and explicit link to a developmental psychology or neuroscience hypothesis
- Table of 29 studies grouped in 4 areas: socially oriented interaction, non-social interaction, agent-related control, mechanisms and processes
- Frameworks: developmental engineering (Sandini), cognitive developmental robotics (Asada), autonomous mental development (Weng), Cog project (Brooks)

## Results that matter

- 11 of 29 studies (about 40%) focus on social interaction, mostly gaze, joint attention and imitation
- Very few go beyond early motor skills like pointing, reaching and gazing
- Only 4 studies explicitly use self-exploration
- Only one navigation study and zero locomotion studies with developmental mechanisms
- Morphology, growth and materials flagged as the big missing piece

## Interesting stuff

- Bernstein's freeze then free DoF idea, implemented on a bouncing robot by Lungarella and Berthouze, is basically a curriculum over the body
- "Physical immaturity" as a source of adaptivity: the body being limited at first helps learning, not hurts
- They explicitly say the review is not critical, so it's more a map than an evaluation
- Restriction to real robots is principled but means many relevant sim studies are missing

## Useful for me

- **As prior work.** Classic citation for the claim that changing morphology during learning (growing or freeing DoF) can make learning easier, and for "no one does development on locomotion".
- **Apply.** The freeze-and-free idea maps directly to a curriculum where the policy first learns with a restricted body (short limbs, locked joints) and then the body is extended.
- **Gap.** They ask for robots that physically grow and change shape. A morphing humanoid is exactly that, so this survey gives a 20-year old motivation for it.
- **Watch out.** Very old and conceptual, no numbers, so use it for framing, not as evidence.

## Key prior work

- [[Intelligence without representation|Intelligence without representation]] · Brooks, the embodiment starting point
- [[Understanding Intelligence|Understanding Intelligence]] · Pfeifer and Scheier, embodied cognitive science
- [[A Dynamic Systems Approach to the Development of Cognition and Action|Thelen and Smith]] · dynamic systems view of development
- [[The Co-ordination and Regulation of Movements|Bernstein]] · the degrees of freedom problem
- [[Adaptivity via alternate freeing and freezing of degrees of freedom|Freeing and freezing DoF]] · Bernstein on a robot
- [[On the role of morphology and materials in adaptive behavior|Morphology and materials]] · Pfeifer on why the body matters
- [[Cognitive developmental robotics as a new paradigm for the design of humanoid robots|Cognitive developmental robotics]] · Asada's framework
- [[Autonomous mental development by robots and animals|Autonomous mental development]] · Weng's framework

## References

1. [[Exploration in the service of prospective control|Exploration in the service of]] · Adolph et al. · 2000 · Infant Behavior and Development · [Scholar](https://scholar.google.com/scholar?q=Exploration+in+the+service+of+prospective+control)
2. [[Learning to crawl|Learning to crawl]] · Adolph et al. · 1988 · Child Development · [Scholar](https://scholar.google.com/scholar?q=Learning+to+crawl)
3. [[Behavioral constraints in the development of neuronal properties - a cortical model embedded in a real world device|Behavioral constraints in the development]] · Almassy et al. · 1998 · Cerebral Cortex · [Scholar](https://scholar.google.com/scholar?q=Behavioral+constraints+in+the+development+of+neuronal+properties+a+cortical+model+embedded+in+a+real+world+device)
4. [[From visuo-motor development to low-level imitation|From visuo-motor development to low-level imitation]] · Andry et al. · 2002 · Proceedings of the 2nd International Conference on Epigenetics Robotics · [Scholar](https://scholar.google.com/scholar?q=From+visuo-motor+development+to+low-level+imitation)
5. [[Exploration and selection of intralimb coordination patterns in 3-month-old infants|Exploration and selection of intralimb]] · Angulo-Kinzler · 2001 · Journal of Motor Behavior · [Scholar](https://scholar.google.com/scholar?q=Exploration+and+selection+of+intralimb+coordination+patterns+in+3-month-old+infants)
6. [[Cognitive developmental robotics as a new paradigm for the design of humanoid robots|Cognitive developmental robotics as a]] · Asada et al. · 2001 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Cognitive+developmental+robotics+as+a+new+paradigm+for+the+design+of+humanoid+robots)
7. [[Proceedings of the 1st International Workshop on Epigenetic Robotics - Modeling Cognitive Development in Robotic Systems|EpiRob 2001 proceedings]] · Balkenius et al. (eds) · 2001 · Lund University Cognitive Studies vol. 85 · [Scholar](https://scholar.google.com/scholar?q=Proceedings+of+the+1st+International+Workshop+on+Epigenetic+Robotics+-+Modeling+Cognitive+Development+in+Robotic+Systems)
8. [[Animate vision|Animate vision]] · Ballard · 1991 · Artificial Intelligence · [Scholar](https://scholar.google.com/scholar?q=Animate+vision)
9. [[Mindblindness|Mindblindness]] · Baron-Cohen · 1995 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Mindblindness)
10. [[Connectionism and the study of change|Connectionism and the study of change]] · Bates & Elman · 2002 · Brain Development and Cognition · [Scholar](https://scholar.google.com/scholar?q=Connectionism+and+the+study+of+change)
11. [[Autopoiesis and cognition in the game of life|Autopoiesis and cognition in the]] · Beer · 2003 · in press · [Scholar](https://scholar.google.com/scholar?q=Autopoiesis+and+cognition+in+the+game+of+life)
12. [[Biorobotics approaches to the study of motor systems|Biorobotics approaches to the study]] · Beer et al. · 1998 · Current Opinion in Neurobiology · [Scholar](https://scholar.google.com/scholar?q=Biorobotics+approaches+to+the+study+of+motor+systems)
13. [[The Co-ordination and Regulation of Movements|The Co-ordination and Regulation of Movements]] · Bernstein · 1967 · Pergamon Press · [Scholar](https://scholar.google.com/scholar?q=The+Co-ordination+and+Regulation+of+Movements)
14. [[Eye and trunk control - the foundation for manual development|Eye and trunk control]] · Bertenthal & Von Hofsten · 1998 · Neuroscience and Biobehavioral Reviews · [Scholar](https://scholar.google.com/scholar?q=Eye+and+trunk+control+the+foundation+for+manual+development)
15. [[Visual information and the control of reaching|Visual information and the control]] · Berthier et al. · 1996 · Journal of Motor Behavior · [Scholar](https://scholar.google.com/scholar?q=Visual+information+and+the+control+of+reaching)
16. [[Learning of oculo-motor control - a prelude to robotic imitation|Learning of oculo-motor control]] · Berthouze et al. · 1997 · IEEE/RSJ International Conference on Robotics and Intelligent Systems · [Scholar](https://scholar.google.com/scholar?q=Learning+of+oculo-motor+control+a+prelude+to+robotic+imitation)
17. [[Emergence and categorization of coordinated visual behavior through embodied interaction|Emergence and categorization of coordinated]] · Berthouze & Kuniyoshi · 1998 · Machine Learning · [Scholar](https://scholar.google.com/scholar?q=Emergence+and+categorization+of+coordinated+visual+behavior+through+embodied+interaction)
18. [[Proceedings of the 1st International Workshop on Emergence and Development of Embodied Cognition|EDEC 1999 proceedings]] · Berthouze et al. (eds) · 1999 · Workshop, Tsukuba (unpublished) · [Scholar](https://scholar.google.com/scholar?q=Proceedings+of+the+1st+International+Workshop+on+Emergence+and+Development+of+Embodied+Cognition)
19. [[Dynamic categorization of explorative behaviors for emergence of stable sensorimotor configurations|Dynamic categorization of explorative behaviors]] · Berthouze et al. · 1998 · International Conference on Simulation of Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Dynamic+categorization+of+explorative+behaviors+for+emergence+of+stable+sensorimotor+configurations)
20. [[The adaptive nature of cognitive immaturity|The adaptive nature of cognitive immaturity]] · Bjorklund & Green · 1992 · American Psychologist · [Scholar](https://scholar.google.com/scholar?q=The+adaptive+nature+of+cognitive+immaturity)
21. [[Old Tricks, New Dogs - Ethology and Interactive Creatures|Old Tricks, New Dogs]] · Blumberg · 1996 · PhD thesis, The Media Laboratory, MIT · [Scholar](https://scholar.google.com/scholar?q=Old+Tricks+New+Dogs+Ethology+and+Interactive+Creatures)
22. [[Sensitive periods in development - structural characteristics and causal interpretations|Sensitive periods in development]] · Bornstein · 1989 · Psychological Bulletin · [Scholar](https://scholar.google.com/scholar?q=Sensitive+periods+in+development+structural+characteristics+and+causal+interpretations)
23. [[Vehicles - Experiments in Synthetic Psychology|Vehicles - Experiments in Synthetic Psychology]] · Braitenberg · 1984 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Vehicles+Experiments+in+Synthetic+Psychology)
24. [[Recognition of affective communicative intent in robot-directed speech|Recognition of affective communicative intent]] · Breazeal & Aryananda · 2002 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Recognition+of+affective+communicative+intent+in+robot-directed+speech)
25. [[Infant-like social interactions between a robot and a human caretaker|Infant-like social interactions between a]] · Breazeal & Scassellati · 2000 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Infant-like+social+interactions+between+a+robot+and+a+human+caretaker)
26. [[Robots that imitate humans|Robots that imitate humans]] · Breazeal & Scassellati · 2002 · Trends in Cognitive Science · [Scholar](https://scholar.google.com/scholar?q=Robots+that+imitate+humans)
27. [[Intelligence without representation|Intelligence without representation]] · Brooks · 1991 · Artificial Intelligence · [Scholar](https://scholar.google.com/scholar?q=Intelligence+without+representation)
28. [[From earwigs to humans|From earwigs to humans]] · Brooks · 1997 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=From+earwigs+to+humans)
29. [[Robot - The Future of Flesh and Machines|Robot - The Future of]] · Brooks · 2003 · Penguin Books · [Scholar](https://scholar.google.com/scholar?q=Robot+The+Future+of+Flesh+and+Machines)
30. [[Alternative essences of intelligence|Alternative essences of intelligence]] · Brooks et al. · 1998 · Proceedings of the 15th National Conference on Artificial Intelligence · [Scholar](https://scholar.google.com/scholar?q=Alternative+essences+of+intelligence)
31. [[Building brains for bodies|Building brains for bodies]] · Brooks & Stein · 1994 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Building+brains+for+bodies)
32. [[Before Speech - The Beginning of Interpersonal Communication|Before Speech - The Beginning]] · Bullowa · 1979 · Cambridge University Press · [Scholar](https://scholar.google.com/scholar?q=Before+Speech+The+Beginning+of+Interpersonal+Communication)
33. [[Motor development in the mind - the potential role of motor abilities as a determinant of perceptual development|Motor development in the mind]] · Bushnell & Boudreau · 1993 · Child Development · [Scholar](https://scholar.google.com/scholar?q=Motor+development+in+the+mind+the+potential+role+of+motor+abilities+as+a+determinant+of+perceptual+development)
34. [[What minds have in common in space - spatial mechanisms serving joint visual attention in infancy|What minds have in common]] · Butterworth & Jarrett · 1991 · British Journal of Developmental Psychology · [Scholar](https://scholar.google.com/scholar?q=What+minds+have+in+common+in+space+spatial+mechanisms+serving+joint+visual+attention+in+infancy)
35. [[Functional Movement Development Across the Life Span|Functional Movement Development Across the]] · Chec & Martin · 2002 · W.B. Saunders · [Scholar](https://scholar.google.com/scholar?q=Functional+Movement+Development+Across+the+Life+Span)
36. [[Knowledge of Language - Its Nature, Origin, and Use|Knowledge of Language - Its]] · Chomsky · 1986 · Praeger · [Scholar](https://scholar.google.com/scholar?q=Knowledge+of+Language+Its+Nature+Origin+and+Use)
37. [[A Critique of Pure Vision|A Critique of Pure Vision]] · Churchland et al. · 1994 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=A+Critique+of+Pure+Vision)
38. [[Being There - Putting Brain, Body and World Together Again|Being There - Putting Brain]] · Clark · 1997 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Being+There+Putting+Brain+Body+and+World+Together+Again)
39. [[Towards a cognitive robotics|Towards a cognitive robotics]] · Clark & Grush · 1999 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Towards+a+cognitive+robotics)
40. [[Developing haptic and visual perceptual categories for reaching and grasping with a humanoid robot|Developing haptic and visual perceptual]] · Coehlo et al. · 2001 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Developing+haptic+and+visual+perceptual+categories+for+reaching+and+grasping+with+a+humanoid+robot)
41. [[Sensor and actuators for ‘humanoid’ robots|Sensor and actuators for ‘humanoid’ robots]] · Dario et al. · 1997 · Advanced Robotics · [Scholar](https://scholar.google.com/scholar?q=Sensor+and+actuators+for+%E2%80%98humanoid%E2%80%99+robots)
42. [[Studying robot social cognition within a developmental psychology framework|Studying robot social cognition within]] · Dautenhahn & Billard · 1999 · Proceedings of the 3rd International Workshop on Advanced Mobile Robots · [Scholar](https://scholar.google.com/scholar?q=Studying+robot+social+cognition+within+a+developmental+psychology+framework)
43. [[Imitation in Animals and Artifacts|Imitation in Animals and Artifacts]] · Dautenhahn & Nehaniv · 2002 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Imitation+in+Animals+and+Artifacts)
44. [[‘CAM-brain’ ATR’s billion neuron artificial brain project - a three year progress report|‘CAM-brain’ ATR’s billion neuron artificial]] · De Garis et al. · 1998 · Journal of Artificial Life and Robotics · [Scholar](https://scholar.google.com/scholar?q=%E2%80%98CAM-brain%E2%80%99+ATR%E2%80%99s+billion+neuron+artificial+brain+project+a+three+year+progress+report)
45. [[Robot Imitation Mechanisms in Robots and Humans|Robot Imitation Mechanisms in Robots]] · Demiris · 1999 · PhD thesis, Division of Informatics, University of Edinburgh · [Scholar](https://scholar.google.com/scholar?q=Robot+Imitation+Mechanisms+in+Robots+and+Humans)
46. [[Imitation as a dual-route process featuring predictive and learning components - a biologically plausible computational model|Imitation as a dual-route process]] · Demiris & Hayes · 2002 · Imitation in Animals and Artifacts · [Scholar](https://scholar.google.com/scholar?q=Imitation+as+a+dual-route+process+featuring+predictive+and+learning+components+a+biologically+plausible+computational+model)
47. [[The reflex arc concept in psychology|The reflex arc concept in psychology]] · Dewey · 1896 · Psychological Review · [Scholar](https://scholar.google.com/scholar?q=The+reflex+arc+concept+in+psychology)
48. [[Special issue on plastic mechanisms, multiple timescales, and lifetime adaptation|Special issue on plastic mechanisms]] · Di Paolo · 2002 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Special+issue+on+plastic+mechanisms+multiple+timescales+and+lifetime+adaptation)
49. [[Understanding motor events - a neurophysiological study|Understanding motor events - a]] · Di Pellegrino et al. · 1992 · Experimental Brain Research · [Scholar](https://scholar.google.com/scholar?q=Understanding+motor+events+a+neurophysiological+study)
50. [[Developmental time course in human infants and infant monkeys, and the neural bases of inhibitory control in reaching|Developmental time course in human]] · Diamond · 1990 · The Development and Neural Bases of Higher Cognitive Functions · [Scholar](https://scholar.google.com/scholar?q=Developmental+time+course+in+human+infants+and+infant+monkeys+and+the+neural+bases+of+inhibitory+control+in+reaching)
51. [[Neuromodulation in invertebrate nervous systems|Neuromodulation in invertebrate nervous systems]] · Dickinson · 2003 · MIT Handbook of Brain Theory and Neural Networks · [Scholar](https://scholar.google.com/scholar?q=Neuromodulation+in+invertebrate+nervous+systems)
52. [[Developmental constraints aid the acquisition of binocular disparity sensitivities|Developmental constraints aid the acquisition]] · Dominguez & Jacobs · 2003 · Neural Computation · [Scholar](https://scholar.google.com/scholar?q=Developmental+constraints+aid+the+acquisition+of+binocular+disparity+sensitivities)
53. [[Neural Darwinism - The Theory of Neural Group Selection|Neural Darwinism - The Theory]] · Edelman · 1987 · Basic Books · [Scholar](https://scholar.google.com/scholar?q=Neural+Darwinism+The+Theory+of+Neural+Group+Selection)
54. [[Consciousness - How Matter Becomes Imagination|Consciousness - How Matter Becomes Imagination]] · Edelman · 2001 · Penguin Books · [Scholar](https://scholar.google.com/scholar?q=Consciousness+How+Matter+Becomes+Imagination)
55. [[Growth and repair - instantiating a biologically inspired model of neural development on the Khepera robot|Growth and repair - instantiating]] · Elliott & Shadbolt · 2001 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Growth+and+repair+instantiating+a+biologically+inspired+model+of+neural+development+on+the+Khepera+robot)
56. [[Developmental robotics - manifesto and application|Developmental robotics - manifesto and application]] · Elliott & Shadbolt · 2003 · Philosophical Transactions of Mathematical · [Scholar](https://scholar.google.com/scholar?q=Developmental+robotics+manifesto+and+application)
57. [[Learning and development in neural networks - the importance of starting small|Learning and development in neural]] · Elman · 1993 · Cognition · [Scholar](https://scholar.google.com/scholar?q=Learning+and+development+in+neural+networks+the+importance+of+starting+small)
58. [[Proceedings of the 2nd International Conference on Development and Learning|ICDL 2002 proceedings]] · Elman et al. (eds) · 2002 · Michigan State University (title as printed says Cognitive Development and Learning) · [Scholar](https://scholar.google.com/scholar?q=Proceedings+of+the+2nd+International+Conference+on+Development+and+Learning+2002)
59. [[3rd International Workshop on Epigenetic Robotics - Modeling Cognitive Development in Robotic Systems|EpiRob 2004]] · EpiRob (ed.) · 2004 · Workshop, University of Genova · [Scholar](https://scholar.google.com/scholar?q=3rd+International+Workshop+on+Epigenetic+Robotics+-+Modeling+Cognitive+Development+in+Robotic+Systems)
60. [[Four-month-old infants prefer to listen to motherese|Four-month-old infants prefer to listen]] · Fernald · 1985 · Infant Behavior and Development · [Scholar](https://scholar.google.com/scholar?q=Four-month-old+infants+prefer+to+listen+to+motherese)
61. [[An ontogenetic perspective on scaling sensorimotor intelligence|An ontogenetic perspective on scaling]] · Ferrell & Kemp · 1996 · Embodied Cognition and Action · [Scholar](https://scholar.google.com/scholar?q=An+ontogenetic+perspective+on+scaling+sensorimotor+intelligence)
62. [[Representations|Representations]] · Fodor · 1981 · Harvester Press · [Scholar](https://scholar.google.com/scholar?q=Representations)
63. [[The Modularity of Mind|The Modularity of Mind]] · Fodor · 1983 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=The+Modularity+of+Mind)
64. [[A survey of socially interactive robots|A survey of socially interactive robots]] · Fong et al. · 2003 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=A+survey+of+socially+interactive+robots)
65. [[Neural control of human motor development|Neural control of human motor development]] · Forssberg · 1999 · Current Opinion in Neurobiology · [Scholar](https://scholar.google.com/scholar?q=Neural+control+of+human+motor+development)
66. [[Value-dependent selection in the brain - simulation in a synthetic neural model|Value-dependent selection in the brain]] · Friston et al. · 1994 · Neuroscience · [Scholar](https://scholar.google.com/scholar?q=Value-dependent+selection+in+the+brain+simulation+in+a+synthetic+neural+model)
67. [[Action recognition in the premotor cortex|Action recognition in the premotor cortex]] · Gallese et al. · 1996 · Brain · [Scholar](https://scholar.google.com/scholar?q=Action+recognition+in+the+premotor+cortex)
68. [[Exploratory behavior in the development of perceiving, acting, and the acquiring of knowledge|Exploratory behavior in the development]] · Gibson · 1988 · Annual Review of Psychology · [Scholar](https://scholar.google.com/scholar?q=Exploratory+behavior+in+the+development+of+perceiving+acting+and+the+acquiring+of+knowledge)
69. [[The theory of affordances|The theory of affordances]] · Gibson · 1977 · Perceiving · [Scholar](https://scholar.google.com/scholar?q=The+theory+of+affordances)
70. [[Emergent Forms - Origins and Early Development of Human Action and Perception|Emergent Forms - Origins and]] · Goldfield · 1995 · Oxford University Press · [Scholar](https://scholar.google.com/scholar?q=Emergent+Forms+Origins+and+Early+Development+of+Human+Action+and+Perception)
71. [[Infant bouncing - the assembly and tuning of an action system|Infant bouncing - the assembly]] · Goldfield et al. · 1993 · Child Development · [Scholar](https://scholar.google.com/scholar?q=Infant+bouncing+the+assembly+and+tuning+of+an+action+system)
72. [[Experiential canalization of behavioral development - theory|Experiential canalization of behavioral development]] · Gottlieb · 1991 · Developmental Psychology · [Scholar](https://scholar.google.com/scholar?q=Experiential+canalization+of+behavioral+development+theory)
73. [[The emulation theory of representation - motor control, imagery, and perception|The emulation theory of representation]] · Grush · 2003 · Behavioral and Brain Sciences · [Scholar](https://scholar.google.com/scholar?q=The+emulation+theory+of+representation+motor+control+imagery+and+perception)
74. [[Ontogeny of postural adjustments during sitting in infancy - variation, selection and modulation|Ontogeny of postural adjustments during]] · Hadders-Algra et al. · 1996 · Journal of Physiology · [Scholar](https://scholar.google.com/scholar?q=Ontogeny+of+postural+adjustments+during+sitting+in+infancy+variation+selection+and+modulation)
75. [[Learning to cruise - Bernstein’s theory applied to skill acquisition during infancy|Learning to cruise - Bernstein’s]] · Haehl et al. · 2000 · Human Movement Science · [Scholar](https://scholar.google.com/scholar?q=Learning+to+cruise+Bernstein%E2%80%99s+theory+applied+to+skill+acquisition+during+infancy)
76. [[Synergetics - An Introduction|Synergetics - An Introduction]] · Haken · 1983 · Springer · [Scholar](https://scholar.google.com/scholar?q=Synergetics+An+Introduction)
77. [[Learning How To Mean - Explorations in the Development of Language|Learning How To Mean]] · Halliday · 1975 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Learning+How+To+Mean+Explorations+in+the+Development+of+Language)
78. [[Shaping Embodied Intelligence—The Morphofunctional Machine Perspective|Shaping Embodied Intelligence—The Morphofunctional Machine Perspective]] · Hara et al. · 2003 · Springer · [Scholar](https://scholar.google.com/scholar?q=Shaping+Embodied+Intelligence%E2%80%94The+Morphofunctional+Machine+Perspective)
79. [[Infant cognition|Infant cognition]] · Harris · 1983 · Handbook of Child Psychology · [Scholar](https://scholar.google.com/scholar?q=Infant+cognition)
80. [[Neuromodulation in mammalian nervous systems|Neuromodulation in mammalian nervous systems]] · Hasselmo et al. · 2003 · MIT Handbook of Brain Theory and Neural Networks · [Scholar](https://scholar.google.com/scholar?q=Neuromodulation+in+mammalian+nervous+systems)
81. [[Catching Ourselves in the Act|Catching Ourselves in the Act]] · Hendriks-Jensen · 1996 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Catching+Ourselves+in+the+Act)
82. [[3rd International Conference on Development and Learning|ICDL 2004]] · ICDL (ed.) · 2004 · Conference, Salk Institute, La Jolla (title as printed says Cognitive Development and Learning) · [Scholar](https://scholar.google.com/scholar?q=3rd+International+Conference+on+Development+and+Learning+ICDL+2004)
83. [[Hand, mouth and brain|Hand, mouth and brain]] · Iverson & Thelen · 1999 · Journal of Consciousness Studies · [Scholar](https://scholar.google.com/scholar?q=Hand+mouth+and+brain)
84. [[Developmental Cognitive Neuroscience|Developmental Cognitive Neuroscience]] · Johnson · 1997 · Blackwell · [Scholar](https://scholar.google.com/scholar?q=Developmental+Cognitive+Neuroscience)
85. [[Developmental changes in the susceptibility to long-term potentiation of neurons in rat visual cortex slices|Developmental changes in the susceptibility]] · Kato et al. · 1991 · Developmental Brain Research · [Scholar](https://scholar.google.com/scholar?q=Developmental+changes+in+the+susceptibility+to+long-term+potentiation+of+neurons+in+rat+visual+cortex+slices)
86. [[Constraints on knowledge and cognitive development|Constraints on knowledge and cognitive development]] · Keil · 1981 · Psychological Review · [Scholar](https://scholar.google.com/scholar?q=Constraints+on+knowledge+and+cognitive+development)
87. [[Dynamic Patterns|Dynamic Patterns]] · Kelso · 1995 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Dynamic+Patterns)
88. [[Information and control - a microscopic analysis of perception–action coupling|Information and control - a]] · Kelso & Kay · 1987 · Perspectives on Perception and Action · [Scholar](https://scholar.google.com/scholar?q=Information+and+control+a+microscopic+analysis+of+perception%E2%80%93action+coupling)
89. [[Development of goal-directed reaching in infants - hand trajectory formation and joint force control|Development of goal-directed reaching in]] · Konczak et al. · 1995 · Experimental Brain Research · [Scholar](https://scholar.google.com/scholar?q=Development+of+goal-directed+reaching+in+infants+hand+trajectory+formation+and+joint+force+control)
90. [[Individual differences in spontaneous oral behavior in neonates|Individual differences in spontaneous oral]] · Korner & Kraemer · 1972 · Proceedings of the 3rd Symposium on Oral Sensation and Perception · [Scholar](https://scholar.google.com/scholar?q=Individual+differences+in+spontaneous+oral+behavior+in+neonates)
91. [[Emergence of imitation mediated by objects|Emergence of imitation mediated by objects]] · Kozima et al. · 2002 · Proceedings of the 2nd International Workshop on Epigenetic Robotics · [Scholar](https://scholar.google.com/scholar?q=Emergence+of+imitation+mediated+by+objects)
92. [[A robot that learns to communicate with human caregivers|A robot that learns to]] · Kozima & Yano · 2001 · Proceedings of the 1st International Workshop on Epigenetic Robotics · [Scholar](https://scholar.google.com/scholar?q=A+robot+that+learns+to+communicate+with+human+caregivers)
93. [[Machine psychology - autonomous behavior, perceptual categorization and conditioning in a brain-based device|Machine psychology - autonomous behavior]] · Krichmar & Edelman · 2002 · Cerebral Cortex · [Scholar](https://scholar.google.com/scholar?q=Machine+psychology+autonomous+behavior+perceptual+categorization+and+conditioning+in+a+brain-based+device)
94. [[Language, mind, and brain - experience alters perception|Language, mind, and brain]] · Kuhl · 2000 · M.S. Gazzaniga · [Scholar](https://scholar.google.com/scholar?q=Language+mind+and+brain+experience+alters+perception)
95. [[From visuo-motor self-learning to early imitation—a neural architecture for humanoid learning|From visuo-motor self-learning to early]] · Kuniyoshi et al. · 2003 · International Conference on Robotics and Automation · [Scholar](https://scholar.google.com/scholar?q=From+visuo-motor+self-learning+to+early+imitation%E2%80%94a+neural+architecture+for+humanoid+learning)
96. [[Women, Fire, and Dangerous Things - What Categories Reveal about the Mind|Women, Fire, and Dangerous Things]] · Lakoff · 1987 · University of Chicago Press · [Scholar](https://scholar.google.com/scholar?q=Women+Fire+and+Dangerous+Things+What+Categories+Reveal+about+the+Mind)
97. [[Philosophy in the Flesh - The Embodied Mind and Its Challenge to Western Thought|Philosophy in the Flesh]] · Lakoff & Johnson · 1999 · Basic Books · [Scholar](https://scholar.google.com/scholar?q=Philosophy+in+the+Flesh+The+Embodied+Mind+and+Its+Challenge+to+Western+Thought)
98. [[An autonomous agent navigating with a polarized light compass|An autonomous agent navigating with]] · Lambrinos et al. · 1997 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=An+autonomous+agent+navigating+with+a+polarized+light+compass)
99. [[Social situatedness of natural and artificial intelligence - Vygotsky and beyond|Social situatedness of natural and]] · Lindblom & Ziemke · 2003 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Social+situatedness+of+natural+and+artificial+intelligence+Vygotsky+and+beyond)
100. [[Autopoiesis - a review and a reappraisal|Autopoiesis - a review and]] · Luisi · 2003 · Naturwissenschaften · [Scholar](https://scholar.google.com/scholar?q=Autopoiesis+a+review+and+a+reappraisal)
101. [[Adaptivity through physical immaturity|Adaptivity through physical immaturity]] · Lungarella & Berthouze · 2002a · Proceedings of the 2nd International Workshop on Epigenetics Robotics · [Scholar](https://scholar.google.com/scholar?q=Adaptivity+through+physical+immaturity)
102. [[Adaptivity via alternate freeing and freezing of degrees of freedom|Adaptivity via alternate freeing and]] · Lungarella & Berthouze · 2002b · Proceedings of the 9th International Conference on Neural Information Processing · [Scholar](https://scholar.google.com/scholar?q=Adaptivity+via+alternate+freeing+and+freezing+of+degrees+of+freedom)
103. [[On the interplay between morphological, neural and environmental dynamics - a robotic case-study|On the interplay between morphological]] · Lungarella & Berthouze · 2002 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=On+the+interplay+between+morphological+neural+and+environmental+dynamics+a+robotic+case-study)
104. [[Learning to bounce - first lessons from a bouncing robot|Learning to bounce - first]] · Lungarella & Berthouze · 2003 · Proceedings of the 2nd International Symposium on Adaptive Motion in Animals and Machines · [Scholar](https://scholar.google.com/scholar?q=Learning+to+bounce+first+lessons+from+a+bouncing+robot)
105. [[Robots as cognitive tools - an information-theoretic analysis of sensory-motor data|Robots as cognitive tools]] · Lungarella & Pfeifer · 2001 · Proceedings of the 2nd IEEE-RAS International Conference on Humanoid Robotics · [Scholar](https://scholar.google.com/scholar?q=Robots+as+cognitive+tools+an+information-theoretic+analysis+of+sensory-motor+data)
106. [[Cellular, synaptic and network effects of neuromodulation|Cellular, synaptic and network effects]] · Marder & Thirumalai · 2002 · Neural Networks · [Scholar](https://scholar.google.com/scholar?q=Cellular+synaptic+and+network+effects+of+neuromodulation)
107. [[Self-taught visually-guided pointing for a humanoid robot|Self-taught visually-guided pointing for a]] · Marjanovic et al. · 1996 · Proceedings of the 4th International Conference on Simulation of Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Self-taught+visually-guided+pointing+for+a+humanoid+robot)
108. [[The Tree of Knowledge - The Biological Roots of Human Understanding|The Tree of Knowledge]] · Maturana & Varela · 1992 · Shambala Publications · [Scholar](https://scholar.google.com/scholar?q=The+Tree+of+Knowledge+The+Biological+Roots+of+Human+Understanding)
109. [[Imitation of facial and manual gestures by human neonates|Imitation of facial and manual]] · Meltzoff & Moore · 1977 · Science · [Scholar](https://scholar.google.com/scholar?q=Imitation+of+facial+and+manual+gestures+by+human+neonates)
110. [[Explaining facial imitation - a theoretical model|Explaining facial imitation - a]] · Meltzoff & Moore · 1997 · Early Development and Parenting · [Scholar](https://scholar.google.com/scholar?q=Explaining+facial+imitation+a+theoretical+model)
111. [[The Imitative Mind - Development, Evolution and Brain Bases|The Imitative Mind - Development]] · Meltzoff & Prinz · 2002 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=The+Imitative+Mind+Development+Evolution+and+Brain+Bases)
112. [[Babybot - A Study Into Sensorimotor Development|Babybot - A Study Into]] · Metta · 2000 · PhD thesis, LIRA-Lab (DIST) · [Scholar](https://scholar.google.com/scholar?q=Babybot+A+Study+Into+Sensorimotor+Development)
113. [[Early integration of vision and manipulation|Early integration of vision and manipulation]] · Metta & Fitzpatrick · 2003 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Early+integration+of+vision+and+manipulation)
114. [[A developmental approach to visually-guided reaching in artificial systems|A developmental approach to visually-guided]] · Metta et al. · 1999 · Neural Networks · [Scholar](https://scholar.google.com/scholar?q=A+developmental+approach+to+visually-guided+reaching+in+artificial+systems)
115. [[Development and robotics|Development and robotics]] · Metta et al. · 2001 · Proceedings of the IEEE-RAS International Conference on Humanoid Robots · [Scholar](https://scholar.google.com/scholar?q=Development+and+robotics)
116. [[Is the cerebellum a smith predictor|Is the cerebellum a smith predictor]] · Miall et al. · 1993 · Journal of Motor Behavior · [Scholar](https://scholar.google.com/scholar?q=Is+the+cerebellum+a+smith+predictor)
117. [[Self-repairing mechanical systems|Self-repairing mechanical systems]] · Murata et al. · 2001 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Self-repairing+mechanical+systems)
118. [[Modular features of motor control and learning|Modular features of motor control]] · Mussa-Ivaldi · 1999 · Current Opinion in Neurobiology · [Scholar](https://scholar.google.com/scholar?q=Modular+features+of+motor+control+and+learning)
119. [[Early Social Cognition (Intellectica)|Early Social Cognition (Intellectica)]] · Nadel · 2003 · in press · [Scholar](https://scholar.google.com/scholar?q=Early+Social+Cognition+%28Intellectica%29)
120. [[Imitation in Infancy|Imitation in Infancy]] · Nadel & Butterworth · 1999 · Cambridge University Press · [Scholar](https://scholar.google.com/scholar?q=Imitation+in+Infancy)
121. [[Developmental learning model for joint attention|Developmental learning model for joint attention]] · Nagai et al. · 2002 · Proceedings of the 15th International Conference on Intelligent Robots and Systems · [Scholar](https://scholar.google.com/scholar?q=Developmental+learning+model+for+joint+attention)
122. [[Development of auditory-evoked reflexes - visuo-acoustic cues integration in a binocular head|Development of auditory-evoked reflexes]] · Natale et al. · 2002 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Development+of+auditory-evoked+reflexes+visuo-acoustic+cues+integration+in+a+binocular+head)
123. [[Unified Theories of Cognition|Unified Theories of Cognition]] · Newell · 1990 · Harvard University Press · [Scholar](https://scholar.google.com/scholar?q=Unified+Theories+of+Cognition)
124. [[Computer science as empirical study - symbols and search|Computer science as empirical study]] · Newell & Simon · 1976 · Communications of the ACM · [Scholar](https://scholar.google.com/scholar?q=Computer+science+as+empirical+study+symbols+and+search)
125. [[Dimensional change in motor learning|Dimensional change in motor learning]] · Newell & Vaillancourt · 2001 · Human Movement Science · [Scholar](https://scholar.google.com/scholar?q=Dimensional+change+in+motor+learning)
126. [[Maturational constraints on language learning|Maturational constraints on language learning]] · Newport · 1990 · Cognitive Science · [Scholar](https://scholar.google.com/scholar?q=Maturational+constraints+on+language+learning)
127. [[Evolutionary Robotics - The Biology, Intelligence, and Technology of Self-Organizing Machines|Evolutionary Robotics - The Biology]] · Nolfi & Floreano · 2000 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Evolutionary+Robotics+The+Biology+Intelligence+and+Technology+of+Self-organizing+Machines)
128. [[Specification of neocortical areas and thalamocortical connections|Specification of neocortical areas and]] · Schlagger & Tuttle · 1994 · Annual Review of Neuroscience · [Scholar](https://scholar.google.com/scholar?q=Specification+of+neocortical+areas+and+thalamocortical+connections)
129. [[Learning visual stabilization reflexes in robots with moving eyes|Learning visual stabilization reflexes in]] · Panerai et al. · 2002 · Neurocomputing · [Scholar](https://scholar.google.com/scholar?q=Learning+visual+stabilization+reflexes+in+robots+with+moving+eyes)
130. [[Infant vocalizations are conditioned both by maternal imitation and motherese speech|Infant vocalizations are conditioned both]] · Pelaez-Nogueras et al. · 1996 · Infant Behavior and Development · [Scholar](https://scholar.google.com/scholar?q=Infant+vocalizations+are+conditioned+both+by+maternal+imitation+and+motherese+speech)
131. [[Catching balls - how to get the hand to the right place at the right time|Catching balls - how to]] · Peper et al. · 1994 · Journal of Experimental Psychology · [Scholar](https://scholar.google.com/scholar?q=Catching+balls+how+to+get+the+hand+to+the+right+place+at+the+right+time)
132. [[Building ‘‘fungus eaters’’ - design principles for autonomous agents|Building ‘‘fungus eaters’’ - design]] · Pfeifer · 1996 · Proceedings of the 4th International Conference on Simulation of Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Building+%E2%80%98%E2%80%98fungus+eaters%E2%80%99%E2%80%99+design+principles+for+autonomous+agents)
133. [[On the role of morphology and materials in adaptive behavior|On the role of morphology]] · Pfeifer · 2000 · Proceedings of the 6th International Conference on Simulation of Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=On+the+role+of+morphology+and+materials+in+adaptive+behavior)
134. [[Robots as cognitive tools|Robots as cognitive tools]] · Pfeifer · 2002 · International Journal of Cognition and Technology · [Scholar](https://scholar.google.com/scholar?q=Robots+as+cognitive+tools)
135. [[Proceedings of the 2nd International Workshop on Emergence and Development of Embodied Cognition|EDEC 2001 proceedings]] · Pfeifer & Lungarella (eds) · 2001 · Workshop, Beijing · [Scholar](https://scholar.google.com/scholar?q=Proceedings+of+the+2nd+International+Workshop+on+Emergence+and+Development+of+Embodied+Cognition)
136. [[From perception to action - The right direction|From perception to action]] · Pfeifer & Scheier · 1994 · From Perception to Action · [Scholar](https://scholar.google.com/scholar?q=From+perception+to+action+The+right+direction)
137. [[Sensory-motor coordination - the metaphor and beyond|Sensory-motor coordination - the metaphor]] · Pfeifer & Scheier · 1997 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Sensory-motor+coordination+the+metaphor+and+beyond)
138. [[Understanding Intelligence|Understanding Intelligence]] · Pfeifer & Scheier · 1999 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Understanding+Intelligence)
139. [[The Origins of Intelligence|The Origins of Intelligence]] · Piaget · 1953 · Routledge · [Scholar](https://scholar.google.com/scholar?q=The+Origins+of+Intelligence)
140. [[Is a quantitative approach useful in the comparison of spontaneous movements in fullterm and preterm infants|Is a quantitative approach useful]] · Piek · 2001 · Human Movement Science · [Scholar](https://scholar.google.com/scholar?q=Is+a+quantitative+approach+useful+in+the+comparison+of+spontaneous+movements+in+fullterm+and+preterm+infants)
141. [[The role of variability in early development|The role of variability in]] · Piek · 2002 · Infant Behavior and Development · [Scholar](https://scholar.google.com/scholar?q=The+role+of+variability+in+early+development)
142. [[Developmental profiles of spontaneous movements in infants|Developmental profiles of spontaneous movements]] · Piek & Carman · 1994 · Early Human Development · [Scholar](https://scholar.google.com/scholar?q=Developmental+profiles+of+spontaneous+movements+in+infants)
143. [[The importance of fetal movements|Fetal movements]] · Prechtl · 1997 · Neurophysiology and Neuropsychology of Motor Development, Mac Keith Press · [Scholar](https://scholar.google.com/scholar?q=The+importance+of+fetal+movements)
144. [[Proceedings of the 3rd International Workshop on Epigenetic Robotics - Modeling Cognitive Development in Robotic Systems|EpiRob 2003 proceedings]] · Prince et al. (eds) · 2003 · Lund University Cognitive Studies vol. 101 · [Scholar](https://scholar.google.com/scholar?q=Proceedings+of+the+3rd+International+Workshop+on+Epigenetic+Robotics+-+Modeling+Cognitive+Development+in+Robotic+Systems)
145. [[Special issue on epigenetic robotics|Special issue on epigenetic robotics]] · Prince & Demiris · 2003 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Special+issue+on+epigenetic+robotics)
146. [[Proceedings of the 2nd International Workshop on Epigenetic Robotics - Modeling Cognitive Development in Robotic Systems|EpiRob 2002 proceedings]] · Prince et al. (eds) · 2002 · Lund University Cognitive Studies vol. 94 · [Scholar](https://scholar.google.com/scholar?q=Proceedings+of+the+2nd+International+Workshop+on+Epigenetic+Robotics+-+Modeling+Cognitive+Development+in+Robotic+Systems)
147. [[Neural Activity and the Growth of the Brain|Neural Activity and the Growth]] · Purves · 1994 · Cambridge University Press · [Scholar](https://scholar.google.com/scholar?q=Neural+Activity+and+the+Growth+of+the+Brain)
148. [[Computation and Cognition - Toward a Foundation for Cognitive Science|Computation and Cognition - Toward]] · Pylyshyn · 1984 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Computation+and+Cognition+Toward+a+Foundation+for+Cognitive+Science)
149. [[Synthetic neural modeling - the ‘Darwin’ series of recognition automata|Synthetic neural modeling - the]] · Reeke et al. · 1990 · Proceedings IEEE · [Scholar](https://scholar.google.com/scholar?q=Synthetic+neural+modeling+the+%E2%80%98Darwin%E2%80%99+series+of+recognition+automata)
150. [[Visual factors in hitting and catching|Visual factors in hitting and catching]] · Regan · 1997 · Journal of Sports Sciences · [Scholar](https://scholar.google.com/scholar?q=Visual+factors+in+hitting+and+catching)
151. [[Language within our grasp|Language within our grasp]] · Rizzolatti & Arbib · 1998 · Trends in Neurosciences · [Scholar](https://scholar.google.com/scholar?q=Language+within+our+grasp)
152. [[Object manipulation and exploration in 2- to 5-month-old infants|Object manipulation and exploration in]] · Rochat · 1989 · Developmental Psychology · [Scholar](https://scholar.google.com/scholar?q=Object+manipulation+and+exploration+in+2-+to+5-month-old+infants)
153. [[Self-perception and action in infancy|Self-perception and action in infancy]] · Rochat · 1998 · Experimental Brain Research · [Scholar](https://scholar.google.com/scholar?q=Self-perception+and+action+in+infancy)
154. [[Perceived self in infancy|Perceived self in infancy]] · Rochat & Striano · 2000 · Infant Behavior and Development · [Scholar](https://scholar.google.com/scholar?q=Perceived+self+in+infancy)
155. [[Life Itself - A Comprehensive Inquiry into the Nature, Origin, and Fabrication of Life|Life Itself - A Comprehensive]] · Rosen · 1991 · Columbia University Press · [Scholar](https://scholar.google.com/scholar?q=Life+Itself+A+Comprehensive+Inquiry+into+the+Nature+Origin+and+Fabrication+of+Life)
156. [[Special issue on self-reconfigurable robots|Special issue on self-reconfigurable robots]] · Rus & Chirikjian · 2001 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Special+issue+on+self-reconfigurable+robots)
157. [[Scaling up sensorimeter systems - constraints from human infancy|Scaling up sensorimeter systems]] · Rutkowska · 1994 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Scaling+up+sensorimeter+systems+constraints+from+human+infancy)
158. [[Can development be designed|Can development be designed]] · Rutkowska · 1995 · What we may learn from the Cog project. In Advances in Artificial Life · [Scholar](https://scholar.google.com/scholar?q=Can+development+be+designed)
159. [[Artificial systems and neuroscience|Artificial systems and neuroscience]] · Sandini · 1997 · M. Srinivasan · [Scholar](https://scholar.google.com/scholar?q=Artificial+systems+and+neuroscience)
160. [[Human sensori-motor development and artificial systems|Human sensori-motor development and artificial systems]] · Sandini et al. · 1997 · Proceedings of the International Symposium on Artificial Intelligence · [Scholar](https://scholar.google.com/scholar?q=Human+sensori-motor+development+and+artificial+systems)
161. [[Building behaviors developmentally - a new formalism|Building behaviors developmentally - a]] · Scassellati · 1998 · Proceedings of the 1998 AAAI Spring Symposium on Integrating Robotics Research · [Scholar](https://scholar.google.com/scholar?q=Building+behaviors+developmentally+a+new+formalism)
162. [[Foundations for a Theory of Mind for a Humanoid Robot|Foundations for a Theory of]] · Scassellati · 2001 · PhD thesis, MIT Department of Electrical Engineering and Computer Science · [Scholar](https://scholar.google.com/scholar?q=Foundations+for+a+Theory+of+Mind+for+a+Humanoid+Robot)
163. [[Is imitation learning the route to humanoid robots|Is imitation learning the route]] · Schaal · 1999 · Trends in Cognitive Science · [Scholar](https://scholar.google.com/scholar?q=Is+imitation+learning+the+route+to+humanoid+robots)
164. [[Categorization in a real-world agent using haptic exploration and active perception|Categorization in a real-world agent]] · Scheier & Lambrinos · 1996 · Proceedings of the 4th International Conference on Simulation of Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Categorization+in+a+real-world+agent+using+haptic+exploration+and+active+perception)
165. [[Mass, center of mass, and moment of inertia estimates for infant limb segments|Mass, center of mass, and]] · Schneider & Zernicke · 1992 · Journal of Biomechanics · [Scholar](https://scholar.google.com/scholar?q=Mass+center+of+mass+and+moment+of+inertia+estimates+for+infant+limb+segments)
166. [[Understanding movement control in infants through the analysis of limb intersegmental dynamics|Understanding movement control in infants]] · Schneider et al. · 1990 · Journal of Motor Behavior · [Scholar](https://scholar.google.com/scholar?q=Understanding+movement+control+in+infants+through+the+analysis+of+limb+intersegmental+dynamics)
167. [[Predictive reward signal of dopamine neurons|Predictive reward signal of dopamine neurons]] · Schultz · 1998 · Journal of Neurophysiology · [Scholar](https://scholar.google.com/scholar?q=Predictive+reward+signal+of+dopamine+neurons)
168. [[Biologically inspired robotics|Biologically inspired robotics]] · Sharkey · 2003 · MIT Handbook of Brain Theory and Neural Networks · [Scholar](https://scholar.google.com/scholar?q=Biologically+inspired+robotics)
169. [[Visual sensory and perceptual abilities of the newborn - beyond the blooming, buzzing confusion|Visual sensory and perceptual abilities]] · Slater & Johnson · 1997 · The Development of Sensory · [Scholar](https://scholar.google.com/scholar?q=Visual+sensory+and+perceptual+abilities+of+the+newborn+beyond+the+blooming+buzzing+confusion)
170. [[Exploratory behavior in blind infants - how to improve touch|Exploratory behavior in blind infants]] · Smitsman & Schellingerhout · 2000 · Infant Behavior and Development · [Scholar](https://scholar.google.com/scholar?q=Exploratory+behavior+in+blind+infants+how+to+improve+touch)
171. [[Core knowledge|Core knowledge]] · Spelke · 2000 · American Psychologist · [Scholar](https://scholar.google.com/scholar?q=Core+knowledge)
172. [[A multiscale state analysis of adult motor learning|A multiscale state analysis of]] · Spencer & Thelen · 1999 · Experimental Brain Research · [Scholar](https://scholar.google.com/scholar?q=A+multiscale+state+analysis+of+adult+motor+learning)
173. [[Embodied cognition|Embodied cognition]] · Sporns · 2003 · MIT Handbook of Brain Theory and Neural Networks · [Scholar](https://scholar.google.com/scholar?q=Embodied+cognition)
174. [[Developing neuro-robotic models|Developing neuro-robotic models]] · Sporns · 2004 · Constructing Cognition · [Scholar](https://scholar.google.com/scholar?q=Developing+neuro-robotic+models)
175. [[Neuromodulation and plasticity in an autonomous robot|Neuromodulation and plasticity in an]] · Sporns & Alexander · 2002 · Neural Networks · [Scholar](https://scholar.google.com/scholar?q=Neuromodulation+and+plasticity+in+an+autonomous+robot)
176. [[Plasticity in value systems and its role in adaptive behavior|Plasticity in value systems and]] · Sporns et al. · 2000 · Adaptive Behavior · [Scholar](https://scholar.google.com/scholar?q=Plasticity+in+value+systems+and+its+role+in+adaptive+behavior)
177. [[Solving Bernstein’s problem - a proposal for the development of coordinated movement by selection|Solving Bernstein’s problem - a]] · Sporns & Edelman · 1993 · Child Development · [Scholar](https://scholar.google.com/scholar?q=Solving+Bernstein%E2%80%99s+problem+a+proposal+for+the+development+of+coordinated+movement+by+selection)
178. [[The artificial life roots of artificial intelligence|The artificial life roots of]] · Steels · 1994 · Artificial Life · [Scholar](https://scholar.google.com/scholar?q=The+artificial+life+roots+of+artificial+intelligence)
179. Steels · 2003 · Personal communication (no paper to link)
180. [[Neural plasticity and cognitive development|Neural plasticity and cognitive development]] · Stiles · 2000 · Developmental Neuropsychology · [Scholar](https://scholar.google.com/scholar?q=Neural+plasticity+and+cognitive+development)
181. [[Robot fostering techniques for sensory-motor development of humanoid robots|Robot fostering techniques for sensory-motor]] · Stoica · 2001 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Robot+fostering+techniques+for+sensory-motor+development+of+humanoid+robots)
182. [[Seeing, Reaching, Touching - The Relations between Vision and Touch in Infancy|Seeing, Reaching, Touching - The]] · Streri · 1993 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Seeing+Reaching+Touching+The+Relations+between+Vision+and+Touch+in+Infancy)
183. [[Cross-modal recognition of shapes from hand to eyes in newborns|Cross-modal recognition of shapes from]] · Streri & Gentaz · 2003 · Somatosensory and Motor Research · [Scholar](https://scholar.google.com/scholar?q=Cross-modal+recognition+of+shapes+from+hand+to+eyes+in+newborns)
184. [[Self-organized control of bipedal locomotion by neural oscillators in unpredictable environments|Self-organized control of bipedal locomotion]] · Taga · 1991 · Biological Cybernetics · [Scholar](https://scholar.google.com/scholar?q=Self-organized+control+of+bipedal+locomotion+by+neural+oscillators+in+unpredictable+environments)
185. [[A model of the neuro-musculo-skeletal system for human locomotion|A model of the neuro-musculo-skeletal]] · Taga · 1995 · Biological Cybernetics · [Scholar](https://scholar.google.com/scholar?q=A+model+of+the+neuro-musculo-skeletal+system+for+human+locomotion)
186. [[Analysis of general movements of infants towards understanding of developmental principle for motor control|Analysis of general movements of]] · Taga et al. · 1999 · Proceedings of the 1999 IEEE International Conference on Systems · [Scholar](https://scholar.google.com/scholar?q=Analysis+of+general+movements+of+infants+towards+understanding+of+developmental+principle+for+motor+control)
187. [[Dimensionality reduction through sensorimotor coordination|Dimensionality reduction through sensorimotor coordination]] · Te Boekhorst et al. · 2003 · Proceedings of the Joint International Conference ICANN/ICONIP · [Scholar](https://scholar.google.com/scholar?q=Dimensionality+reduction+through+sensorimotor+coordination)
188. [[Bio-inspired computing tissues - towards machines that evolve, grow, and learn|Bio-inspired computing tissues - towards]] · Teuscher et al. · 2003 · Biosystems · [Scholar](https://scholar.google.com/scholar?q=Bio-inspired+computing+tissues+towards+machines+that+evolve+grow+and+learn)
189. [[Kicking, rocking and waving - contextual analysis of rhythmical stereotypes in normal human infants|Kicking, rocking and waving]] · Thelen · 1981 · Animal Behaviour · [Scholar](https://scholar.google.com/scholar?q=Kicking+rocking+and+waving+contextual+analysis+of+rhythmical+stereotypes+in+normal+human+infants)
190. [[Time-scale dynamics and the development of an embodied cognition|Time-scale dynamics and the development]] · Thelen · 1995 · Mind as Motion · [Scholar](https://scholar.google.com/scholar?q=Time-scale+dynamics+and+the+development+of+an+embodied+cognition)
191. [[The organization of spontaneous leg movements in newborn infants|The organization of spontaneous leg]] · Thelen & Fischer · 1983 · Journal of Motor Behavior · [Scholar](https://scholar.google.com/scholar?q=The+organization+of+spontaneous+leg+movements+in+newborn+infants)
192. [[A Dynamic Systems Approach to the Development of Cognition and Action|A Dynamic Systems Approach to]] · Thelen & Smith · 1994 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=A+Dynamic+Systems+Approach+to+the+Development+of+Cognition+and+Action)
193. [[Man, Robot, and Society|Man, Robot, and Society]] · Toda · 1982 · Nijhoff · [Scholar](https://scholar.google.com/scholar?q=Man+Robot+and+Society)
194. [[Limitation on input as a basis for neural organization and perceptual development - a preliminary theoretical statement|Limitation on input as a]] · Turkewitz & Kenny · 1982 · Developmental Psychology · [Scholar](https://scholar.google.com/scholar?q=Limitation+on+input+as+a+basis+for+neural+organization+and+perceptual+development+a+preliminary+theoretical+statement)
195. [[Hebb and homeostasis in neural plasticity|Hebb and homeostasis in neural plasticity]] · Turrigiano & Nelson · 2000 · Current Opinion in Neurobiology · [Scholar](https://scholar.google.com/scholar?q=Hebb+and+homeostasis+in+neural+plasticity)
196. [[The Embodied Mind|The Embodied Mind]] · Varela et al. · 1991 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=The+Embodied+Mind)
197. [[Behavior-based early language development on a humanoid robot|Behavior-based early language development on]] · Varshavskaya · 2002 · Proceedings of the 2nd International Conference on Epigenetics Robotics · [Scholar](https://scholar.google.com/scholar?q=Behavior-based+early+language+development+on+a+humanoid+robot)
198. [[Free(z)ing degrees of freedom in skill acquisition|Free(z)ing degrees of freedom in]] · Vereijken et al. · 1992 · Journal of Motor Behavoir · [Scholar](https://scholar.google.com/scholar?q=Free%28z%29ing+degrees+of+freedom+in+skill+acquisition)
199. [[Prospective control - a basic aspect of action development|Prospective control - a basic]] · Von Hofsten · 1993 · Human Development · [Scholar](https://scholar.google.com/scholar?q=Prospective+control+a+basic+aspect+of+action+development)
200. [[Predictive action in infancy - head tracking and reaching for moving objects|Predictive action in infancy]] · Von Hofsten et al. · 1998 · Cognition · [Scholar](https://scholar.google.com/scholar?q=Predictive+action+in+infancy+head+tracking+and+reaching+for+moving+objects)
201. [[Thought and Language|Thought and Language]] · Vygotsky · 1962 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Thought+and+Language)
202. [[Can robots make good models of biological behaviour|Can robots make good models]] · Webb · 2001 · Behavioral and Brain Sciences · [Scholar](https://scholar.google.com/scholar?q=Can+robots+make+good+models+of+biological+behaviour)
203. [[NSF-DARPA Workshop on Development and Learning|NSF-DARPA WDL]] · Weng (ed.) · 2001 · Workshop, Cambridge MA · [Scholar](https://scholar.google.com/scholar?q=NSF+DARPA+Workshop+on+Development+and+Learning)
204. [[Developmental humanoids - humanoids that develop skills automatically|Developmental humanoids - humanoids that]] · Weng et al. · 2000 · Proceedings of the 1st IEEE-RAS Conference on Humanoid Robots · [Scholar](https://scholar.google.com/scholar?q=Developmental+humanoids+humanoids+that+develop+skills+automatically)
205. [[Autonomous mental development by robots and animals|Autonomous mental development by robots]] · Weng et al. · 2001 · Science · [Scholar](https://scholar.google.com/scholar?q=Autonomous+mental+development+by+robots+and+animals)
206. [[Constructivist Neural Network Models of Cognitive Development|Constructivist Neural Network Models of]] · Westermann · 2000 · PhD thesis, Division of Informatics, University of Edinburgh · [Scholar](https://scholar.google.com/scholar?q=Constructivist+Neural+Network+Models+of+Cognitive+Development)
207. [[Proceedings of the 1st International Workshop on Developmental Embodied Cognition|DECO 2001 proceedings]] · Westermann et al. · 2001 · Workshop, Edinburgh · [Scholar](https://scholar.google.com/scholar?q=Proceedings+of+the+1st+International+Workshop+on+Developmental+Embodied+Cognition)
208. [[Primate culture and social learning|Primate culture and social learning]] · Whiten · 2000 · Cognitive Science · [Scholar](https://scholar.google.com/scholar?q=Primate+culture+and+social+learning)
209. [[Neural control of rhythmic arm movements|Neural control of rhythmic arm movements]] · Williamson · 1998 · Neural Networks · [Scholar](https://scholar.google.com/scholar?q=Neural+control+of+rhythmic+arm+movements)
210. [[Perspectives and problems in motor learning|Perspectives and problems in motor learning]] · Wolpert et al. · 2001 · Trends in Cognitive Sciences · [Scholar](https://scholar.google.com/scholar?q=Perspectives+and+problems+in+motor+learning)
211. [[A unifying computational framework for motor control and social interaction|A unifying computational framework for]] · Wolpert et al. · 2003 · Philosophical Transactions of the Royal Society London B · [Scholar](https://scholar.google.com/scholar?q=A+unifying+computational+framework+for+motor+control+and+social+interaction)
212. [[The role of tutoring in problem solving|The role of tutoring in]] · Wood et al. · 1976 · Journal of Child Psychology and Psychiatry · [Scholar](https://scholar.google.com/scholar?q=The+role+of+tutoring+in+problem+solving)
213. [[A constructive model of mother-infant interaction towards infant’s vowel acquisition|A constructive model of mother-infant]] · Yoshikawa et al. · 2004 · Connection Science · [Scholar](https://scholar.google.com/scholar?q=A+constructive+model+of+mother-infant+interaction+towards+infant%E2%80%99s+vowel+acquisition)
214. [[Biomechanics and developmental neuromotor control|Biomechanics and developmental neuromotor control]] · Zernicke & Schneider · 1993 · Child Development · [Scholar](https://scholar.google.com/scholar?q=Biomechanics+and+developmental+neuromotor+control)
215. [[On the role of robot simulations in embodied cognitive science|On the role of robot]] · Ziemke · 2003 · Journal of Artificial Intelligence and the Simulation of Behavior · [Scholar](https://scholar.google.com/scholar?q=On+the+role+of+robot+simulations+in+embodied+cognitive+science)
216. [[Introduction - Why ‘epigenetic robotics’|Introduction - Why ‘epigenetic robotics’]] · Zlatev & Balkenius · 2001 · Proceedings of the 1st International Workshop on Epigenetic Robotics · [Scholar](https://scholar.google.com/scholar?q=Introduction+Why+%E2%80%98epigenetic+robotics%E2%80%99)

## See also

- [[Poppy - Open Source 3D Printed Robot for Experiments in Developmental Robotics|Poppy]] · open humanoid built for developmental robotics with morphology as variable
- [[Evolutionary robotics as a modelling tool in evolutionary biology|ER in evolutionary biology]] · the evolutionary time scale counterpart of this synthetic methodology
