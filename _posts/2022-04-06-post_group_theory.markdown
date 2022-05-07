---
layout: post
comments: true
usemathjax: true
title: "On prime numbers and their usages"
excerpt: "Two algorithms that uses prime numbers and group theory"
date: 2022-05-7
mathjax: true
---

In this blogpost I review two algorithms that deal with prime numbers.

The first is Miller&Rabin's algorithm for prime number validation in
polynomial time.

The second one is the RSA cryptosystem. It is one of the few things I know about cryptography and apparenlty is the basis of many encriptions
we use everyday.

Both algorithms make use of some basic concepts from group theory which
we'll go through briefly here.

These two are of the very few usages I know of for group theory which is
one of the reasons for writing this blog


Group theory basics
===================

#### Group definition {#group-definition .unnumbered}

A group is a mathematical object similar to a field. It is basicaly a
set, G, equiped with a single operator (unlike a filed that has two
distint operators) **$*:<G,G>\longrightarrow G$** with the following
propeties:

\- \* is associative.

\- A group has to have an identithy element usualy marked $e$ such that
$\forall g\in G\ g*e=g$

\-$\forall g\in G\ \exists h\in G\ g*h=e$

#### A subgroup  {#a-subgroup .unnumbered}

We say that $H$ sub group of $G$if $H\subseteq G$ and $(H,*)$ is a group
in itself

#### The $\mathbb{Z}_{n}^{*}$ group  {#the-mathbbz_n-group .unnumbered}

A very well studied family of groups we'll be using is the Z-star group.
For any natural number n we can define the group
$\mathbb{Z}_{n}^{*}=\left\{ k|1\leq k\leq n-1,GCD(k,n)=1\right\}$. i.e
all the numbers from 1 to n-1 that are mutually prime with n (have no
common divisor but 1).

The group operator \* is a modolu $n$ multiplication i.e $a*b=[a*b]_{n}$

$[x]_{n}$is the way I'll use to mark $(x\text{ mod }n)$ which is the
remainder of dividing x by n

Have a moment to think why t this is indeed a group.

For example $\mathbb{Z}_{15}^{*}=\{1,2,4,7,8,11,13,14\}$

Note that for any prime $p$ we have
$\mathbb{Z}_{p}^{*}=\{1,2,3....p-1\}$ namely $|\mathbb{Z}_{n}^{*}|=p-1$

#### lemma 1: If $H\subseteq G$ then $|H|$ divides $|G|$ {#lemma-1-if-hsubseteq-g-then-h-divides-g .unnumbered}

#### lemma 2: $\forall g\in G\ g^{|G|}=e$  {#lemma-2-forall-gin-g-gge .unnumbered}

#### lemma 3: $[ab]_{c}=[[a]_{c}[b]_{c}]_{c}$ {#lemma-3-ab_ca_cb_c_c .unnumbered}

#### Fermma's little theorem {#fermmas-little-theorem .unnumbered}

For any prime $p$ and a natural number $a$:

$GCD(p,a)=1\Longrightarrow[a^{p-1}]_{p}=1$

This is a direct derivation from lemma-1, the fact that
$|\mathbb{Z}_{n}^{*}|=p-1$ and lemma-3

Miller Rabin polynomial time prime validation
=============================================

The most straigh-forward algorithm for verifying if an integer $n$ is
prime will be to go over all the number betwee $2$ and $\sqrt{n}$ and
check wheher they divide $n$.

This takes $O(\sqrt{n})$ operations and as we recieve the input ($n)$ in
a digital representation which is $log(n)$ long we end up with an
exponential time algorithm.

M&R suggested a probabilisic algorithm that given an integer $n$ and a
percision value $\epsilon$ returns the wrong answer for $n's$ primeness
with probability $<\epsilon$.

Here is the algorithm

Inputs $n,\epsilon$

Find an integer $r$such that $\frac{1}{2^{r}}<\epsilon$

Repeat $r$ times:

1\. choose a random number, $k,$ between $2$ and $n-1$(no repetitions)

2\. if $[k^{n-1}]_{n}\neq1$ return "not-Prime"

return "Prime"

#### Proof: {#proof .unnumbered}

If $n$is a prime then $\forall_{1\leq k\leq n-1}$ $GCD(p,k)=1$ (no
smaller number divides it) and so from little ferma's theorem we have
$[k^{n-1}]_{n}=1$ and the condition in each iteration will be false and
the algorithm will finaly return "Prime" w.p 1.

In the case $n$ is not a prime we'll show that (for most numbers) the
probability of choosing $2\leq k\leq n-1$ that addmits $[k^{n-1}]_{n}=1$
is smaller then $\frac{1}{2}$ and so the probability of choosing such a
number $r$ times and returning "Prime" is $\leq\frac{1}{2^{r}}$.

