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

$n_f + n_b = n_2$

$n_f + 2n_b = n_4$

We then have the first constraint in terms of free termini:

The number of free termini must be less or equal to ($n_1$ + 2).

*Proof:*

Consider the most extreme case, when all the cores are connected.

Before connection, we have 3$n_1$ free termini, when all the cores are free monomers.

After fully connected, we have ($n_1$ - 1) "connection nodes".

When each "connection node" is formed, we remove 2 termini, which yields:

3 $n_1$ - 2( $n_1$ - 1) = $n_1$ + 2

Note that the extender (E) does not contribute to new termini for the whole system, because E can only connect with linker (L).

For a open linker termini: |-L-

it has 1 free termini.

When a E block is connected: |-L--E-

it removes the one originally at L and add 1 termini contributed by E.

The net addition is 0 new free termini.

Therefore, $n_3$ does not need to be involved in the constraint above.


Here in order to simplify the problem, we do not consider the case of ring forming.

The reason is we assume the stable molecular structure requires ca 120&deg bond angle.

That means we need 6 cores to form a ring. (According to the preliminary solubility test results, this size already beyond the soluble limit and would be unlikely to appear in the MALDI spectrum.)


As for the "bonded" version of linker:

They can be regarded as the nodes between C and E, so

$n_b = n_1 + n_3 - 1$

Substitute it back to the expression of $n_2$:

$n_2 = n_f + n_b = n_f + n_1 + n_3 - 1$

In terms of the newly formed HBr, it is generated every time one a new connection is constructed,

which can also be expressed by the two states of L:

For each free L - it generates 1 HBr;

For each bonded L - it generates 2 HBr.

Therefore, we have

$n_4 = n_f + 2n_b = n_f + 2n_1 + 2n_3 - 2$


Now, once $n_f$, $n_1$, $n_3$ are known, we can solve $n_4$ and $n_2$.

Set the total molecular mass as M (this would be the signal appears on the spectrum).

$M = n_1 m_1 + (n_f + n_b)m_2 + n_3 m_3 - (n_f + 2n_b)m_4$

Rearrange the form and only keep $n_1$, $n_3$ and $n_f$

yield

$M = n_1 (m_1 + m_2 - 2m_4) + n_3 (m_2 + m_3 - 2m_4) + n_f (m_2 - m_4) - m_2 + 2m_4$

Now, replace all the coefficients and constants with $a_1$, $a_2$, $a_3$ and $a$, i.e., 

$a_1 = m_1 + m_2 - 2m_4$

$a_2 = m_2 + m_3 - 2m_4$

$a_3 = m_2 - m_4$

$a = M + m_2 - 2m_4$


Equation construction done.


## Now start


### Update molecualr mass of core, linker, extender, removed unit when the new bond is formed

```
mass = [482.01, 108.14, 279.92, 80.91]  # mw of core, linker, ext, removed unit (HBr in this case)
```

### Update the target peak to solve

The first argument in #find_solution# correspond to signal of the MALDI that is waiting to be solved.

The second argument k is the number of possible structures. For example, k = 3 will give three possible combinations.

```
idx, calc_mass, err = find_solution(563, k=3)
```

Press run all to execute all the cells.

## Results

The tool will return all the attempted mass based on the potential combination of structure.

The index in the following result are in the order or C, E, L

The first element in the calculated mass list is with smallest error from the target peak on the MALDI.

```
all attempted valid mass:
[  53.68   80.91  108.14  279.92  307.15  334.38  482.01  506.16  509.24
  533.39  536.47  560.62  563.7   708.25  735.48  762.71  789.94  934.49
  961.72  988.95 1016.18]
index: [[1 0 3]
 [0 2 2]
 [1 0 2]]
calculated mass: [563.7  560.62 536.47]
error to goal: [  0.7   -2.38 -26.53]
```

In the next cell result, it returns the structure corresponded to the calculated mass and error to goal list.

The first three elements in each row are number of [C, L, E] building block.

The first row is with the smallest error. 

```
[cores linkers exts sites]:
 [[1 3 0 3]
 [0 3 2 4]
 [1 2 0 2]]
```

