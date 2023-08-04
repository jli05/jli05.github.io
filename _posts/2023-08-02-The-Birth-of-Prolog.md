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


Bergman, Marc and Henry Kanoui, Application of mechanical theorem proving to symbolic calculus, Third International Colloquium on Advanced Computing Methods in Theoretical Physics, Marseilles, France, June 1973.

Joubert, Michel, Un système de résolution de problèmes à tendance naturelle, 3e cycle thesis, Groupe Intelligence Artificielle, Faculte des Sciences de Luminy, Université Aix-Marseille II, France, Feb 1974.