Lets look at the set $A_{n}=\{k|1\leq k\leq n-1,[k^{n-1}]_{n}=1\}$. We
will soon show show that when equiped with the (mod n) multiplication
operator this is a subgroup of $\mathbb{Z}_{n}^{*}$. If so we can use
Lemma-1 to say that $|A_{n}|$divides $|\mathbb{Z}_{n}^{*}|$ i.e that
$|\mathbb{Z}_{n}^{*}|=k_{n}|A_{n}|$ for $k\in\mathbb{N}$. If we ignore
for a moment numbers $n$ for which $k_{n}=1$ then we can say that
$|A_{n}|\leq$$\frac{|\mathbb{Z}_{n}^{*}|}{2}$ and since
$|\mathbb{Z}_{n}^{*}|\leq n-1$ we have $|A_{n}|\leq\frac{n-1}{2}$ so
when sampling from $U(2,n-1)$ the probability of chosing a member of
$|A_{n}|$ is smaller then $\frac{1}{2}$as desired.

We are left with dealing with those numbers $n$for which $k=1$ i.3
$|A_{n}|=|\mathbb{Z}_{n}^{*}|$.

So what we'll do is be sloppy and just not deal with them. Apparently
these numbers are very rare and have their own name, Carmichael numbers.

RSA
===

#### Public-ky cryptosystems {#public-ky-cryptosystems .unnumbered}

RSA is an implementationof a public-key cryptosystem. Basicly it means
that every member of a communication channel have a public
**encription** key and a private **decription** key. In such case every
one can encript a message with Eve's public's key so that only here will
be able to decript it.

Each key defines an encription/decription function
$D:KxM\longrightarrow M,$ $E:KxM\longrightarrow M.$ where $K$ is the
keys space and $M$ is the messages space and $D(k_{d},E(k_{e},m))=m$.

#### RSA {#rsa-1 .unnumbered}

In RSA both keys are pairs of numbers $k_{e}=(n,e),k_{d}=(n,d)$ and both
encription and decription is done in the same way:
$E((n,e),m)=[m^{e}]_{n}$ and $E((n,d),m)=[m^{d}]_{n}$(Remember we are
communicating strings of bits which can be looked at as numbers). The
idea is to find such numbers, $n,e,d$ such that the decription is
correct, i.e $[([m^{e}]_{n})^{d}]_{n}=m$ and that the private key $d$
can not be reveled by knowing the public key $(n,e).$

The way to generate such numbers for communicating messages of $b$bits
is the following:

1.  Choose two prime numbers $p,q$ with \~$\left[\frac{b}{2}\right]$
    bits and set $n=p*q.$

2.  Set $e$ to be a small number such that $GCD((p-1)(q-1),e)=1$

3.  Set $d$ to be the inverse of $e$ in $\mathbb{Z^{*}}_{(p-1)(q-1)}$.
    i.e $[e*d]_{(p-1)(q-1)}=1$

Lets show the correctness of the keys generated this way. First from
lemma 3 $[([m^{e}]_{n})^{d}]_{n}=[m^{ed}]_{n}$and we want to show it
equals $m$. Its enough to show that $[m^{ed}]_{q}=m$ and
$[m^{ed}]_{p}=m$ because then $[m^{ed}-m]_{n}=0$ i.e $[m^{ed}]_{n}=m$

We will show $[m^{ed}]_{p}=m$ and the proof is identical for $q.$

We split into cases: Since $p$is prime it either devides $m$or
$GCD(p,m)=1$

1.  If $[m]_{p}=0:$ then $m$and 0 are identical mode $p$so
    $[m^{ed}]_{p}=[[m]_{p}^{ed}]_{p}=0=[m]_{p}.$ i.e
    $m^{de}=m(\text{mod p)}$

2.  $GCD(p,m)=1$ so from fermma's lemma $[m^{p-1}]_{p}=1$\
    Now remember we chose $d,e$ so that $[e*d]_{(p-1)(q-1)}=1$ so we can
    say\
    $ed=k(p-1)(q-1)+1$ for some $k\in\mathbb{N}$ and\
    $m^{de}=m^{k(p-1)(q-1)+1}=m(m^{(p-1)})^{k(q-1)}$$=m1^{k(q-1)}=m$

How is the secret key protected? Given that we know the public key $e$
requires knowing p,q in order to find its inverse in
$\mathbb{Z^{*}}_{(p-1)(q-1)}$. So this summs up to finding $p,q$when we
also know their product $n.$ Since $p,q$ are primes they form $n's$
prime factorization which is generally a hard to task to which no
polynomial time algorithm is known.
