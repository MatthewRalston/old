---
title: Differentiation in Computational Research Environments
subtitle: My first post on Medium about Bioinformatics
tags: meta prose education beginner
toc: true
---

## Introduction

Big data is a meme, but there is a dichotomy worth discussing in the emerging interdisciplinary fields involving computer science. There is a choice that hybrid scientist-quants make during their training: data science vs data engineering. In this article, I'll describe some differences in skillsets between those two specializations, some reasons why choosing one over the other might better suit your team style, and in what way your choice will make you reliant on, or independent from, others regarding the development of sophisticated models, reports, and applications. 

Before I begin drawing imaginary boundaries on what data scientists or engineers can and cannot do, I'd like to remind you that you have your own unique journey and role in your current and future team. You're reading this article because you'd like to hear what my journey has been like. I'm another millenial navigating a complex interdisciplinary field coming from someplace I am strong (biology) towards somewhere I am not as strong at (maths and computer science). There are no boundaries, real or otherwise; any attempt I make at describing hypothetical roles or strength/weaknesses of the various categories is my attempt to organize differences in expectations and roles.



## Interdisciplinary science is a balance of scientific expertise, math, and computer skills

Bioinformaticians come in all different types: some remain active at the bench and continue to do wet-laborary experiments. Some are desk jockeys and theoreticians that crunch numbers and derive metrics or interpret datasets to make the next advances in their specialized fields. Finally, there are infrastructure builders that create interesting and advanced architectures that enable others.

But like scientists, in a general sense, bioinformaticians have to choose how much of each skill to master and bring to the table. For example, some bioinformaticians puruse knowledge of statistics and mathematics typically because math is some peoples' best language for learning and/or effecting their environment. Others may avoid the side of bioinformatics that stems from mathematics and computer science in favor of learning just enough math to make sense of the alignment problem, which has enough applications in theoretical biology to make an entire career. To some degree, alignment is one mathematical basis (no pun) for evolutionary theory to grow.

Another factor during the differentiation process of bioinformaticians is the level of programming or systems administration skills to learn and the wilingness to apply them. It's been a long time since I have had a software bug that has totally incapacitated my computer, perhaps 6 months or so? The larger challenge for most of us is ranking or choosing software packages to use and justify the usage of to our colleagues.


<blockquote>My first frustration in experiencing my own indecisions was understanding to what extent I want to use computers as a tool of quantitative strength, flexibility and work-life balance, or as the fundamental center of my scientific career. I don't like sitting in front of a computer all day. But I do enjoy the feeling that multi-tasking on the server gives me, when I know my simulations and theoretical pursuits are progressing even if I'm writing or doing something in the laboratory.</blockquote>



<blockquote>I would argue that heterogeneity is greater among students of multiple disciplines. The degree of publicly available information in software development is very large, while the fraction of material that is actual computer science wisdom is very low. Currently, it doesn't feel like 4 years is enough time to be introduced to the available subfields, have enough classes to focus on understanding all the strong theories that may play a role, and learn enough applied areas through journal articles and laboratory investigation to build a financial foundation strong enough to go back through the cycle to either generalize or specialize. </blockquote>

The other factor driving the growth of interdisciplinary studies is the complexity of systems under study. It might seem that the mark of a true scientist is the generation of multiple feasible hypotheses and answers to them every quarter. That may be true if they are intimately familiar with the subject or their experience with a technique makes them a specialist in some way. Most research is driven by data to the point where hypotheses aren't always made explicit. Data is collected, the system is characterized, and only when something <em>doesn't</em> make sense with the existing model is a hyptothesis formalized. In some ways, we are incredibly reliant on the techniques, technologies, and assumptions made during their creation that gives them power during our daily activities. Moreover, there aren't many young comedians bringing light to those issues in a good way.

<blockquote>My point is that some sciences aren't hypothesis driven any more; they're data driven. The system complexity doesn't always make for easy or obvious cherry-picked hypotheses. Only after significant study of the system's normal function can you begin to anticipate what perturbations might be useful to study its response. And in this case the system is typically thousands to hundreds of thousands of 'genes' if you go by the high-school definition of what a gene is.</blockquote>

<blockquote>So what? So the system complexity of any one gene is typically non-linear, there is no absolute unit of gene expression, the resolution required to focus on intervals of gene expression with linear characteristics is expensive, and genes are typically regulated in clusters. It turns out those things are necessary with multiplexed expression data to look at one simple perturbation experiment.</blockquote>

Data driven science is effective at building consensus, but it is ineffective at reasserting and refining existing theoretical foundations or challenging, deconstructing, and alternating new hypothetical frameworks. This is why interdisciplinary education can be so important, because assumptions from both fields are at play, in the methods that make the border between two disciplines, that may be convenient but not necessarily correct.

