---
title: Anaphoric Resolution
layout: post
---

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

# An Iterative Rule-based Anaphoric Resolution Algorithm

For each Mention, we try to extract the following properties: speaker name/person name, gender, plurality, NER tag. For two Mentions to be clustered together, if both have a non-null common property, they must have the same value for that property. For each entity, we also maintain a list of head words of all the coreferent mentions.

The algorithm is as follows:

1. Cluster only the Named Entities, e.g. "Jaguar". If two Named Entities share the same NER tag, plurality, and head word, they’re clustered into one;

2. Cluster non-pronominal phrases, e.g. "the car". If one shares the same properties with a Named Entity, or another non-pronominal phrase, and if it either has the same head word with that coreferent match, or from the training corpus its head word could co-occur with the head word of the match e.g. "Jaguar" vs "the car", it is clustered with that entity.

3. Cluster pronouns. In the search we wont’t cluster one pronoun with another pronoun; we always try to pinpoint a Named Entity or a non-pronominal phrase that it refers to.

  1. First-person pronouns: if the speaker of the mention is given, it’s anchored to that speaker, otherwise a new entity is created for this first-person noun.

  2. Second-person pronouns: we generally don’t have sufficient information to identify the person name it refers to, so we always create an entity for the second-person pronoun. (apart from cases like "You Mr Dick lied." but it's not always reliable to always corefer two juxtaposing NP.)

  3. Third-person pronoun: We apply a simplified Hobbs algorithm.

     1. We go from the current sentence of the mention, do the left-to-right search of mentions, then go backward one sentence, repeat the search, until we find the first consistent non-pronominal match, or we have searched through $K$ prior sentences. (We used $K=5$)

     2. For each sentence, we sort the mentions in that sentence in the left-to-right order, and bigger-span mentions take precedence over shorter ones if both start from the same word index, to mimic the breadth-first, left-to-right search of the Hobbs algorithm

     3. However, for the current sentence of the mention, we don’t use the entire sentence for search, we find the 2nd node that’s either NP, S, VP, or SBAR immediately dominating the mention, and only do search to the left of it, to mimic the spirit of Hobbs proposition regarding the scope of search. 

     4. We avoid doing search to the right of the scope, as there doesn’t feel to be any safe rule of clustering considering real language uses.

4. For any unclustered mention after Step 3, we create a new Entity for each distinct combination of the four properties, head word of the mention.

# Intuition of the Hobbs Algorithm
The layout of the algorithm is cryptic, however consider the following sentence:

> It's not always optimal to minimize the measure of the mismatch of the semantics, though it is good to have it.

To corefer the last "it" we could use the Hobbs algorithm, which is a statistical approach, otherwise we need to understand what the sentence means, especially involving the words "minimize", "though".

# References regarding the Hobbs Algorithm
There's little reference on the web, below is a good survey.

[http://people.ucsc.edu/~abrsvn/anaphora%20resolution.ppt](http://people.ucsc.edu/~abrsvn/anaphora%20resolution.ppt)
