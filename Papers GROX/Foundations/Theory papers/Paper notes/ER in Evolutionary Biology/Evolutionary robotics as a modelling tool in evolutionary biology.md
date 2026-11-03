---
title: "Evolutionary robotics as a modelling tool in evolutionary biology"
aliases:
  - ER in Evolutionary Biology
  - ER in evolutionary biology
authors:
  - "[[Alan F. T. Winfield]]"
corresponding: "[[Alan F. T. Winfield]]"
institutions:
  - "[[University of the West of England]]"
labs:
  - "[[Bristol Robotics Laboratory]]"
countries:
  - United Kingdom
year: 2024
date: 2024-11-05
venue: Frontiers in Robotics and AI 2024
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.3389/frobt.2024.1278983
pdf: ""
code: "No official repo (position paper, checked 2026-11-02)"
status: to-read
relevance: low
added: 2026-11-02
tags:
  - paper
  - topic/evolutionary-robotics
  - topic/morphology
  - topic/co-design
  - topic/sim-to-real
  - topic/developmental-robotics
  - method/evolutionary-algorithms
---

# ER in Evolutionary Biology

> [!info] Quick view
> **Authors:** [[Alan F. T. Winfield]] (corresponding)
> **From:** [[University of the West of England]] · [[Bristol Robotics Laboratory]]
> **Published:** Frontiers in Robotics and AI 11:1278983 · November 2024
> **Keywords:** evolutionary robotics, evolutionary biology, synthetic method, Webb's model dimensions, artificial selection, evo-devo, reality gap, major transitions
> **Links:** [DOI](https://doi.org/10.3389/frobt.2024.1278983)
> **One line:** position paper asking what kind of model an evolutionary robotics system is, extending Webb's seven model dimensions with eight ER-specific ones and concluding ER models artificial selection, not natural selection

## Summary

Winfield ask a simple question that people in ER rarely ask: when you use evolutionary robotics to say something about biology, what kind of model are you actually building? He starts from Grey Walter's tortoises and the synthetic method, then describes three case studies with Webb's seven dimensions (relevance, level, generality, abstraction, structural accuracy, performance match, medium): Long's Tadro robots evolving tail stiffness, Waibel et al. evolving altruism in ALICE robots, and Clune et al. evolving modular networks. He proposes eight extra dimensions for ER (gene pools, population size, generations, genotype-phenotype mapping, brain/body, fitness, selection strategy, evo vs evo-devo).

The main argument is that ER is robot husbandry: it models artificial selection, with sparse environments, tiny populations, few generations, weak genotype-phenotype mapping and the reality gap. So it can't touch the big questions like the major transitions, but it is still useful for testing specific trait hypotheses. No experiments, it's a conceptual paper. For me it's mostly background, but the evo-devo part and the "ER cannot be scientific instrument and robot designer at the same time" point are worth keeping in mind.

## How it works

- Robots as scientific instruments, from Walter's machina speculatrix to biorobotics
- Webb's 7 dimensions applied to 3 case studies: Long 2006 (body, 3 robots, 10 generations), Waibel 2009 (controller, 1000 individuals, 300 generations), Clune 2013 (ANN, simulation)
- Proposed extension: gene pools, population size, generations, mapping, brain/body, fitness function, selection strategy, evo or evo-devo
- Strengths: modelling when nothing else is possible, multiple species, the richness of real matter, controllable conditions
- Limitations: sparse environment, small evolutionary distance, weak genotype to phenotype mapping, reality gap
- Goes through Maynard Smith and Szathmáry's 8 major transitions and which ones ER might model
- 6 recommendations for roboticists, like working with biologists and being hypothesis-driven

## Results that matter

- No quantitative results, it's a position paper
- Conclusion: ER is a weak model of evolutionary biology because it models artificial selection, not natural selection
- Transitions 1 to 4 are out of scope, sex and multicellularity are maybe reachable, eusociality only partly (Ferrante 2015 task specialization)
- "ER cannot model the evolution of a backbone but only the evolution of a better backbone"

## Interesting stuff

- Robot husbandry is a nice way to frame what morphology optimization actually is
- The extended model description table is a good checklist for reporting any morphology search paper
- Kriegman 2018 cited as showing that development during lifetime can increase evolvability, which connects to morphing during the task
- Single author, very opinionated, and only three case studies so the coverage is narrow

## Useful for me

- **As prior work.** Citation for the reality gap and resource cost of evolving bodies in hardware, and for evo-devo being important in ER ("if it evolves it needs to learn").
- **Apply.** When I report morphology optimization, use the extended dimensions (population, generations, mapping, brain/body, fitness) so the setup is clear.
- **Watch out.** He says don't use the same ER system both to study biology and to design useful robots. My work is design, so I should not claim biological insight from it.
- **Gap.** Body change during lifetime (development) is mentioned only as a future dimension, nobody models it seriously here.

## Key prior work

- [[Can robots make good models of biological behaviour|Webb 2001]] · the seven dimensions framework the paper extends
- [[Evolutionary robotics - model or design|ER model or design]] · Trianni, first critique of ER as a model for biology
- [[Evolutionary Robotics - What, Why, and Where to|ER what why where]] · Doncieux et al., the ER overview
- [[Biomimetic evolutionary analysis - testing the adaptive value of vertebrate tail stiffness in autonomous swimming robots|Tadro]] · evolving a body in real hardware
- [[The evolutionary origins of modularity|Origins of modularity]] · Clune et al., connection cost leads to modular networks
- [[How morphological development can guide evolution|Morphological development]] · Kriegman, development during lifetime helps evolution
- [[Noise and the reality gap - the use of simulation in evolutionary robotics|Noise and the reality gap]] · Jakobi, the classic reality gap paper

## References

1. [[The masterpiece of nature - the evolution and genetics of sexuality|Masterpiece of nature]] · Bell · 1982 · Routledge · [Scholar](https://scholar.google.com/scholar?q=The+masterpiece+of+nature+the+evolution+and+genetics+of+sexuality)
2. [[Toward open-ended evolutionary robotics - evolving elementary robotic units able to self-assemble and self-reproduce|Open-ended ER]] · Bianco & Nolfi · 2004 · Connection Science · [DOI](https://doi.org/10.1080/09540090412331314759)
3. [[Understanding by design - the synthetic approach to intelligence|Understanding by design]] · Bisig et al. · 2008 · Birkhäuser · [Scholar](https://scholar.google.com/scholar?q=Understanding+by+design+the+synthetic+approach+to+intelligence)
4. [[Contingency and determinism in evolution - Replaying life's tape|Replaying life's tape]] · Blount et al. · 2018 · Science · [DOI](https://doi.org/10.1126/science.aam5979)
5. [[Hamilton's rule and the causes of social evolution|Hamilton's rule]] · Bourke · 2014 · Phil. Trans. R. Soc. B · [DOI](https://doi.org/10.1098/rstb.2013.0362)
6. [[Environment-driven open-ended evolution with a population of autonomous robots|Environment-driven evolution]] · Bredeche & Montanier · 2012 · Evolving Physical Systems Workshop · [Scholar](https://scholar.google.com/scholar?q=Environment-driven+open-ended+evolution+with+a+population+of+autonomous+robots)
7. [[Unshackling evolution - evolving soft robots with multiple materials and a powerful generative encoding|Unshackling evolution]] · Cheney et al. · 2013 · GECCO · [DOI](https://doi.org/10.1145/2463372.2463404)
8. [[The evolutionary origins of modularity|Origins of modularity]] · Clune et al. · 2013 · Proc. R. Soc. B · [DOI](https://doi.org/10.1098/rspb.2012.2863)
9. [[Evolutionary developmental soft robotics as a framework to study intelligence and adaptive behavior in animals and plants|Evo-devo soft robotics]] · Corucci et al. · 2017 · Front. Robotics AI · [DOI](https://doi.org/10.3389/frobt.2017.00034)
10. [[Evolving soft locomotion in aquatic and terrestrial environments - effects of material properties and environmental transitions|Soft locomotion land and water]] · Corucci et al. · 2018 · Soft Robotics · [DOI](https://doi.org/10.1089/soro.2017.0055)
11. [[On the origin of species by means of natural selection|Origin of species]] · Darwin · 1859 · John Murray · [Scholar](https://scholar.google.com/scholar?q=On+the+origin+of+species+by+means+of+natural+selection)
12. [[Heritability of morphological and behavioural traits in evolving robots|Heritability in evolving robots]] · De Carlo et al. · 2024 · Evol. Intell. · [DOI](https://doi.org/10.1007/s12065-023-00860-0)
13. [[Evolutionary Robotics - What, Why, and Where to|ER what why where]] · Doncieux et al. · 2015 · Front. Robotics AI · [DOI](https://doi.org/10.3389/frobt.2015.00004)
14. [[Real-World Robot Evolution - Why Would it (not) Work|Real-world robot evolution]] · Eiben · 2021 · Front. Robotics AI · [DOI](https://doi.org/10.3389/frobt.2021.696452)
15. [[If it evolves it needs to learn|If it evolves it needs to learn]] · Eiben & Hart · 2020 · GECCO Companion · [DOI](https://doi.org/10.1145/3377929.3398151)
16. [[Towards autonomous robot evolution|Autonomous robot evolution]] · Eiben et al. · 2021 · Springer · [DOI](https://doi.org/10.1007/978-3-030-66494-7_2)
17. [[From evolutionary computation to the evolution of things|Evolution of things]] · Eiben & Smith · 2015 · Nature · [DOI](https://doi.org/10.1038/nature14544)
18. [[Comparison of selection methods in on-line distributed evolutionary robotics|Selection methods]] · Fernández Pérez et al. · 2014 · ALIFE 14 · [DOI](https://doi.org/10.1162/978-0-262-326216-ch046)
19. [[Evolution of self-organized task specialization in robot swarms|Task specialization in swarms]] · Ferrante et al. · 2015 · PLoS Comput. Biol. · [DOI](https://doi.org/10.1371/journal.pcbi.1004273)
20. [[Adaptive behavior in competing co-evolving species|Predator-prey coevolution]] · Floreano & Nolfi · 1997 · ECAL · [Scholar](https://scholar.google.com/scholar?q=Adaptive+behavior+in+competing+co-evolving+species)
21. [[Exploration and high adventure - the legacy of Grey Walter|Legacy of Grey Walter]] · Holland · 2003 · Phil. Trans. R. Soc. A · [DOI](https://doi.org/10.1098/rsta.2003.1260)
22. [[The first biologically inspired robots|First bio-inspired robots]] · Holland · 2003 · Robotica · [DOI](https://doi.org/10.1017/S0263574703004971)
23. [[Artificial ethology|Artificial ethology]] · Holland & McFarland · 2001 · Oxford University Press · [Scholar](https://scholar.google.com/scholar?q=Artificial+ethology+Holland+McFarland)
24. [[Automated antenna design with evolutionary algorithms|Evolved antenna]] · Hornby et al. · 2006 · Space 2006 · [DOI](https://doi.org/10.2514/6.2006-7242)
25. [[Noise and the reality gap - the use of simulation in evolutionary robotics|Noise and the reality gap]] · Jakobi et al. · 1995 · ECAL · [Scholar](https://scholar.google.com/scholar?q=Noise+and+the+reality+gap+the+use+of+simulation+in+evolutionary+robotics)
26. [[Interactive robots in experimental biology|Robots in experimental biology]] · Krause et al. · 2011 · Trends Ecol. Evol. · [DOI](https://doi.org/10.1016/j.tree.2011.03.015)
27. [[How morphological development can guide evolution|Morphological development]] · Kriegman et al. · 2018 · Scientific Reports · [DOI](https://doi.org/10.1038/s41598-018-31868-7)
28. [[Niche construction, biological evolution, and cultural change|Niche construction]] · Laland et al. · 2000 · Behav. Brain Sci. · [DOI](https://doi.org/10.1017/S0140525X00002417)
29. [[Robotics as a comparative method in ecology and evolutionary biology|Robotics comparative method]] · Lauder · 2022 · Integr. Comp. Biol. · [DOI](https://doi.org/10.1093/icb/icac016)
30. [[The dialectical biologist|Dialectical biologist]] · Levins & Lewontin · 1985 · Harvard University Press · [Scholar](https://scholar.google.com/scholar?q=The+dialectical+biologist)
31. [[Automatic design and manufacture of robotic lifeforms|GOLEM]] · Lipson & Pollack · 2000 · Nature · [DOI](https://doi.org/10.1038/35023115)
32. [[Autonomous Morphogenesis in Self-assembling Robots using IR-Based Sensing and Local Communications|Autonomous morphogenesis]] · Liu & Winfield · 2010 · Swarm Intelligence (ANTS) · [Scholar](https://scholar.google.com/scholar?q=Autonomous+Morphogenesis+in+Self-assembling+Robots+using+IR-Based+Sensing+and+Local+Communications)
33. [[Darwin's devices|Darwin's devices]] · Long · 2012 · Basic Books · [Scholar](https://scholar.google.com/scholar?q=Darwin%27s+devices+Long)
34. [[Biomimetic evolutionary analysis - testing the adaptive value of vertebrate tail stiffness in autonomous swimming robots|Tadro]] · Long et al. · 2006 · J. Exp. Biol. · [DOI](https://doi.org/10.1242/jeb.02559)
35. [[Using individual-based modelling to investigate a pluralistic explanation for the prevalence of sexual reproduction in animal species|Sexual reproduction modelling]] · MacPherson et al. · 2023 · Ecol. Model. · [DOI](https://doi.org/10.1016/j.ecolmodel.2022.110191)
36. [[Byte-sized evolution|Byte-sized evolution]] · Maynard Smith · 1992 · Nature · [DOI](https://doi.org/10.1038/355772a0)
37. [[The major transitions in evolution|Major transitions]] · Maynard Smith & Szathmáry · 1995 · Oxford University Press · [Scholar](https://scholar.google.com/scholar?q=The+major+transitions+in+evolution)
38. [[Collective sorting and segregation in robots with minimal sensing|Collective sorting]] · Melhuish et al. · 1998 · SAB · [Scholar](https://scholar.google.com/scholar?q=Collective+sorting+and+segregation+in+robots+with+minimal+sensing)
39. [[Environmental influences on evolvable robots|Environmental influences]] · Miras et al. · 2020 · PLOS ONE · [DOI](https://doi.org/10.1371/journal.pone.0233848)
40. [[Fitness functions in evolutionary robotics - A survey and analysis|ER fitness functions]] · Nelson et al. · 2009 · Robotics and Autonomous Systems · [DOI](https://doi.org/10.1016/j.robot.2008.09.009)
41. [[A reference guide to evo-devo|Evo-devo guide]] · Nuño de la Rosa & Müller · 2020 · Springer · [DOI](https://doi.org/10.1007/978-3-319-33038-9_194-1)
42. [[The causes of evolvability and their evolution|Causes of evolvability]] · Payne & Wagner · 2019 · Nat. Rev. Genet. · [DOI](https://doi.org/10.1038/s41576-018-0069-z)
43. [[Genotype–phenotype mapping and the end of the 'genes as blueprint' metaphor|Genes as blueprint]] · Pigliucci · 2010 · Phil. Trans. R. Soc. B · [DOI](https://doi.org/10.1098/rstb.2009.0241)
44. [[Evolve-ability of the robot platform in the Symbrion project|Symbrion]] · Schlachter et al. · 2008 · SASO Workshops · [DOI](https://doi.org/10.1109/SASOW.2008.31)
45. [[Open issues in evolutionary robotics|Open issues in ER]] · Silva et al. · 2016 · Evolutionary Computation · [DOI](https://doi.org/10.1162/EVCO_a_00172)
46. [[Evolutionary robotics - model or design|ER model or design]] · Trianni · 2014 · Front. Robotics AI · [DOI](https://doi.org/10.3389/frobt.2014.00013)
47. [[Modularity in animal development and evolution - elements of a conceptual framework for EvoDevo|Modularity in EvoDevo]] · von Dassow & Munro · 1999 · J. Exp. Zool. · [Scholar](https://scholar.google.com/scholar?q=Modularity+in+animal+development+and+evolution+elements+of+a+conceptual+framework+for+EvoDevo)
48. [[A quantitative test of Hamilton's rule for the evolution of altruism|Hamilton's rule test]] · Waibel et al. · 2011 · PLOS Biology · [DOI](https://doi.org/10.1371/journal.pbio.1000615)
49. [[Genetic team composition and level of selection in the evolution of cooperation|Team composition]] · Waibel et al. · 2009 · IEEE TEVC · [DOI](https://doi.org/10.1109/TEVC.2008.2011741)
50. [[Evolutionary branching in a finite population - deterministic branching vs. stochastic branching|Evolutionary branching]] · Wakano & Iwasa · 2012 · Genetics · [DOI](https://doi.org/10.1534/genetics.112.144980)
51. [[The living brain|The living brain]] · Walter · 1953 · W. W. Norton · [Scholar](https://scholar.google.com/scholar?q=The+living+brain+Walter)
52. [[Robots as models of evolving systems|Robots as evolving systems]] · Wang et al. · 2022 · PNAS · [DOI](https://doi.org/10.1073/pnas.2120019119)
53. [[Using robots to model animals - a cricket test|Cricket robot]] · Webb · 1995 · Robotics and Autonomous Systems · [DOI](https://doi.org/10.1016/0921-8890(95)00044-5)
54. [[Can robots make good models of biological behaviour|Webb 2001]] · Webb · 2001 · Behav. Brain Sci. · [DOI](https://doi.org/10.1017/S0140525X01000127)
55. [[Robotics - a very short introduction|Robotics VSI]] · Winfield · 2012 · Oxford University Press · [Scholar](https://scholar.google.com/scholar?q=Robotics+a+very+short+introduction+Winfield)
56. [[Estimating the Energy cost of (artificial) evolution|Energy cost of evolution]] · Winfield · 2014 · ALIFE 14 · [Scholar](https://scholar.google.com/scholar?q=Estimating+the+Energy+cost+of+artificial+evolution)
57. [[Experiments in artificial culture - from noisy imitation to storytelling robots|Artificial culture]] · Winfield & Blackmore · 2022 · Phil. Trans. R. Soc. B · [DOI](https://doi.org/10.1098/rstb.2020.0323)
58. [[Quantifying morphological computation|Morphological computation]] · Zahedi & Ay · 2013 · Entropy · [DOI](https://doi.org/10.3390/e15051887)

## See also

- [[Developmental robotics - a survey|Developmental robotics survey]] · the lifetime development counterpart
- [[Combining Evolutionary and Adaptive Control Strategies for Quadruped Robotic Locomotion|Evolutionary Adaptive CPG]] · evolution with learning in the loop, a small evo-devo flavour experiment
