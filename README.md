# CEL_MALDI_tool (TO BE UPDATED)
Solve MALDI (mainly for CMP)

## Introduction

This tool was designed to help solving potential fragment combination of conjugated microporous polymers. Can be useful in oligomer component analysis.

## Before start

- This version only works for tri-branched (eq) core + dual-termini (eq) extender + dual-termini (eq) linker.
- All the reaction sites of core and extender are same, which can form bond with both end of the linker. For example, core has three -Br; extender has two -Br; linker has two -NH2.


## Principle

Given the system contains $n_1$ core (C), $n_2$ linker (L), $n_3$ extender (E).

- (C, 3, Br) - core with three equivalent termini -Br.
- (L, 2, NH2) - linker with two equivalent termini -NH2.
- (E, 2, Br) - extender with two equivalent termini -Br.


For each new bond (AC-N-AC) formation:

- $n_4$ HBr will form and leave the current system.

Linker can be classified into two groups:
- Free ($n_f$) |-L--: with one free termini, one bonded (can only happen at the termini of entire molecule).
- Bond ($n_b$) |-L-|: both ends bonded.

We will have:

$n_f + n_b = n_2\n$
$n_f + 2n_b = n_4$

TBC...


## How to use this tool?

```
import numpy as np
```

### Input molecualr mass of core, linker, extender, removed unit when the new bond is formed

```
mass = [482.01, 108.14, 279.92, 80.91]  # mw of core, linker, ext, removed unit (HBr in this case)
```

goal = peak on the spectrum to be explained







