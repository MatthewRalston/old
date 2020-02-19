---
layout: page
title: What am I reading?
toc: true
subtitle: What's on the menu today
favicon: /assets/img/favicon.ico
hero_image: /assets/img/microbiology-163470__480.jpg
---

## Linear Algebra

8th Ed. - Steven J. Leon


I've had a tempestuous relationship with this book. The class I struggled the most with in graduate school was a combined L.A.-DiffEq course with a focus on students with usable knowledge of ODE systems design. I found the material very stimulating mathematics but ultimately, the shapes of those systems memorized by students completing all of that homework evaded me. My time was split between 4 courses and sliding my foot in the then closed door of the Papoutsakis laboratory: they had no room for a unproven bioinformatician. Most of the Chemical Engineers who would have enjoyed having a dedicated Matlab coder weren't used to collaborating with software developers. 

Since linear algebra is often a prerequisite to solving systems of ODEs, I found the treatment of matrix algebra a little challenging spatially. Eventually, after putting the book down and returning to it later, the concepts of basis, orthogonality and familiarity with examples in the book let me eventually enjoy the course and its profoundly different approach to algebra in the general sense. 

Ever since I struggled that semester with DiffEq, I enjoyed very much the concepts of linear algebra and I continue to look forward to finding applications where a SVD or other known decompositions and their applications are immediately obvious to me when dealing with data of a tabular nature.


## Statistics

12th Ed. McClave and Sincich

This is just an awesome overview of basic statistics and intermediate theory applied to basic statistical concepts. I'd be happier if the book had a better treatment of alternative probabilistic brands of statistical inference, but as a summary text of a frequentist style, it is a solid underclassmen oriented-text on the basics. I'm in my second read through of the two distribution types and the classical linear modeling acoutrements. Applying this book in practice may be easier than I thought with the presence of datasets that I can use to model expression or other relationships.


## Understanding Bioinformatiacs

Zvelebil and Baum

This is also a behemoth of a text and I'm struggling to maintain my productivity regarding my improvements in coding capabilities, my blog's content, and the growing book debt to become a fully fledged bioinformatician. I've found the content enjoyable if some of the verbosity could be turned down a little. The amazing attention to the number of available databases is an excellent resource had I the time to organize the contents in detailed study. Typically, I peruse Z&B superficially to get the gist of the application space and theoretical progress and then I look into another more detailed algorithmic text for information about formulation or problem solving techniques. I'm still not at the level where I can implement an alignment or assembly algorithm on my own. I look forward to those applications and focus my efforts on continued study.

## Biological Modeling and Simulation

Schwartz 

The algorithmic text of choice at the moment is a rather condensed overview of the main strategies employed by classical bioinformatics algorithms. It informs the reader just enough about the techniques for some naive implementations. But leaves to the imagination how difficult the implementation and the cleaning of the code must be to get acceptance or even academic interest in citation or use as training material.

After looking at the oversimplification of algorithmic approaches used by this text, I sometimes feel overwhelmed by the amount of work needed for a solid implementation of a bioinformatic algorithm.

## Biological Sequence Analysis

R. Durbin and Sean Eddy

This book has came to me a little bit more personally than the other textbooks to encourage my continued study of the methods and the determination required to become more than a superficial script kiddie. When I joined the Papoutsakis laboratory, one of my first deliverables that helped me stay in their laboratory were the results of stochastic context-free grammars that were being used to model internal RNA structure landscapes and ultimately sequence similarity. While palindromes may be simpler to model than RNA secondary structure algorithmically, mathematically the modeling is only slightly more complex. RNA secondary structure was facing fierce competition between Vienna and the American groups at Janelia farms, who had already become famous for their successful algorithm HMMer.

Without understanding or even witnessing the conflicts take place through blogs and reddit posts and on Twitter... I imagined the worthwhile competition was just taking place in the journals. Eventually when I joined the Computer Assisted Drug Design group, I had become interested more than superficially in nucleic acid sequence similarity having found someone who showed me an application of the thermo that I had such a superficial grasp over. It is clear that thermodynamics is a challenging subject and a depressing one at that. But I had struggled with the fear of failure to find an area where I could commit for years. There are so many nuanced theoretical problems and applied areas to focus efforts, I hadn't found the right fit until the same work I was doing in the beginning of graduate school, looking for RNA secondary structure became useful if it could be automated in its production.

Eventually, the CADD group left me some books and I took them gratefully. One of them was Biological Sequence Analysis and I was already familiar with PSPM/PSSM motif matching with the meme suite. My failure to use HMMer in the nucleic acid space was due to a lack of understanding of the 2-bit system that makes nucleic acids difficult to model. Eventually, I found the time to look into this book specifically at basic Markov models and work backwards towards Bayesian treatments of alignment probability scores to show myself without the frequentist blind pulled over my eyes what probabilistic formulations can do when the applications are simple enough.
