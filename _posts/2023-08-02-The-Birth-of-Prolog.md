---
title: The Birth of Prolog
layout: post
---

[http://alain.colmerauer.free.fr/alcol/ArchivesPublications/PrologHistory/19november92.pdf](http://alain.colmerauer.free.fr/alcol/ArchivesPublications/PrologHistory/19november92.pdf)

## The Birth of Prolog
Alain Colmerauer, Philippe Roussel

### 7.5.1 Resolution Strategy
> In addition, after a visit to Edinburgh, Philippe had in mind the basis for an architecture that is extremely simple to implement from the point of view of memory management and much more efficient in terms of time and space, if we maintained the philosophy of managing nondeterminism by backtracking.

### 7.5.2 Syntax and Primitives
> It should be noted that among the basic primitives for processing morphology problems, one single primitive `UNIV` was used to create dynamically an atom from a character sequence, to construct a structured object from its elements, and, conversely, to perform the inverse splitting operations. This primitive was one of the basic tools used to create programs dynamically and to manipulate objects whose structures were unknown prior to the execution of the program.

### 7.5.4 Implementation of the Interpreter
> The system consisted of the actual interpreter (i.e. the inference machine equipped with a library of built-in predicates), a loader to read clauses in a restricted syntax, and a supervisor written in Prolog. Among other things, this supervisor contained a query evaluator, an analyzer accepting extended syntax, and the high-level input-output predicates.

### Conclusion
> The article published by Alan Robinson in January 1965, "A machine-oriented logic based on the resolution principle," contained the seeds of the Prolog language.
>
> Our contribution was to transform that theorem prover into a programming language. To that end, we did not hesitate to introduce purely computational mechanisms and restrictions that were heresies for the existng theoretical model. These modifications, so often critized, assured the viability, and thus the success, of Prolog. Robert Kowalski's contribution was to single out the concept of the "Horn clause", which legitimised our principal heresy: a strategy of linear demonstration with backtracking and with unification only at the heads of clauses.
>
> ... We have had the pleasure of recalling it for this paper over fresh almonds accompanied by a dry martini.

### References
Bergman, Marc and Henry Kanoui, Application of mechanical theorem proving to symbolic calculus, Third International Colloquium on Advanced Computing Methods in Theoretical Physics, Marseilles, France, June 1973.

Joubert, Michel, Un système de résolution de problèmes à tendance naturelle, 3e cycle thesis, Groupe Intelligence Artificielle, Faculte des Sciences de Luminy, Université Aix-Marseille II, France, Feb 1974.

### Transcript of Presentation
#### Slide 15
> This graph representation leads to an efficient bottom-up parser which keeps most common parts together.
>
> The important thing was that, within a rule, you were allowed to speak about complex symbols and complex symbols were just trees, ...

Separately, [https://www.openbookproject.net/py4fun/prolog/prolog1.html](https://www.openbookproject.net/py4fun/prolog/prolog1.html) and [https://openbookproject.net/py4fun/prolog/prolog2.html](https://openbookproject.net/py4fun/prolog/prolog2.html) provide a simplest implementation of Prolog and its `unify()` routine. A git repo is at [https://github.com/brief-ds/prolog](https://github.com/brief-ds/prolog).

A book on the unification is Ait-Kacl, H. 1991. The WAM: A (Real) Tutorial. MIT Press, Cambridge, MA. for Warren Abstract Machine. [https://mitpress.mit.edu/9780262510585/warrens-abstract-machine/](https://mitpress.mit.edu/9780262510585/warrens-abstract-machine/)