### Scientists and Analysts

Bioinformaticians don't hold the monopoly on data analysis. Most scientists can do simple and frequent regression and hypothesis testing in Excel and that makes them a large factor in the stability of the specialty/subfield in your organization, since they can build a complete narrative by checking existing theories in the laboratory, and ensuring that other important differences or trends hold weight through simple graphical applications. Years of advanced statistical training matter much less when you're side by side with wet-lab scientists that are experts in hardware and technological trends in the science, test hypotheses, and develop methods regularly.

The opportunity for bioinformaticians to market their abilties stems from a few key transactions that can be easy for computer scientists to monitor: the choice of software, the available computational performance (that could be optimized), the ease of use for the scientists, the optimization of parameters, and user friendly documentation on the paradigms and competition within the theory that gives rise to each choice made in generated calculations. Perhaps the other more often neglected opportunity is to give scientists positive examples where the calculation was partially decisional or negative examples where the information was misinterpreted.

That said, some bioinformaticians might not even find themselves relating to the next two categories I outline in this article. They might find they relate closely with the scientists and prefer to do light, simple, and elegant modeling tasks with JMP, Excel, or similar. An issue facing those bioinformaticians is competition and job security. Some might read that as opportunity, instead of as warning. It's true that some bioinformaticians might prefer job security over other things, but there is opportunity for researchers who can brave the waters of instability with a dash of **reproducibility**. 

Other issues facing bioinformatic analysts include their reliance on others for access to databases, applications, datasets, and available models. Being reliant on graphical user interfaces means that processing times may be somewhat larger or lower in throughput than is possible with bioinformaticians who prefer to work at the command-line. If you fall in the former category, remember that working closely with scientists is a must-have, reproducibility can be challenging without Excel templates or macros, and you'll likely rely on others for access to heavily 'munged' data being stored in databases.

### Data Scientists

If you fell into the later category, you might find that there is so much insight to gain from existing datasets, and indexed, organized datasets can quickly yield insights, models, and application ideas were there merely the personnel to harness it. The data science specialty is often touted as the gamechanging new specialty, yet it's a rebranding of existing analytical and statistical specialties. 

Data scientists tend to have strong backgounds in mathematics, statistics, and software development to retrieve information, parse it quickly, and develop model parameters. Report generation may be an interesting subfield with the rise of literate programming frameworks like Jupyter and Rmd. 

These skills may contrast with the scientist or analyst in a very strong way, but can be especially strong if web development and form generation is a skill in a data scientist's wheelhouse. Data scientists can process data with extraordinary throughput, but are in some ways more susceptible to becoming part of the machine that builds upon existing models or paradigms without actually challenging them. Since they are removed from the laboratory environment, it means they might not be aware of trends or idiosyncracies that may threaten the longevity of their models. The temptation to appear as a data 'authority figure' may make antagonism between traditional bench scientists, analysts, and this 'new' category of technically inclined computer experts. 

The great power of analytics in this way should mean that data scientists need to feel comfortable with older hardware wherever possible, being candid about needs for collaborations, datasets, or interactions so they might share what they have built. The results oriented nature and speed that they produce necessitates frequent presentation. Fortunately, they have one category of individuals even more removed from the truly integrated science ideal and stuck more into the IT millieu. 

### Data Engineer

The final category is newer in its formalization, and has largely grown from the increased size and complexity of data representations. Data engineers have rich software engineering and computer science abilities that lets them build critical infrastructure for others with a focus on stability, usability, and efficiency. Precomputing, algorithmic efficiency, and automation may be required for applications in their wheelhouse. Enterprise software is often expensive to develop and licenses; data engineers make a team responsive to the team's computational needs in a way that scientists and analysts often cannot. They are invaluable teammates and often provide scripts, REST-APIs, databases, automated systems, web applications, and high-performance computing to the teams that they are directly or indirectly supporting. 

Unfortunately, they closely resemble IT and may face budgetary restrictions that do not mesh with the throughput and results driven design of data engineering teams or teams that integrate products of data engineers. Moreover, salesmanship may be challenging for data engineers who may favor simple and elegant application designs and might dread working with designers. Informal and formal feedback, design, and financial feedback may be important skillsets for data engineers to build credibility inside and outside their teams.


***

As you can see, the choices facing millenials braving these waters is both astonishing and significant. The resulting divide between wet and dry science and even between dry-laboratory roles can be challenging without good communication, leadership, and salesmanship on both sides of the equation. To facilitate this, educational leaders should consider developing problem sets or specializations that reflect which strengths of the student are already formed vs those that are just beginning to grow.

