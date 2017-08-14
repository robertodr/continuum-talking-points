name: inverse
layout: true
class: center, middle, inverse
---
## Solvation Models

.subtitle[Implementation challenges, lessons learnt, best practices]

.author[Marco Caricato]

.institution[The University of Kansas]

.author[Roberto Di Remigio]

.institution[UiT - The Arctic University of Norway]

.footnote[[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) licensed.
Browse slides at [http://tinyurl.com/continuum-talking-points](http://tinyurl.com/continuum-talking-points)]

---
layout: false

## Software Development Is Essential to Quantum Chemistry

- Software architecture must allow fast development of new methods.
- Computer implementations must be reproducible.red[<sup>1</sup>]
- New modelling challenges require new (or improved) methods and new (or
  improved) software implementations.
- Software cannot be rewritten from scratch every time, we need to have
  **extensible** architectures
- Fast-paced development cycles can only occur within a clear software structure
- **Modularity**. Delimit capabilities, concerns, results.
- Reinventing the wheel is bad
- Build small, well-documented software components
- Reuse components in different contexts

.footnote-cite[.red[<sup>1</sup>] Hatton, L.; Warr, G. [arXiv [q-bio.QM]](https://arxiv.org/abs/1608.06897), (2016)]

---
layout: false
class: split-50-50
## Quantum/Classical __Polarizable__ Multiscale Models.red[<sup>1</sup>]

- Use **quantum** and **classical** physics together
  * quantum for chemically relevant subsystem
  * classical **and** polarizable for the environment

.column[
<p style="text-align:center;"><img src="images/gfp_barrel.png" style="width: 100%"></p>
<p style="clear: both;">
]
.column[
<p style="text-align:center;"><img src="images/pyridine_Continuum.png" style="width: 50%"></p>
<p style="clear: both;">
]
.footnote-cite[.red[<sup>1</sup>] Senn, H. M.; Thiel, W. _Angew. Chem. Int. Ed._ (2009), __48__, 1198]

???

- The idea of multiscale models is to _focus_ on the chemically relevant part
  of the system and treat the environment approximately.

---
layout: false

## Quantum/Classical __Polarizable__ Multiscale Models

- Discrete and continuum are similar
- Solution of a linear system of equations to obtain polarization
`$$
 \color{Green}{\mathbf{A}}\color{Red}{\mathbf{p}} = - \color{Green}{\mathbf{B}}\color{Blue}{\mathbf{s}}
$$`

1. What should be the boundary between classical and quantum software?
  - What should the quantum module take care of?
  - How to ensure extensibility?

1. Can we exploit the similarity?
  - What should the classical module take care of?
  - How should quantum and classical communicate?

2. What is needed to layer different methods for the environment?
  - How should different classical layers communicate?
  - Variational formulations?

3. Performance bottlenecks and how to overcome them.

---
layout: false

## Quantum Layer

1. Integrals
  - Interoperable formats for data transfer?
2. SCF procedure
  - Updates of the energy
  - Updates of the Fock matrix
3. Response calculations
4. Post-SCF methods

---
layout: false

## Continuum layer

1. Generating the cavity
  - Reading the geometry
  - Identifying functional groups
  - Discretization
  - Is there an interoperable format to transfer cavity information?

2. Classical solver
  - Environment representations, _i.e._ Green’s functions
  - Operator representations, _i.e._ collocation _vs_ Galerkin _vs_ domain decomposition
  - Linear system solver

---

## A Checklist of Sorts for Application Programming Interfaces

_In computer programming, an __application programming interface__ (__API__) is
a set of subroutine definitions, protocols, and tools for building software and
applications._

### Encapsulation

- Hide internals by language or by convention
- Interface exposed in a separate file
- Expose the **what**, hide the **how**

### Development history

- Decouple the development history
- Each unit should have its own Git history/repository

.footnote-cite[Source: Wikipedia [Application programming interface](https://en.wikipedia.org/wiki/Application_programming_interface)]

---

## A Checklist of Sorts for Application Programming Interfaces, contd.

### Documentation

- Separate the **what it can do** from **how is it implemented**
- Documented **application programming interface** (API)
- Versioned API ([semantic](http://semver.org) or [sentimental](http://sentimentalversioning.org)
  or [romantic](https://github.com/jashkenas/backbone/issues/2888#issuecomment-29076249) versioning)

### Testing

- You don't want to find a bug in the Alps
- Sharpens interfaces
- Exposes coupling and cohesion

### Building and deploying

- Prerequisite for testable on its own
- Suitable distribution mechanisms

---
name: last-page
template: inverse

# Thanks for your attention!

Slideshow created using [remark] and served using [cicero]

Slides available on [GitHub](https://github.com/robertodr/continuum-talking-points)

Browse slides at [http://tinyurl.com/continuum-talking-points](http://tinyurl.com/continuum-talking-points)

[remark]: https://github.com/gnab/remark
[cicero]: https://github.com/bast/cicero
