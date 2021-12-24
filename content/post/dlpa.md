+++
author = "Jeffrey Yang"
title = "Dynamic Logic of Propositional Assignments (DL-PA)"
date = "2021-12-24"
description = "A brief introduction to DL-PA."
tags = [
    "dl-pa"
]
math = true
+++

The purpose of this post is to provide some necessary context to https://github.com/j3yang/dl-pa. We shall briefly discuss model checking and then introduce the grammar of DL-PA.

## Model Checking
**Model checking** seeks to answer the question of whether a system satisfies some given properties. Thus, model checking plays an essential role in software verification and is an active area of research.

Within the context of DL-PA, model checking is the process of verifying whether a DL-PA formula is satisfied by some given **valuation** (an assignment of variables to true or false).

## The Grammar of DL-PA
### Formulas
Formulas \\( \varphi \\) in DL-PA are defined by the grammar 
$$
\mathcal{L}_{\text{fml}}:\varphi \quad ::= \quad p \\; | \\; \top \\; | \\; \neg\varphi \\; | \\; \varphi\lor\varphi \\; | \\; \langle\pi\rangle\varphi
$$
where \\( p \\) ranges over the set of propositional variables \\( \mathbb{P} \\). **False** (\\( \bot \\)) and **conjunctions** (\\( \varphi \land \varphi \\)) are not part of the definition but are abbreviations for \\( \neg \top \\) and \\( \neg(\neg \varphi \lor \neg \varphi) \\) respectively.

Given a formula \\( \varphi \\) and valuation \\( M \\), we write \\( M \models \varphi \\) and say that \\( M \\) **models** \\( \varphi \\) if the valuation yields true for the formula. For instance, let \\( M_1 = \\{ a, b\\} \\), \\( M_2 = \\{ b, c\\} \\) and \\( \varphi = a \lor \neg b \\). Then we have
\\( M_1 \models \varphi \\) and \\( M_2 \not\models \varphi \\).

### Programs
Programs \\( \pi \\) in DL-PA are defined by the grammar
$$
\mathcal{L}_{\text{pgm}}:\pi \quad ::= \quad p\gets \varphi \\; | \\; \varphi? \\; | \\; \pi;\pi \\; | \\; \pi\cup\pi \\; | \\; \pi^*.
$$
The program \\( p \gets \varphi \\) is the **assignment** of \\( \varphi \\) to \\( p \\).

The program \\( \varphi? \\) is the **test** of whether \\( \varphi \\) is true. It fails if \\( \varphi \\) is false.

\\( \pi;\pi' \\) is the **sequential composition** of programs \\( \pi \\) and \\( \pi' \\).

\\( \pi \cup \pi' \\) is the **non-deterministic composition** of programs \\( \pi \\) and \\( \pi' \\).

\\( \pi^* \\) is the **finite iteration** of \\( \pi \\). Here, \\( ^* \\) is commonly referred to as the **Kleene star**.

Now, given a program \\( \pi \\) and a pair of valuations \\( (M,M') \\), we write \\( (M,M')\models \pi \\) and say \\( (M,M') \\) **models** \\( \pi \\) if \\( \pi \\) transforms \\( M \\) into \\( M' \\). For example, let \\( N = \left(\\{a\\}, \\{c\\}\right) \\) and \\( \pi = a \gets \bot; c\gets \top \\). Then \\( N \\) models \\( \pi \\).
### Diamonds
The meaning of \\( \langle \pi \rangle \varphi \\) is that after performing \\( \pi \\), it is **possible** that \\( \varphi \\) holds. On the other hand, \\( [ \pi ] \varphi := \neg\langle\pi\rangle\neg\varphi \\) means that after performing \\( \pi \\), it is **necessarily** the case that \\( \varphi \\) holds. 

We write \\( M \models \langle \pi \rangle \varphi \\) if there **exists** a successful execution of \\( \pi \\) on \\( M \\) that leads to some \\( M' \\) that models \\( \varphi \\).

We write \\( M \models [\pi]\varphi \\) if **for all** successful executions of \\( \pi \\) on \\( M \\), the output \\( M' \models \varphi \\).

