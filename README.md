# jlFriCAS

[![Build and Test](https://github.com/gvanuxem/jlfricas/actions/workflows/linuxJulia_sbcl.yml/badge.svg)](https://github.com/gvanuxem/jlfricas/actions/workflows/linuxJulia_sbcl.yml)\
[![Build and Test](https://github.com/gvanuxem/jlfricas/actions/workflows/linuxJulia_ccl.yml/badge.svg)](https://github.com/gvanuxem/jlfricas/actions/workflows/linuxJulia_ccl.yml)\
[![Build and Test](https://github.com/gvanuxem/jlfricas/actions/workflows/macOSJulia_sbcl.yml/badge.svg)](https://github.com/gvanuxem/jlfricas/actions/workflows/macOSJulia_sbcl.yml)\
[![Build and Test](https://github.com/gvanuxem/jlfricas/actions/workflows/windowsJulia_sbcl.yml/badge.svg)](https://github.com/gvanuxem/jlfricas/actions/workflows/windowsJulia_sbcl.yml)

[FriCAS](https://fricas.github.io) is a general purpose computer algebra
system (CAS).

In this work-in-progress repository, wrappers to some [Julia](https://julialang.org)
specific operations are added to FriCAS. It supports SBCL or Clozure CL build but, as of now, only Julia 1.10.0 is
supported for a SBCL based FriCAS, see [Caveats](#caveats). It should not be considered production-ready.

## Building and Installing

For general installation instructions see INSTALL. For general documentation
consult <https://fricas.github.io>.

To build FriCAS with Julia support, a simple
<code>./configure --enable-julia</code> should do the trick. The required Julia packages are Suppressor, Nemo and SpecialFunctions. If you want to visualize your data, small support is provided using Plots and eventually LaTeXStrings Julia packages . 

The <code>julia</code> executable needs to be available in your PATH.
If you want to add Jupyter support with a SBCL based FriCAS, make sure [hunchentoot](https://edicl.github.io/hunchentoot/) is installed, and as of now on Clozure CL [queues](https://github.com/oconnore/queues) is also required (use installed [quicklisp](https://www.quicklisp.org/beta/) with `queues` at configure time if necessary, see the `quicklisp` documentation).

On a Debian like system you can add `hunchentoot` with <code>sudo apt install cl-hunchentoot</code>
and issue, for example,
<code>./configure --enable-gmp --enable-julia --enable-hunchentoot</code>.
To know which categories/domains/packages are added to FriCAS issue in the
FriCAS interpreter <code>)what things julia</code> and/or <code>)what things nemo</code>  or use HyperDoc:

```
(1) -> )what things julia
------------------------------- Categories --------------------------------
 JARBPR   JuliaArbitraryPrecision      JMATCAT  JuliaMatrixCategory
 JOBJECT  JuliaObject(only with SBCL)  JRING    JuliaRing
 JTYPE    JuliaType                    JVECCAT  JuliaVectorCategory
--------------------------------- Domains ---------------------------------
 JCF64    JuliaComplexF64              JCF64MAT JuliaComplexF64Matrix
 JCF64SMA JuliaComplexF64SquareMatrix  JCF64VEC JuliaComplexF64Vector
 JCFLOAT  JuliaComplexFloat (only with SBCL)
 JF64     JuliaFloat64
 JF64MAT  JuliaFloat64Matrix           JF64SMAT JuliaF64SquareMatrix
 JF64VEC  JuliaFloat64Vector           JFLOAT   JuliaFloat (only with SBCL)
 JI64     JuliaInt64                   JI64VEC  JuliaInt64Vector
 JMATRIX  JuliaMatrix (only with SBCL) JOBJECT- JuliaObject& (only with SBCL)
 JSTR     JuliaString                  JSYM     JuliaSymbol
 JVECTOR  JuliaVector (only with SBCL)
-------------------------------- Packages ---------------------------------
 JCF64MTF JuliaComplexF64MatrixTranscendentalFunctions
 JCFSF    JuliaComplexFloatSpecialFunctions
 JCLA     JuliaComplexLinearAlgebra    JDRAW    JuliaDrawFunctions
 JF64MTF  JuliaF64MatrixTranscendentalFunctions
 JF64SF   JuliaFloat64SpecialFunctions
 JF64SF2  JuliaFloat64SpecialFunctions2
 JF64VEC2 JuliaFloat64VectorFunctions2
 JFSF     JuliaFloatSpecialFunctions (only with SBCL)
 JFSF2    JuliaFloatSpecialFunctions2 (only with SBCL)
 JPLOT    JuliaPlotFunctions
 JRLA     JuliaRealLinearAlgebra       JUF      JuliaUtilityFunctions
 JVECTOR2 JuliaVectorFunctions2
--------------- System Commands for User Level: development ---------------
 
------------------------- System Command Synonyms -------------------------
```
Nemo Categories/Domains/Packages (JuliaObject) are only built using SBCL:
```
(1) -> )what things nemo
------------------------------- Categories --------------------------------
 NRING    NemoRing                     NTYPE    NemoType
--------------------------------- Domains ---------------------------------
 NAN      NemoAlgebraicNumber          NCB      NemoComplexBall
 NCF      NemoComplexField             NECF     NemoExactComplexField
 NFF      NemoFiniteField              NINT     NemoInteger
 NMP      NemoMultivariatePolynomial   NPF      NemoPrimeField
 NRAT     NemoRational                 NRB      NemoRealBall
 NRF      NemoRealField                NUP      NemoUnivariatePolynomial
 NZMOD    NemoIntegerMod
-------------------------------- Packages ---------------------------------
--------------- System Commands for User Level: development ---------------

System commands at this level matching patterns:
     julia

julia     juliad
------------------------- System Command Synonyms -------------------------

user-defined synonyms satisfying patterns:
     ju

 )ju ............................ )julia
 )jud ........................... )juliad

```

If you want to build and install locally the HTML documentation,
you need to install Sphinx. On a Debian like system, to add it, issue in a
terminal <code>sudo apt install python3 python3-pip && pip3 install -U Sphinx</code>.
After building FriCAS, and before the installation, issue in the terminal
<code>make htmldoc</code>.

## Description

The basic goal of [FriCAS](https://fricas.github.io) is to create a free
advanced world-class CAS. In 2007 [FriCAS](https://fricas.github.io)
forked from [Axiom](http://axiom-developer.org). Currently the
[FriCAS](https://fricas.github.io) algebra library is one of the largest
and most advanced free general purpose computer algebra systems \-- this
gives a good foundation to build on. Additionally, the
[FriCAS](https://fricas.github.io) algebra library is written in a high
level strongly typed language (Spad), which allows natural expression of
mathematical algorithms. This makes [FriCAS](https://fricas.github.io)
easier to understand and extend.

[FriCAS](https://fricas.github.io) uses lightweight development
methodology. Compared to [Axiom](http://axiom-developer.org),
[FriCAS](https://fricas.github.io) is significantly restructured \-- it
is more portable and fixed several defects.
[FriCAS](https://fricas.github.io) removed rather large unused parts
(without removing functionality).

## Goals

Current development goals:

-   continue structural improvements
-   add new mathematical algorithms
-   develop better user interface
-   develop improved Spad compiler
-   make it easier for external programs to interface with FriCAS
-   support for using external mathematical routines from Spad

## Caveats

Julia support with SBCL is erratic, depending on the Julia version used. The 1.10.0 version seems to have solved problems related to memory management interactions with SBCL, but with Julia 1.10.1 and 1.10.2 problems occur again. Note that with Julia 1.11.0-alpha2, FriCAS seems to work fine. More work needs to be done in this regards. So, if you use SBCL to build FriCAS, imperatively use a version of Julia that is known to be compatible.
