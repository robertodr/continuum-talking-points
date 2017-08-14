name: inverse
layout: true
class: center, middle, inverse
---
## The Polarizable Continuum Model Goes Viral!

.subtitle[Extensible, Modular and Sustainable Development of Quantum Mechanical Continuum Solvation Models]

.author[Roberto Di Remigio]

.institution[UiT - The Arctic University of Norway]

.date[16 January 2017, Tromsø]

.footnote[[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) licensed.
Browse slides at [http://tinyurl.com/phd-forsvaring](http://tinyurl.com/phd-forsvaring)]

???

This talk is an introduction to continuum solvation models and in particular to
the polarizable continuum model (PCM)
I will introduce the basic features of the model and talk about our
implementation as an independent library

---

# Prologue

---
layout: false

## The Problem of Solvation

Aspects computational models need to capture
- Dynamical features of solvation
- Average nature of observables
- Variety of interactions

<p style="text-align:center;"><img src="images/solvation-dynamics.gif" style="width: 70%"></p>
<p style="clear: both;">

???

- Chemistry is a wet science: experiments mostly happen in solution.
- We can classify solvent effects as:
  * Direct: these effects stem straightforwardly from the modification
    underwent by the solute electronic density when interacting with the
    environment.
  * Indirect: it is common for solutes to exhibit different minimum-energy
    conformations in different environments. These effects are commonly
    labelled as indirect.
  * Local field: light-matter interactions are also affected by the
    environment. Local modifications of externally applied fields subtly
    influence molecular responses.
  * Dynamic: the presence of the environment radically influences excited
    states, since relaxation processes in the medium become important.
  * Specific:  This catch-all category includes all effects stemming from the
    peculiar solute-solvent pair interactions that cannot be fully described
    under any of the previous labels. In general, modelling such effects
    demands an atomistic level of detail.

<font color="red">
Problems
</font>
<p style="clear: both;">

- Size \\(\Leftrightarrow\\) accurate _ab initio_ not possible
- Conformations \\(\Leftrightarrow\\) statistical sampling

- Size is a huge problem. It is simply not possible to model large systems very
  accurately.
- Moreover, the conformational space is very large and needs to be sampled
  extensively to have meaningful results.

---
layout: false

## Viral!
### Software Development Is Essential to Quantum Chemistry
<p style="text-align:center;"><img src="images/viral.svg" style="width: 100%"></p>
<p style="clear: both;">

???

- Software development in essential to quantum chemistry.
- Software architecture must allow fast development of new methods. We want to
  solve physical problems, not software engineering problems.
- The central tenet of the scientific method is reproducibility. Our work is
  based on models and their computer simulation, these must be reproducible
  too.red[<sup>1</sup>]

.footnote-cite[.red[<sup>1</sup>] Hatton, L.; Warr, G. [arXiv [q-bio.QM]](https://arxiv.org/abs/1608.06897), (2016)]

---
layout: false

## Extensible
### New Challenges! New Methods! New Software?!?

<p style="text-align:center;"><img src="images/yoda-future-once.gif" style="width: 100%;"/></p>
<p style="clear: both;">

???

- New modelling challenges require new (or improved) methods and new (or
  improved) software implementations.
- Software cannot be rewritten from scratch every time, we need to have
  _extensible_ architectures

---
layout: false

## Modular
### Do One Thing, Do It Well
<p style="text-align:center;"><img src="images/lego-once.gif" style="width: 100%"></p>
<p style="clear: both;">

???

- Fast-paced development cycles can only occur within a clear software structure
- Delimit capabilities, concerns, results
- Modularity can help create such a structure
- A LEGO-like structure can enhance our capabilities to reason about complex software workflows.
- Assemble, de-assemble, re-assemble is easier with standalone modules.

---
layout: false

## Sustainable
### Software Reuse for a Better Life

<p style="text-align:center;"><img src="images/reinvent-wheel.gif" style="width: 100%;"/></p>
<p style="clear: both;">

.footnote-cite[Source: [IBM Big Data & Analytics Hub](http://www.ibmbigdatahub.com/blog/dont-reinvent-wheel-increase-productivity-strategic-reuse)]

???

- Reinventing the wheel is bad
- Build small, well-documented software components
- Reuse components in different contexts

---
layout: false

## Quantum Chemistry

### **Understand** and **predict** chemistry from first principles

<p style="text-align:center;"><img src="images/model-chem.svg" style="width: 80%"></p>
<p style="clear: both;">

???

- Quantum many-body theory applied to chemistry

`$$
H\Psi = E\Psi
$$`

- Compute molecular energies and properties

---
layout: false

## March of the Solvation Robots

<p style="text-align:center;"><img src="images/mind-map.png" style="width: 100%"></p>
<p style="clear: both;">

???

In the thesis we have based the treatment of solvent effects in quantum chemistry
on two "pillars":
1. A variational formulation of quantum/classical polarizable Hamiltonians
2. A modular implementation of the PCM

Based on these two pillars, we have achieved:
1. The coupling of continuum solvation with 4-component relativistic SCF
   methods for the computation of energies and selected magnetic properties.
2. Numerically more robust solvers with higher accuracy for the solution of the
   PCM electrostatic problem.
3. An extension of the range of applicability of the PCM to complex
   environments, characterized by a position-dependent permittivity.
4. An open-ended formulation of molecular response properties at the SCF level
   of theory.
5. A unifying view of the MBPT and CC models when including solvent effects.

---
layout: false
class: center, middle, inverse

# Foundations

<p style="text-align:center;"><img src="images/foundations.png" style="width: 100%"></p>
<p style="clear: both;">

---
layout: false
class: split-50-50
## Quantum/Classical Multiscale Models.red[<sup>1</sup>]

- Use **quantum** and **classical** physics together
  * quantum for chemically relevant subsystem
  * classical for the environment

.column[
<p style="text-align:center;"><img src="images/gfp_barrel.png" style="width: 100%"></p>
<p style="clear: both;">
]
.column[
<p style="text-align:center;"><img src="images/pyridine+water_12AA_QMMM.png" style="width: 50%"></p>
<p style="clear: both;">
]
.footnote-cite[.red[<sup>1</sup>] Senn, H. M.; Thiel, W. _Angew. Chem. Int. Ed._ (2009), __48__, 1198]

???

- The idea of multiscale models is to _focus_ on the chemically relevant part
  of the system and treat the environment approximately.

---
layout: false
class: split-60-40

## QM/Continuum.red[<sup>2</sup>]

- Use **quantum** and **classical** physics together
  * quantum for chemically relevant subsystem
  * _continuum dielectric_ for the environment

.column[
.red[Pros]
- **Exact** electrostatics
- Self-consistent polarization
- Statistically averaged _by construction_

.red[Cons]
- **No** chemical detail in the environment
- Dispersion and repulsion approximate
]
.column[
<p style="text-align:right;"><img src="images/pyridine_Continuum.png" style="width: 40%"></p>
<p style="clear: both;">
]

.footnote-cite[.red[<sup>2</sup>] Tomasi, J.; Mennucci, B.; Cammi, R. _Chem. Rev._ (2005), __105__, 2999]

???

---
layout: false
class: split-60-40

## QM/Continuum: The Polarizable Continuum Model.red[<sup>2</sup>]

.column[
### Transmission problem
`$$
  \begin{align}
  L_\mathrm{i} u &= \nabla^2 u = -4\pi\rho \,\, \text{in}\,\, \Omega_\mathrm{i} \label{eq:internal} \\
  L_\mathrm{e} u &= 0 \,\, \text{in}\,\, \Omega_\mathrm{e} \label{eq:external} \\
  [u](\mathbf{s}) &= u_\mathrm{e} - u_\mathrm{i} = 0 \,\, \text{on}\,\, \Gamma
  \label{eq:trace-jump} \\
[\partial_L u](\mathbf{s}) &= \partial_{L_\mathrm{e}} u - \partial_{L_\mathrm{i}} u = 0 \,\, \text{on}\,\, \Gamma \label{eq:conormal-jump} \\
|u(\mathbf{r})| &\leq C \|\mathbf{r} \|^{-1} \,\,\text{for}\,\,\| \mathbf{r} \|\rightarrow\infty
\label{eq:radiation}
\end{align}
$$`
]
.column[
<p style="text-align:right;"><img src="images/alanine.svg" style="width: 40%"></p>
<p style="clear: both;">
]

.footnote-cite[.red[<sup>2</sup>] Tomasi, J.; Mennucci, B.; Cammi, R. _Chem. Rev._ (2005), __105__, 2999]

???

- Replace environment with continuum \\(\varepsilon\\)
- Create cavity in continuum \\(\Omega_i\\)
- Vacuum inside cavity \\(\varepsilon=1\\)
- Solute charge density _entirely_ in \\(\Omega_i\\)

- Model the solvent as a polarizable dielectric continuum
- Parameters for the definition of the boundary, i.e. the cavity
- Parameters describing the solvent: permittivity (static and optical)
- Notice that the solvent parameters are, _by definition_, averaged!
- \\(L_\star\\) are elliptic differential operators
- Trace operators are the extension of the concept of restriction of a function
  over a boundary to generalized functions in Sobolev space
- Conormal derivative extends the notion of a normal derivative to functions in
  Sobolev spaces
- \\(\sigma(\mathbf{s})\\) is called the apparent surface charge (ASC)

* \\(L_\star\\) are elliptic differential operators
* Dirichlet condition: \\([u] (\mathbf{s})\\)
* Neumann condition: \\([\partial_L u] (\mathbf{s})\\)

---
## Mathematics of PCM.red[<sup>3</sup>]

- Define the .red[reaction potential]
`$$
 u(\mathbf{r}) = \color{Blue}{\varphi(\mathbf{r})} + \color{Red}{\xi(\mathbf{r})}
 = \int_C \mathop{}\!\mathrm{d}\mathbf{r}^\prime \frac{\color{Blue}{\rho(\mathbf{r}^\prime)}}{|\mathbf{r} - \mathbf{r}^\prime|} +
  \int_{\partial C} \mathop{}\!\mathrm{d}\mathbf{s} \frac{\color{Red}{\sigma(\mathbf{s})}}{|\mathbf{r} - \mathbf{s}|}
$$`

- .red[Apparent surface charge (ASC)]
`$$
\color{Green}{\mathcal{T}}\color{Red}{\sigma(\mathbf{s})} = -\color{Green}{\mathcal{R}}\color{Blue}{\varphi(\mathbf{s})}
$$`

- Green's functions for \\(L_\star\\) define integral operators

`$$
 \begin{align}
  (\color{Green}{\mathcal{S}_\star} f)(\mathbf{s}) &=
\int_{\partial C} \mathop{}\!\mathrm{d}\mathbf{s}^\prime \color{Green}{G_\star(\mathbf{s}, \mathbf{s}^\prime)}f(\mathbf{s}^\prime) \\
  (\color{Green}{\mathcal{D}_\star} f)(\mathbf{s}) &=
\int_{\partial C} \mathop{}\!\mathrm{d}\mathbf{s}^\prime [\partial_{L_\star}^\prime\color{Green}{G_\star(\mathbf{s}, \mathbf{s}^\prime)}]f(\mathbf{s}^\prime) \\
  (\color{Green}{\mathcal{D}^\dagger_\star} f)(\mathbf{s}) &=
\int_{\partial C} \mathop{}\!\mathrm{d}\mathbf{s}^\prime [\partial_{L_\star}\color{Green}{G_\star(\mathbf{s}, \mathbf{s}^\prime)}]f(\mathbf{s}^\prime)
 \end{align}
$$`

.footnote-cite[.red[<sup>3</sup>] Cancès, E.; Mennucci, B. _J. Math. Chem._ (1998), __23__, 309]

???

- We've transformed a boundary value problem (BVP) into a boundary integral equation (BIE)
- Integral operators are defined in terms of traces and conormal derivatives
- The integral operators have well-defined mapping properties between Sobolev
  spaces of fractional order
- Knowledge of the Green's functions inside and outside the cavity is key to
  the method

---

## A PCM for All Seasons.red[<sup>3</sup>]

- Environments with anisotropies and nonuniformities
`$$
\begin{align}
\left[\left(2\pi\mathcal{I} - \color{Green}{\mathcal{D}_\mathrm{e}}\right)\color{Green}{\mathcal{S}_\mathrm{i}} +
      \color{Green}{\mathcal{S}_\mathrm{e}}\left(2\pi\mathcal{I} + \color{Green}{\mathcal{D}^\dagger_\mathrm{i}}\right) \right]&\color{Red}{\sigma(\mathbf{s})} = \\
&-\left[\left(2\pi\mathcal{I} - \color{Green}{\mathcal{D}_\mathrm{e}}\right) -
\color{Green}{\mathcal{S}_\mathrm{e}}\color{Green}{\mathcal{S}_\mathcal{i}^{-1}}\left(2\pi\mathcal{I} - \color{Green}{\mathcal{D}_\mathrm{i}}\right)\right]\color{Blue}{\varphi(\mathbf{s})}
\end{align}
$$`
- Isotropic environments
`$$
\left[ 2\pi \left(\frac{\varepsilon+1}{\varepsilon-1}\right)\mathcal{I} - \color{Green}{\mathcal{D}}\right]
\color{Green}{\mathcal{S}}\color{Red}{\sigma(\mathbf{s})} = -\left( 2\pi\mathcal{I} - \color{Green}{\mathcal{D}} \right)
\color{Blue}{\varphi(\mathbf{s})}
$$`
- Conductor-like boundary conditions
`$$
\color{Green}{\mathcal{S}}\color{Red}{\sigma(\mathbf{s})} = -f(\varepsilon)\color{Blue}{\varphi(\mathbf{s})}$$`

<font color="red">
<center>How to solve these? <img src="images/fearful.svg" style="width: 5%"></center>
</font>
<p style="clear: both;">

.footnote-cite[.red[<sup>3</sup>] Cancès, E.; Mennucci, B. _J. Math. Chem._ (1998), __23__, 309]

???

---
layout: false
class: split-70-30

## Boundary Element Method and PCM.red[<sup>4</sup>]

.column[
Solution by a boundary element method (BEM) <img src="images/smiley.svg" style="width: 5%">

* Cavity of interlocking, atom-centered spheres
* \\(N_\mathrm{ts}\\) finite elements on the cavity boundary
* Form boundary integral operators
`$$
 \color{Green}{\mathbf{T}}\color{Red}{\mathbf{q}} = - \color{Green}{\mathbf{R}}\color{Blue}{\mathbf{v}}
$$`
* Solve linear system
]
.column[
<p style="text-align:right;"><img src="images/benzene_GePol.png" style="width: 30%"></p>
<p style="clear: both;">
]

- Similar equation for IEF-PCM, isotropic PCM and COSMO
- _Independent_ of QM method!

.footnote-cite[.red[<sup>4</sup>] Ern, A; Guermond, J.-L. _Theory and Practice of Finite Elements_, Springer, 2004]

???

- _Galerkin_ or _collocation_ method
- Direct inversion or iterative solver

---
layout: false

## Variational Formulation of the PCM.red[<sup>5</sup>]

Find \\(\color{Red}{\sigma(\mathbf{s})}\\) minimizing
`$$
U_\mathrm{PCM}[\color{Red}{\sigma(\mathbf{s})}]
=\frac{1}{2}\int_{\partial C} \mathop{}\!\mathrm{d}\mathbf{s} \color{Red}{\sigma(\mathbf{s})} \color{Green}{[\mathcal{R}^{-1}\mathcal{T}]} \color{Red}{\sigma(\mathbf{s})}
+\int_{\partial C} \mathop{}\!\mathrm{d}\mathbf{s} \color{Blue}{\varphi(\mathbf{s})}\color{Red}{\sigma(\mathbf{s})}
$$`

1. Physically meaningful
   - Unfavourable self-interaction of \\(\color{Red}{\sigma(\mathbf{s})}\\)
   - Favourable interaction of \\(\color{Red}{\sigma(\mathbf{s})}\\) and  \\(\color{Blue}{\varphi(\mathbf{s})}\\)

2. Equilibrium values are energies
3. Convexity

.footnote-cite[.red[<sup>5</sup>] Lipparini, F.; Scalmani, G.; Mennucci, B.; Cancès, E.; Caricato, M.; Frisch, M. J. _J. Chem. Phys._ (2010), __133__, 014106]

???

---
layout: false

## The Importance of Being Variational.red[<sup>6</sup>]

<p style="text-align:center;"><img src="images/variational-brain.png" style="width: 85%"></p>
<p style="clear: both;">

1. Avoids nonlinear couplings in the QM Hamiltonian
2. _Classical_ Hellmann-Feynman theorem
`$$
\frac{\mathop{}\!\mathrm{d}U_\mathrm{pol}}{\mathop{}\!\mathrm{d} F} =
\frac{\partial U_\mathrm{pol}}{\partial F}
+\cancelto{0}{\frac{\partial U_\mathrm{pol}}{\partial \color{Red}{\mathrm{p}}}\frac{\partial \color{Red}{\mathrm{p}}}{\partial F}}
$$`
3. **Valid for polarizable MM models**
4. Extended Lagrangian dynamics


.footnote-cite[.red[<sup>6</sup>] Lipparini, F.; Lagardère, L.; Raynaud, C.; Stamm, B.; Cancès, E.; Mennucci, B.; Schnieders, M.; Ren, P.; Maday, Y.; Piquemal, J.-P.
_J. Chem. Theory Comput._ (2015), __11__, 623]

???

Classical polarization energy of mixed discrete-continuum system.
`$$
U_\mathrm{pol}[\mathrm{p}] = \frac{1}{2}\mathrm{p}\color{Green}{\mathbb{V}}\mathrm{p} + \mathrm{p}\mathrm{s}
$$`
- p is the polarization degree of freedom
- s is the source term
- V is the classical interaction operator
- \\(\mathrm{p}\\) is an _independent_ degree of freedom
- _linear_ QM/classical coupling through

---
layout: false

## Self-Consistent Field with PCM

* .blue[Molecular electrostatic potential (MEP)]
  `$$
  \varphi(\mathbf{s}) = \sum_{\kappa\lambda}^{N_\mathrm{AO}} \color{Blue}{\varphi_{\kappa\lambda}(\mathbf{s})} D_{\lambda\kappa},
  \quad
  \color{Blue}{\varphi_{\kappa\lambda}(\mathbf{s}) }= \int\mathop{}\!\mathrm{d}\mathbf{r} \frac{-\chi^*_\kappa(\mathbf{r})\chi_\lambda(\mathbf{r}) }{|\mathbf{r} - \mathbf{s}|}
  $$`
* .red[Apparent surface charge (ASC)]
  `$$
   \color{Green}{\mathcal{T}}\color{Red}{\sigma(\mathbf{s})} = - \color{Green}{\mathcal{R}}\color{Blue}{\varphi(\mathbf{s})}
  $$`
* Fock matrix
  `$$
   f_{\kappa\lambda} =
   f^\mathrm{vac}_{\kappa\lambda} +
    (\color{Red}{\sigma(\mathbf{s})}, \color{Blue}{\varphi_{\kappa\lambda}(\mathbf{s})})_{
  \partial C}
  $$`

<p style="text-align:center;"><img src="images/scf.png" style="width: 100%"></p>
<p style="clear: both;">

???

---

## Application Programming Interfaces to the Rescue

_In computer programming, an __application programming interface__ (__API__) is
a set of subroutine definitions, protocols, and tools for building software and
applications._

<p style="text-align:center;"><img src="images/hypnotoad-api.gif" style="width: 45%;"/></p>
<p style="clear: both;">

.footnote-cite[Source: Wikipedia [Application programming interface](https://en.wikipedia.org/wiki/Application_programming_interface)]

---
class: split-50-50

## How to API Like a Pro

.column[
__Encapsulation__ Decide what others should know

__Development history__ Know your past

__Documentation__ Let others know what you can do
]
.column[
__Versioning__ What changed?

__Testing__ Did you mess up?

__Building__ Can it stand on its own feet?

__Deploying__ Get it out in the open!
]

<p style="text-align:center;"><img src="images/stranger-in-the-alps-twice.gif" style="width: 80%;"/></p>
<p style="clear: both;">

???

### Encapsulation

- Hide internals by language or by convention
- Interface exposed in a separate file
- Expose the **what**, hide the **how**

### Development history

- Decouple the development history
- Each unit should have its own Git history/repository

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

## PCMSolver: An API for the PCM.red[<sup>7</sup>]

- **Free and open-source** [LGPLv3](https://www.gnu.org/copyleft/lesser.html)-licensed
- Developed on [GitHub](https://github.com/PCMSolver/pcmsolver)
- C++ (~5000 LOC) and Fortran (~3000 LOC)

<p style="text-align:center;"><img src="images/pcmsolver-scheme.png" style="width: 60%"></p>
<p style="clear: both;">

.footnote-cite[.red[<sup>7</sup>] Find it at [pcmsolver.readthedocs.io](http://pcmsolver.readthedocs.io/en/latest/) and [github.com/PCMSolver/pcmsolver](https://github.com/PCMSolver/pcmsolver)]

???

- Coupling
    - Assemble/deassemble/reassemble
    - Reason about/understand the code

- Low cohesion: difficult to maintain, test, reuse, or even understand
    - Non-cohesive code has unnecessary dependencies
    - Swiss army knife modules

- High cohesion: associated with robustness, reliability, reusability, and understandability
    - Do one thing only and do it well
    - API of cohesive code changes less over time
    - Power of the Unix command line is a set of highly cohesive tools
    - Microservices

---

## PCMSolver: An API for the PCM.red[<sup>7</sup>]

- Capabilities
  * Traditional collocation solvers
  * Isotropic and anisotropic Green's functions
  * Wavelet solvers (_unreleased_)
  * Real-time solvers (_in progress_)

- Build system
  * [CMake](https://cmake.org/) _via_ [Autocmake](http://autocmake.readthedocs.io/en/latest/)
  * Static and shared libraries available

- API
  * Pure C for interoperability
  * Fortran bindings built on-demand

.footnote-cite[.red[<sup>7</sup>] Find it at [pcmsolver.readthedocs.io](http://pcmsolver.readthedocs.io/en/latest/) and [github.com/PCMSolver/pcmsolver](https://github.com/PCMSolver/pcmsolver)]

---
layout: false

## PCMSolver: An API for the PCM.red[<sup>7</sup>]

- Testing
  * Unit tests with [Catch](https://github.com/philsquared/Catch) C++ framework
  * The `master` branch has 51 tests
  * _Continuous integration_ (CI) on [Travis](https://travis-ci.org/) and [Magnum](https://magnum-ci.com/)

- Release process
  * Public repo on GitHub
  * Private repo on GitLab
  * Precompiled library served on [Anaconda cloud](https://anaconda.org/psi4/pcmsolver)

- Documented
  * Doxygen for _in-code_ documentation
  * Sphinx for users and programmers documentation
  * Served on [ReadTheDocs](https://readthedocs.org/)

.footnote-cite[.red[<sup>7</sup>] Find it at [pcmsolver.readthedocs.io](http://pcmsolver.readthedocs.io/en/latest/) and [github.com/PCMSolver/pcmsolver](https://github.com/PCMSolver/pcmsolver)]

---
layout: false

## One PCM to rule them all

<p style="text-align:center;"><img src="images/boromir-ring-once.gif" style="width: 70%"></p>
<p style="clear: both;">

### Currently interfaced programs

- DALTON
- LSDALTON
- Psi4
- DIRAC
- ReSpect
- KOALA

???

### What's in it for us?

- Separation of concerns
- Leveraging functionality


## Interfaces to QM programs

- DALTON (w/ A. H. Steindal, K. Ruud, M. Ringholm _et al._)
  * HF and DFT up to linear electric response properties
  * Interfaced also to open-ended response code (only electric)
  * Status: **unreleased**

- LSDALTON (w/ S. S. Reine)
  * HF and DFT up to linear electric response properties
  * Status: **released**

- Psi4 (w/ T. Daniel Crawford, A. Simmonett, L. A. Burns)
  * HF and DFT
  * Status: **released**

- DIRAC (w/ T. Saue)
  * 4-component, Kramers restricted, HF and DFT up to linear static electric response properties
  * Status: **released**

- ReSpect (w/ M. Repisky, S. Komorowsky, K. Ruud, S. Pipolo)
  * 4-component, Kramers restricted and unrestricted HF and DFT
  * Real-time propagation _in progress_
  * Status: **unreleased**

- KOALA (w/ S. Höfener)
  * Subsystem DFT up to linear electric response properties
  * Status: **unreleased**


---

## Using PCMSolver

- Loosely coupled
- Highly cohesive

```Fortran
! Initialize PCMSolver
! --------------------
! 1. Generate cavity and BEM solver
pcm_context = pcmsolver_new(...)
! 2. Get cavity points, needed to compute MEP
call pcmsolver_get_centers(pcm_context, ...)

! SCF Cycle
! ---------
! 1. Set surface function with MEP@cavity
call pcmsolver_set_surface_function(pcm_context, ...)
! 2. Compute ASC given the MEP
call pcmsolver_compute_asc(pcm_context, ...)
! 3. Get values of ASC@cavity from surface function
call pcmsolver_get_surface_function(pcm_context, ...)
! 4. Compute polarization energy from MEP/ASC pair
energy = pcmsolver_compute_polarization_energy(pcm_context, ...)
! Clean up API context
call pcmsolver_delete(pcm_context)
```

???

[Context-aware](https://github.com/bast/context-api-example) API
  * Confine state to module
  * Multiple contexts possible
  * Operations require context
  * Enforce namespacing by convention
  * `get` and `set` functions

[Surface functions](http://pcmsolver.readthedocs.io/en/latest/users/interfacing.html#how-pcmsolver-handles-potentials-and-charges-surface-functions)
  * PCM \\(\Leftrightarrow\\) functions defined on cavity surface
  * Function label + function data = surface function
  * Map of labels and data

```Fortran
use, intrinsic :: iso_c_binding
use pcmsolver
type(c_ptr) :: pcm_context
character(kind=c_char, len=1) :: mep_lbl(7) =  &
                                 (/'N', 'u', 'c', 'M', 'E', 'P', c_null_char/)
character(kind=c_char, len=1) :: asc_lbl(7) =  &
                                 (/'N', 'u', 'c', 'A', 'S', 'C', c_null_char/)
real(c_double), allocatable :: grid(:), mep(:), asc(:)
integer(c_int) :: irrep
integer(c_int) :: grid_size, irr_grid_size
real(c_double) :: energy

pcm_context = pcmsolver_new(PCMSOLVER_READER_OWN,           &
                            nr_nuclei, charges, coordinates, &
                            symmetry_info, host_input)

call pcmsolver_print(pcm_context)
grid_size = pcmsolver_get_cavity_size(pcm_context)
irr_grid_size = pcmsolver_get_irreducible_cavity_size(pcm_context)
allocate(grid(3*grid_size))
grid = 0.0d0
call pcmsolver_get_centers(pcm_context, grid)

allocate(mep(grid_size))
mep = 0.0d0
mep = nuclear_mep(nr_nuclei, charges, &
                  reshape(coordinates, (/ 3, nr_nuclei /)), &
                  grid_size, reshape(grid, (/ 3, grid_size /)))
call pcmsolver_set_surface_function(pcm_context, grid_size, mep, mep_lbl)
irrep = 0
call pcmsolver_compute_asc(pcm_context, mep_lbl, asc_lbl, irrep)
allocate(asc(grid_size))
asc = 0.0d0
call pcmsolver_get_surface_function(pcm_context, grid_size, asc, asc_lbl)
energy = pcmsolver_compute_polarization_energy(pcm_context, mep_lbl, asc_lbl)
```

---
layout: false
class: center, middle, inverse

# PCM Above and Beyond

<p style="text-align:center;"><img src="images/mind-map.png" style="width: 100%"></p>
<p style="clear: both;">

---
layout: false

## PCM Beyond SCF: MBPT and Coupled Cluster


<p style="text-align:center;"><img src="images/excitations.png" style="width: 40%"></p>
<p style="clear: both;">
`
$$
|\mathrm{CC}\rangle = \mathrm{e}^T |\mathrm{HF}\rangle,
\quad
T = \sum_{u=1}^\mathcal{M} T_u = \sum_{u=1}^\mathcal{M}\sum_{\mu_u}t_{\mu_u}\tau_{\mu_u},
\quad
\overline{O} = \mathrm{e}^{-T}O\mathrm{e}^{T}
$$
`

### PCM-CC _effective_ Lagrangian
`
$$
\begin{align}
\mathcal{L}_\mathrm{eff}(\color{Blue}{t}, \color{Green}{\bar{t}}, \color{Red}{\mathbf{p}})_\mathcal{M} =
\langle\mathrm{HF} | \color{Blue}{\overline{H}} | \mathrm{HF}\rangle
  &+\sum_{u=1}^{\mathcal{M}}\langle\color{Green}{\bar{t}_{u}} | \color{Blue}{\overline{H}} | \mathrm{HF}\rangle
  +U_\mathrm{pol}^\mathrm{ref} \\
+\frac{1}{2}\color{Red}{\mathrm{p}}\color{Green}{\mathbb{V}}\color{Red}{\mathrm{p}} &+
\color{Red}{\mathrm{p}}\mathrm{s}_{\mathrm{N}}(\color{Blue}{t},\color{Green}{\bar{t}})_\mathcal{M}
\end{align}
$$
`
???

- Attacking the methodology axis
- Separation of reference and correlation reaction fields
- CC equations
- Everything is coupled

---
layout: false

## PTED-CC

`
$$
\begin{align}
 &\epsilon_{\mu_q}\color{Blue}{t_{\mu_q}} +\langle\mu_q | \color{Blue}{\overline{\Phi}} | \mathrm{HF}\rangle
 +\color{Red}{\mathrm{p}}\langle\mu_q | \color{Blue}{\overline{\mathrm{s}}} | \mathrm{HF}\rangle = 0 \\
 &\epsilon_{\mu_q}\color{Green}{\bar{t}_{\mu_q}}
  +\langle\mathrm{HF} | [\color{Blue}{\overline{\Phi}}, \tau_{\mu_q}] | \mathrm{HF}\rangle
  +\sum_{u=1}^{\mathcal{M}}\langle\color{Green}{\bar{t}_{u}} | [\color{Blue}{\overline{\Phi}}, \tau_{\mu_q}] | \mathrm{HF}\rangle \\
  &+\color{Red}{\mathrm{p}}\Big(\langle\mathrm{HF} | [\mathrm{s}, \tau_{\mu_q}] | \mathrm{HF}\rangle\delta_{\mu_q\mu_1}
  +\sum_{u=1}^{\mathcal{M}}\langle\color{Green}{\bar{t}_{u}} | [\mathrm{s}, \tau_{\mu_q}] | \mathrm{HF}\rangle
  +\sum_{u=1}^{\mathcal{M}}\langle\color{Green}{\bar{t}_{u}} | [[\mathrm{s}, \color{Blue}{T}], \tau_{\mu_q}] | \mathrm{HF}\rangle\Big) = 0 \\
  &\color{Green}{\mathbb{V}}\color{Red}{\mathrm{p}} + \mathrm{s}_{\mathrm{N}}(\color{Blue}{t}, \color{Green}{\bar{t}})_\mathcal{M} = 0
\end{align}
$$
`
<img src="images/pted-cc.png" style="float: center; width: 100%; margin-right: 1%; margin-top: 2.5em;">
<p style="clear: both;">

???

- Solution strategies: I need a flow chart for this!
- Approximate quantum/classical coupling schemes
- MBPT expansion: 2n+1 and 2n+2 rules
- Approximate CC methods

---
layout: false

## Barking Up The Wrong Tree?

<img src="images/barking-wrong-tree.gif" style="float: center; width: 100%; margin-right: 1%; margin-top: -1em;">
<p style="clear: both;">

<img src="images/pted-cc.png" style="float: center; width: 100%; margin-right: 1%; margin-top: -1em;">
<p style="clear: both;">

---
layout: false

## PTED-CCSD(T) and PTED-\\(\Lambda\\)CCSD(T)

.left-column[
### PTED-MBPT
]
.right-column[
- Perturbative expansion of PTED-CC
- Source operator \\(\mathrm{s}\\) is _zeroth-order_
- Use \\(2n + 1\\) and \\(2n + 2\\) rules

#### PTED-\\(\Lambda\\)CCSD(T)
`
$$
  G_{\Lambda(\text{T})}
  =\sum_{u=1}^2\langle\color{Green}{\bar{t}_{u}^{*}}| [\Phi, \color{Blue}{T_3^{*}}] | \mathrm{HF}\rangle
    +\color{Red}{\mathrm{p}^{*}}\langle\color{Green}{\bar{t}_{2}^{*}} | [\mathrm{s}, \color{Blue}{T_3^{*}}] | \mathrm{HF}\rangle
$$
`
#### PTED-CCSD(T)

`
$$
  G_{(\text{T})}
  =\sum_{u=1}^2\langle\color{Blue}{t_{u}^{*}}| [\Phi, \color{Blue}{T_3^{*}}] | \mathrm{HF}\rangle
    +\color{Red}{
  \mathrm{p} ^ { * }}\langle\color{Blue}{t_{2}^{
  *}} | [\mathrm{s}, \color{Blue}{T_3^{
  *}}] | \mathrm{
  HF}\rangle
$$
`
]

???

- Similarly for iterative approximate singles-doubles (CC2) and
  singles-doubles-triples (CC3) methods

---
layout: false

## Solvation in the Relativistic Regime

<p style="text-align:left;"><img src="images/hamilton-attack.png" style="width: 65%"></p>
<p style="clear: both;">

`$$
\mathbf{F} = \begin{pmatrix}
\mathbf{F}^{LL, \mathrm{vac}} + \mathbf{q}\cdot\mathbf{v}^{LL} & \mathbf{F}^{LS, \mathrm { vac }} \\
\mathbf{F}^{SL, \mathrm{vac}} & \mathbf{F}^{SS, \mathrm{vac}} + \mathbf{q}\cdot\mathbf{v}^{SS}
\end{pmatrix}
$$`

???

Attacking the Hamiltonian axis

---
layout: false

## Solvation in the Relativistic Regime

<p style="text-align:center;"><img src="images/paper-i.png" style="width: 90%"></p>
<p style="clear: both;">

<p style="text-align:center;"><img src="images/rela-mep.png" style="width: 80%"></p>
<p style="clear: both;">

???

Attacking the Hamiltonian axis


---
layout: false

## Relativity and Solvation: EPR and pNMR parameters

<p style="text-align:left;"><img src="images/paper-iv.png" style="width: 110%"></p>
<p style="clear: both;">

`$$
\begin{align}
g_{uv}   = \frac{2c} {\langle \tilde{S}_v \rangle} \, \mathrm{ Tr} \left\{\mathbf{\Lambda}_{B_u} \mathbf{D}^{ (J_v)} \right\},
\quad&\quad
  \left(\mathbf{\Lambda}_{B_u}\right)_{\lambda\tau} = \frac{1}{2}
  \left\langle
       \mathbf{X}_\lambda |
       \left( \mathbf{r}_{G} \times \mathbf{\alpha} \right)_u
       |
       \mathbf{X}_\tau
  \right\rangle.
\\
A^M_{uv} = \frac{1} {\langle \tilde{S}_v \rangle} \,
           \mathrm{Tr} \left\{\mathbf{\Lambda}_{I^M_u} \mathbf{D}^{(J_v)}\right\},
\quad&\quad
\left(\mathbf{\Lambda}^{\mathrm{PN}}_{I^M_u}\right)_{\lambda\tau}
    = \gamma^M
    \left\langle\mathbf{X}_\lambda
       \left|
          \left(
          \frac{\mathbf{r}_{M} \times \mathbf{\alpha}}{r^{3}_{M}}
          \right)_u
       \right|
       \mathbf{X}_\tau
    \right\rangle
\\
\delta_M^\mathrm{para} &= \frac{\mu_e}{
  12\gamma^MkT} \,\text{Tr} \left( \mathbf{g}\mathbf{A}^\mathrm{T}_M \right)
\end{align}
$$`

???

- We want properties
- Expectation values
- Particularly sensitive to the environment

---
layout: false

## Relativity and Solvation: EPR and pNMR parameters

<p style="text-align:center;"><img src="images/pNMR-tab3.png" style="width: 100%"></p>
<p style="clear: both;">

???

---
layout: false

## Spherical Diffuse Interfaces

<p style="text-align:center;"><img src="images/paper-iii.png" style="width: 100%"></p>
<p style="clear: both;">

<p style="text-align:center;"><img src="images/diffuse+profiles.png" style="width: 100%"></p>
<p style="clear: both;">

???

- Modelling of nonhomogeneous environments
  - Surface enhanced spectroscopies
  - QM/MM only recently, for planar interfaces

- Continuum approach: consider a spatially varying permittivity
  - Numerical Green's function

---
layout: false
class: split-50-50

## Green's functions in spherical symmetry

<p style="text-align:center;"><img src="images/droplet+profiles.png" style="width: 80%"></p>
<p style="clear: both;">

`
$$
\nabla \cdot [\varepsilon(\mathbf{r})\nabla G(\mathbf{r}, \mathbf{r}^\prime)] = -4\pi\delta(\mathbf{r}-\mathbf{r}^\prime)
$$
`

`
$$
\begin{align}
  G(\mathbf{r}, \mathbf{r}^\prime) = \frac{1}{C(\mathbf{r}, \mathbf{r}^\prime)|\mathbf{r}-\mathbf{r}^\prime|} &+ G_\mathrm{img}(\mathbf{r}, \mathbf{r}^\prime) \\
  &=
  \begin{cases}
    \sum\limits_{\ell=1}^\infty
    \frac{-(2\ell+1)}{r^{\prime 2}\varepsilon(r^\prime)}
    \color{Orange}{\frac{u_\ell(r)v_\ell(r^\prime)}{W_\ell(r ^\prime)}}
    P_\ell(\cos\gamma),\,\text{for}\,\, r<r^\prime \\
   \sum\limits_{\ell=1}^\infty
   \frac{-(2\ell+1)}{r^{\prime 2}\varepsilon(r^\prime)}
   \color{Orange}{\frac{u_\ell(r^\prime)v_\ell(r)}{W_\ell(r ^\prime)}}
    P_\ell(\cos\gamma),\,\text{for}\,\, r>r^\prime
  \end{cases}
\end{align}
$$
`

???

- The Green's function for a given differential operator is the solution to the
  differential equation with a point source. It's also called the _fundamental
  solution_
- We also have azimuthal symmetry, hence the summation over Legendre
  polynomials. This can be shown more rigorously starting from the summation
  over spherical harmonics.
- The radial equation is similar to the usual one where spherical boundary
  conditions are imposed. The difference is the additional first order term due
  to the weighted dielectric distribution, which can be thought to act as a damping
  term. The source term is also "damped".
- Given that the permittivity profile is sigmoidal, we impose that the
  solutions to the homogeneous equation are asymptotically equal to the ones
  for spherical boundary conditions.
- The actual solutions are fixed by integrating the equation on a vanishingly
  small volume around the source discontinuity
- We have to remove an unphysical monopole contribution
- on the x axis is the distance from the interface center
- `\(w = \frac{\tilde{w}}{
  6}\)` to keep consistency with Frediani et al.
- `\(\Sigma\)` is a sigmoidal function, centered at `\(c\)`. The `\(w\)` parameter
is the HWHM for the associated distribution function, i.e. its first derivative
- Despite the fact that the variation in permittivity happens around `\(c\)`, the onset
of the dielectric effect will be delayed slightly. This can be rationalized in
terms of the Onsager factor.
- The figure assumes `\(c = 10~\AA\)` and the droplet centered at the origin

---
layout: false

## Water-Vapor Transfer: Effect of Interface Width

<img src="images/acetone-par-water_vapor.jpg" style="float: left; width: 45%; margin-right: 1%; margin-bottom: 0.5em;">
<img src="images/acetone-perp-water_vapor.jpg" style="float: left; width: 45%; margin-right: 1%; margin-bottom: 0.5em;">
<p style="clear: both;">
<img src="images/acetone_tangent.jpg" style="float: left; width: 45%; margin-right: 1%; margin-bottom: 0.5em;">
<img src="images/acetone_normal.jpg" style="float: left; width: 45%; margin-right: 1%; margin-bottom: 0.5em;">
<p style="clear: both;">

???

- on the x axis is the distance from the interface center
- Acetone with dipole moment tangent or normal to the droplet surface
- Transport from a water droplet to vapour
- `\(R_0 = 20~\AA\)`
- Figures report user-defined values of `\(\tilde{
  w}\)`
- When the dipole moment is normal to the droplet, different parts of the
  molecule experience different dielectric environments. Of course, the
  interface size has to be quite small!
- Wider profile, smoother transition
- Interface effect delayed (Onsager factor)
- Orientation matters


---
layout: false
class: split-50-50

## Effect of Interface Curvature

`$$\Delta G_{R_0}(z) = G_{R_0}(z+R_0) - G_{R_0=80} (z+80)$$`

### Interface effect determined by \\(G_\mathrm{img}\\)

.column[
- \\(G_\mathrm{img}\\) **medium-long range**
- interface effect **before** crossing
]

.column[
- \\(G_\mathrm{img}\\) **short range**
- interface effect **after** crossing
]

<p style="text-align:center;"><img src="images/Li-curvature.png" style="width: 100%"></p>
<p style="clear: both;">


???

- These plots are for the lithium ion

- all interfaces have `\(\tilde{w} = 5.0~\AA\)`
- on the x axis is the distance from the droplet center
- We vary the droplet radius `\(R_0 = 5, 10, 20, 40, 80~\AA\)`
- Take `\(R_0 = 80~\AA\)` as reference. We're not taking the difference wrt the
  planar results as the two implementations are different and we don't want to
  introduce implementation bias.
- First picture: lithium ion, water to vapor transfer
  * The high dielectric constant of water makes the image potential negligible
    inside the droplet and dominant outside.
  * \\(G_\mathrm{img}\\) **medium-long range**
  * interface effect **before** crossing

- Second picture: lithium ion, oil (`\(\varepsilon = 2\)`) to vapor transfer
  * For the oil droplet the converse happens.
  * \\(G_\mathrm{img}\\) **short range**
  * interface effect **after** crossing

---
layout: false

## Using Wavelets for the PCM

`
$$
\mathbf{A}\mathbf{f} = \mathbf{g}
$$
`

**Collocation BEM**
`$$
A_{ij} = \int_{\Pi_j} k_A(\mathbf{s}_i, \mathbf{s}^\prime)\mathop{}\!\mathrm{d}\mathbf{s}^\prime,
\quad g_i = g(\mathbf{s}_i)
$$`
 - Parametrized diagonal elements
 - Dense linear systems
 - Symmetry-breaking
 - Convergence not ensured

**Galerkin BEM**
`$$
A_{ij} = \int_{\Pi_i}\left(\int_{\Pi_j} k_A(\mathbf{s}, \mathbf{s}^\prime)\mathop{}\!\mathrm{d}\mathbf{s}^\prime\right)\mathop{}\!\mathrm{d}\mathbf{s},
\quad g_i = \int_{\Pi_i}g(\mathbf{s})\mathop{}\!\mathrm{d}\mathbf{s}
$$`
 - Sparse linear systems
 - Symmetry-preserving
 - Convergence ensured

???

---
layout: false
class: split-60-40

<p style="text-align:center;"><img src="images/paper-ii.png" style="width: 100%"></p>
<p style="clear: both;">

.column[
<p style="text-align:center;"><img src="images/finger-structure.png" style="width: 60%"></p>
<p style="clear: both;">
]
.column[
**_Wavelet_ Galerkin BEM**
- _Linear_ space requirements
- _Linear_ time requirements
- _Guaranteed_ accuracy
]

???

---
layout: false

## Wavelets for QM/Continuum: Accuracy and Cost

<p style="text-align:center;"><img src="images/alpha_convergence.png" style="width: 90%"></p>
<p style="clear: both;">

???

- Convergence pattern
- Stricter error control
- Larger number of surface points

---
layout: false

## Arbitrary Order Molecular Response Properties in Solution

<div class="imageWrapper">
  <img class="overlayImage" src="images/pcm_openrsp.png" style="width: 90%" align="middle">
--
  <img class="overlayImage" src="images/paper-v.png" style="width: 110%" align="left">
</div>

---
layout: false

## Exploiting Our Variational Advantages

`
$$
 \tilde{L}^a(\tilde{\mathbf{D}}, \color{Red}{\tilde{\mathrm{p}}}, t)
 \overset{\lbrace\text{Tr}\rbrace_T}{=}
 \tilde{\mathcal{G}}^{00,a} - \tilde{\mathbf{S}}^a\tilde{\mathbf{W}}
$$
`

**Fixed cavity approximation**
`
$$
\tilde{\mathcal{G}}^{00,a}(\tilde{\mathbf{D}}, \color{Red}{\tilde{\mathrm{p}}}, t)
\overset{\text{Tr}}{=}
(\tilde{\mathbf{h}}^a + \tilde{\mathbf{V}}^{t,a} +
\frac{1}{2}\tilde{\mathbf{G}}^{\gamma, a}(\tilde{\mathbf{D}}) +
\tilde{\mathbf{F}}^{\Omega_a}_\mathrm{xc}
-\frac{\mathrm{i}}{2}\tilde{\mathbf{T}}^a)\tilde{\mathbf{D}} + h_\mathrm{nuc}^a
+\color{Red}{\tilde{\mathrm{p}}}\color{Blue}{\tilde{\mathbf{\mathrm{s}}}^a}\tilde{\mathbf{D}}
$$
`

**Response functions**
`
$$
\langle\langle A; B \rangle\rangle_{\omega_b} =
    L^{ab}
 \overset{\lbrace\text{Tr}\rbrace_T}{=}
\mathcal{G}^{00,ab}
+\mathcal{G}^{10,a}\mathbf{D}^{b}
+\mathcal{G}^{01,a}\color{Red}{\mathrm{p}^{b}}
-\mathbf{S}^{ab}\mathbf{W} - \mathbf{S}^{a}\mathbf{W}^{b}
$$
`

**PCM response equation**
`
$$
\begin{alignat*}{2}
  \mathcal{R}^{-1}\mathcal{T}\color{SkyBlue}{\mathrm{p}_\mathrm{H}^{b_N}}
  +\text{Tr}\mathbf{\varphi}\color{SkyBlue}{\mathbf{D}_\mathrm{H}^{b_N}} = 0
  \quad&\quad
  \mathcal{R}^{-1}\mathcal{T}\color{Orange}{\mathrm{p}_\mathrm{P}^{b_N}}
  +\text{Tr}\mathbf{\varphi}\color{Orange}{\mathbf{D}_\mathrm{P}^{b_N}} =
  \color{Orange}{\mathbb{S}^{(n-1)}_\omega}
\end{alignat*}
$$
`

**\\(N\\)-the order response equations**
`
$$
\bbox[SkyBlue, 5pt]{\left[\mathbf{E}^{[2]} - \omega_{b_N}\mathbf{S}^{[2]} \right]
\mathbf{X}^{b_N}}
=\bbox[Orange, 5pt]{\mathbf{M}_\mathrm{RHS}^{b_N}}
$$
`

---
layout: false
class: split-50-50

## Multiphoton Absorption in Solution

.column[
<p style="text-align:center;"><img src="images/PDNB_energy.png" style="width: 100%"></p>
<p style="clear: both;">

- Nonequilibrium solvation
- Odd-order MPA
- Even-order MPA
]
.column[
<p style="text-align:center;"><img src="images/PDNB_MPA.png" style="width: 50%"></p>
<p style="clear: both;">
]

---
layout: false
class: center, middle, inverse

# Finale

---
layout: false

## Conclusions and Perspectives

.left-column[
### Variational Formulation
]
.right-column[
**Convenient theoretical foundation**
- Transparent derivations
- Framework for quantum/classical polarizable models
]

---
layout: false

## Conclusions and Perspectives

.left-column[
### Variational Formulation
### PCMSolver
]
.right-column[
**Convenient theoretical foundation**
- Transparent derivations
- Framework for quantum/classical polarizable models

**Extensible, modular, sustainable development**
- Develop once, reuse anywhere
- _Free_, _open-source_, _facile_ access to new PCM features
- **Unique package** in current QM software ecosystem
]

???

- PCMSolver is a unique package in the current landscape of QM-related
  libraries

---
layout: false
class: split-50-50

## Acknowledgements

<p style="text-align:center;"><img src="images/acknowledgements.gif" style="width: 70%"></p>
<p style="clear: both;">

.column[
__Benedetta Mennucci__ .cite[Università di Pisa]

__Radovan Bast__ .cite[University of Tromsø]

__Glenn Morello__ .cite[CTCC, University of Tromsø]

__Michal Repisky__ .cite[CTCC, University of Tromsø]
]
.column[
__Luca Frediani__ .cite[CTCC, University of Tromsø]

__Filippo Lipparini__ .cite[Universität Mainz]

__T. Daniel Crawford__ .cite[Virginia Tech]

__Lori A. Burns__ .cite[Georgia Tech]
]

---
name: last-page
template: inverse

# Thanks for your attention!

<p style="text-align:center;"><img src="images/shoot.gif" style="width: 50%"></p>
<p style="clear: both;">

Slideshow created using [remark] and served using [cicero]

Slides available on [GitHub](https://github.com/robertodr/phd-forsvaring)

Browse slides at [http://tinyurl.com/phd-forsvaring](http://tinyurl.com/phd-forsvaring)

[remark]: https://github.com/gnab/remark
[cicero]: https://github.com/bast/cicero
