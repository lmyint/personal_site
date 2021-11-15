---
title: 'The Book of Why'
author: Leslie Myint
date: '2018-07-25'
slug: book-of-why
categories: ["Causal Inference", "Books"]
tags: ["causal diagrams"]
math: true
header:
  caption: ''
  image: ''
---

I just finished reading *The Book of Why* by Judea Pearl and Dana Mackenzie, and I really enjoyed it. I had been wanting to read it for some time now because I know very little about methodology relating to causal diagrams and structure learning. The book provides an overview of the main ideas that formed and historical events that led up to what Pearl calls the "Causal Revolution", a burgeoning of the direct interrogation of causation as opposed to its implicit renouncement in science for a period before then. Much of the causal inference methodology that Pearl discusses in the book is his own, namely that of causal diagrams and *do*-calculus. In light of his own methods, he also discusses techniques that are popular in disciplines such as psychology and economics. He also discusses another major framework for causal inference, the Rubin causal model. In reflecting on the book, I found it useful to organize my thoughts according to major themes I saw in the book.

## Table of Contents

1. [Language and diagrams](#language-diagrams)
2. [History](#history)
3. [Cognition](#cognition)
4. [Teaching](#teaching)
5. [Conclusions](#conclusions)

## Language and diagrams <a id="language-diagrams"></a>

Right from the introduction, Pearl emphasizes the importance of language in science:

> My emphasis on language also comes from a deep conviction that language 
> shapes our thoughts. You cannot answer a question that you cannot ask, 
> and you cannot ask a question that you have no words for. (p. 10)

I love this quote because it echoes how essential careful language is for humanity's progress. We cannot understand an idea without expressing it in some language in our own minds. We cannot transfer this understanding faithfully to others without carefully crafting language. This crafting of language affects how others understand, consume, act upon, and transfer the idea. And the cycle repeats. Essentially, language governs our intellectual progeny, which has profound scientific, moral, and cultural implications. There is even a growing body of scientific evidence regarding specific ways in which language shapes our thoughts. A [TED talk by Lera Boroditsky](https://www.ted.com/talks/lera_boroditsky_how_language_shapes_the_way_we_think) discusses some of these.

Why does Pearl emphasize the importance of language for causal inference? It has to do with precision, and it reminds me of a scene from Lois Lowry's *The Giver*.

> "What is it, Jonas?" his father asked. 
> 
> He made himself say the words, though he felt flushed with 
> embarrassment. He had rehearsed them in his mind all the way 
> home from the Annex. 
> 
> "Do you love me?" 
> 
> There was an awkward silence for a moment. Then Father gave a 
> little chuckle. "Jonas. You, of all people. Precision of
> language, please!"

Earlier in the book it is revealed that the reason for the strict adherence to precision of language in Jonas's community is to avoid unintentional lies that can come about through exaggeration or misinterpretation. We learn quickly in the story that this precision of language creates a dystopia, devoid of true feeling and the emotions that make up a beautiful human life.

A bleak picture in the case of *The Giver*, but the existence of a language that allows for precise expression is indispensable when it comes to science! Consider the following epidemiological investigation: we want to know how the consumption of red meat influences risk for colon cancer. There are two questions that we might think of quickly.

1. Does eating red meat cause an increase in colon cancer risk?
2. How much red meat consumption is needed to increase the risk of colon cancer?

My impression is that the first question is how the majority of the public perceives a causal effect. *Does* exposure cause outcome? Pearl explains that this conventional way of thinking about causal analysis is really not the goal of the field at all:

> Many people still make Niles's mistake of thinking that the goal 
> of causal analysis is to prove that X is a cause of Y or else to 
> find the cause of Y from scratch. That is the problem of causal 
> discovery. (p. 79)

(Niles was a critic of path analysis/causal diagrams.) The goal of causal analysis is to quantitatively estimate causal effects while fully capturing the state of the analyst's current knowledge. The full capturing of current knowledge is achieved by drawing a causal diagram.

The second question, though quantitative, is still imprecise. It gets more at the estimation goal of causal analysis, but its imprecision leads to individual interpretations of the best way to proceed (essentially researcher degrees of freedom). Certainly there will be differences between individuals in terms of what they feel is the current state of knowledge on a subject. That is, experts may disagree on their causal diagrams. This disagreement is ok as long as their working set of assumptions (the causal diagrams) are made explicit. Usually when researchers ask a causal question like number 2, researcher degrees of freedom abound in both the variables considered and the manner in which the variables are handled in the analytic method.

Both of these issues can be avoided by using causal diagrams and the accompanying language of *do*-calculus. Causal diagrams are a means of precisely representing current knowledge. *Do*-calculus consists of a set of rules that allow us to express a causal quantity that we want to estimate in terms of quantities that can be computed from data. That is, it is a set of rules that allows us to express the effect of an **intervention** in terms of observational data quantities. An interventional effect is specified with the *do*-operator as with $P(Y \mid do(X))$ to indicate a deliberate intervention. This is usually quite different from the observational quantity $P(Y \mid X)$ as a classic confounding example illustrates. Let $Y$ denote reading ability and $X$ denote shoe size. We all know that age confounds the relationship as it is a cause of both shoe size and reading ability (provided the individual benefits from education). Were it not completely insane, intervening on shoe size would not change $P(Y \mid do(X))$, but the observational quantity $P(Y \mid X)$ does change as $X$ changes.

It was not intuitive for me that the effect of an intervention that has not actually been carried out could be estimated from observational data, but Pearl builds up these ideas in *The Book of Why* to explain (in Chapter 7) that 3 rules of *do*-calculus suffice for determining if a particular causal effect can be estimated from observation data given a causal diagram. [This blog post](https://www.inference.vc/untitled/) gives more technical details about causal diagrams and *do*-calculus, and the [introductory paper](https://arxiv.org/abs/1305.5506) cited in that post explains the 3 rules in detail.

The combination of causal diagrams and *do*-calculus is a powerful idea for me because of the precision of language that it offers for making causal queries. We first must lay our assumptions bare with a causal diagram. This was not a hard point to sell me on because I am already a firm believer in the power of network methods to organize domain knowledge. *Do*-calculus on the other hand is quite surprising. Still, the 3 rules provide clear guidelines on how to express a causal effect from observational data, and this clarity in allowable expressions is for me a compelling motivator for their use. Pearl mentions in the book that these rules have been algorithmized, and in my brief searching, I have found the R package called [daggity](http://www.dagitty.net/) that seems to implement the rules of *do*-calculus.

## History <a id="history"></a>

Another major theme of this book is understanding history. One entire chapter is devoted to tracing the history of how causal inference came to be. Pearl and Mackenzie start by recounting the tale of Francis Galton, a British scientist who was on a quest to understand the genetic determinants of features like height and intelligence. He eventually stumbled upon the phenomenon of regression to the mean and saw it as a physical, casual process because it was able to reconcile some peculiar features of models that he had developed for height distributions across generations. However, he eventually grew dissatisfied with the idea after finding that the phenomenon persisted regardless of which variable was treated as the causal agent. He was never able to resolve his initial causal queries about genetic determinants, but he did pass on ideas of scatterplots and correlation to future generations of statisticians. In particular, Karl Pearson took to the idea of correlation quite excitedly and came to eschew ideas of causality, which he viewed as imprecise and vague in contrast to his clean, mathematized correlation coefficient. Such was a major force behind the lack of causality research in statistics for some time. Pearl and Mackenzie end this historical chapter with the tale of Sewall Wright, who seems to have been one of the first to come up with the idea of using causal diagrams. Through Wright's tale we see a reemergence of the willingness to study casuality rigorously and quantitatively.

This historical discussion is fascinating because it allows us (with our hindsight goggles on) to understand why research progressed the way that it did. Through understanding the personalities and culture of these historical figures, we can understand why certain ideas were pursued, why shortcomings arose, and hopefully mediate ourselves to be better scientists because of this understanding.

## Cognition <a id="cognition"></a>

The importance of an awareness of human cognition is also a recurring idea in the book. Pearl motivates his journey through causal inference with his interests in artificial intelligence, and he claims that causal inference methods should ideally try to emulate the powerful causal reasoning faculties within our own minds that stem from simply asking the question: why? I like the apparent simplicity of this. It is easy to see how asking this question could give rise to causal diagrams. Each "because" becomes a directed connection from nodes that represent variables in our "because" statement. Still, at the same time, I wondered: should there not be some higher standard to which causal inference methods should aspire? Why simply aim to replicate human reasoning? Shouldn't we strive for our methods to achieve something *more* than just human reasoning in some sense? After thinking about this, I feel that these goals are too lofty. We humans are limited by our capabilities, so even if causal inference methods could achieve beyond-human reasoning, we wouldn't *know* that they were. Given a method that produces some results, we would still evaluate the method by asking, "Do those results make sense?" We would still be using our (powerful) causal reasoning capabilities to make sense of the results generated by the method. Thus even if some deeper meaning was somehow conveyed by the method, the meaning we would be able to extract from it is limited by the framework of our understanding. I feel that Pearl's claims about using human reasoning as a gold standard for causal inference methods is reasonable. This thinking also reaffirms my belief in the importance of understanding how humans interact with the tools we develop, such as through the fields of ergonomics, human-computer interaction, human-data interaction.

An awareness of human cognition is explored most extensively in a chapter on several paradoxes: the Monty Hall problem, Berkson's paradox, Simpson's paradox, and Lord's paradox. I had actualy never heard of Berkson's paradox, but it is the appearance of an association in a subpopulation that is not seen in the general population. Pearl explores all of these paradoxes in light of causal diagrams, and I actually did find it helpful to view these problems with this causal lens. The causal diagrams were useful in generalizing the structure of the situations governing the paradoxes, which I think is helpful in recognizing when they occur. Further, Pearl lays forth a reasonable set of criteria for dealing with paradoxes:

> Any claim to resolve a paradox (especially one that is decades old) 
> should meet some basic criteria. First, as I said above in connection 
> with the Monty Hall paradox, it should explain why people find the 
> paradox surprising or unbelievable. Second, it should identify the 
> class of scenarios in which the paradox can occur. Third, it should 
> inform us of scenarios, if any, in which the paradox cannot occur. 
> Finally, when the paradox does occur, and we have to make a choice 
> between two plausible yet contradictory statements, it should tell us 
> which statement is correct. (p. 202)

## Teaching <a id="teaching"></a>

By its nature, the book aims to inform readers about the development of and about central ideas in causal inference, but teaching and pedagogy are not direct themes of the book. That being said, I was amazed at how appropriate the writing of the book is for a classroom textbook. There are a lot of great thought experiments, historical examples, and activities to engage students at the undergraduate level and beyond. In particular, the chapters that recount the evolution of the smoking-lung cancer debate (Chapter 5) and that explore "statistical" paradoxes through a causal lens (Chapter 6) are great sources of classroom content.

## Conclusions <a id="conclusions"></a>

I highly recommend this book to anyone who cares about science. Even if causal inference isn't an area of interest for you, the ideas in this book are important for understanding the causal research that we otherwise consume or hear about. Pearl is very invested in these ideas, so the language in the book is very enthusiastically in favor of these methods. I can see how this might irritate some readers, but I found that the ideas he presented were compelling and interesting in their own right. Certainly these methods are not a panacea, but I do believe that they can be quite useful. Reading the book has motivated me to continue learning about these topics, and I hope that I can eventually fully understand the answers to some questions I was left with:

- Is there no reconciliation at all for Rubin causal model type methods and *do*-calculus methods?
- How can causal diagrams and *do*-calculus be used to study networks that evove with time?
- How is interference between units handled?
- How can we measure the causal effect of several variables simultaneously?
- I have heard of edges being random variables in the graphical model literature. (i.e. Arrows can point to arrows.) Is this part of the *do*-calculus framework?
