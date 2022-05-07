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

### Group definition 

A group is a mathematical object similar to a field. It is basically a
set, $$G$$, equipped with a single operator (unlike a filed that has two
distinct operators) 

$$*:<G,G>\longrightarrow G$$

with the following
properties:

1. **Associativity:** \* is associative.

2. **Identity elemen:t** A group has to have an identity element usually marked $$e$$ such that
$$\forall g\in G\ g*e=g$$

3. **Inverse element:** $$\forall g\in G\ \exists h\in G\ g*h=e$$

### Subgroups

We say that $$H$$ sub group of $$G$$ if 

$$H\subseteq G$$ and $$(H,*)$$ is a group
in itself

### The $$\mathbb{Z}_{n}^{*}$$ group

A very well studied family of groups we'll be using is the Z-star group.
For any natural number $$n$$ we can define the group

$$\mathbb{Z}_{n}^{*}=\left\{ k|1\leq k\leq n-1,GCD(k,n)=1\right\}$$.

i.e all the numbers from 1 to n-1 that are mutually prime with n (have no
common divisor but 1).

The group operator $$*$$ is a modulo $$n$$ multiplication i.e $$a*b=[a*b]_{n}$$

$$[x]_{n}$$ is the way I'll use to mark $$(x\text{ mod }n)$$ which is the
remainder of dividing x by n

Have a moment to think why $$\mathbb{Z}_{n}^{*}$$ is indeed a group.

For example $$\mathbb{Z}_{15}^{*}=\{1,2,4,7,8,11,13,14\}$$

Note that for any prime $$p$$ we have

$$\mathbb{Z}_{p}^{*}=\{1,2,3....p-1\}$$

namely $$|\mathbb{Z}_{n}^{*}|=p-1$$

### <u>lemma 1:</u> 
If $$H\subseteq G$$ is a subgroup then $$|H|$$ divides $$|G|$$

### <u>lemma 2:</u>
 $$\forall g\in G\ g^{|G|}=e$$  

### <u>lemma 3:</u>
 $$[ab]_{c}=[[a]_{c}[b]_{c}]_{c}$$

### <u>Fermma's little theorem:</u>

For any prime $$p$$ and a natural number $$a$$:

$$GCD(p,a)=1\Longrightarrow[a^{p-1}]_{p}=1$$

This is a direct derivation from lemma-1, the fact that
$$|\mathbb{Z}_{n}^{*}|=p-1$$ and lemma-3

Miller Rabin polynomial time prime validation
=============================================

The most straight-forward algorithm for verifying if an integer $$n$$ is
prime will be to go over all the number between $$2$$ and $$\sqrt{n}$$ and
check whether they divide $$n$$.

This takes $$O(\sqrt{n})$$ operations and as we receive the input $$(n)$$ in digital representation which is $$log(n)$$ long we end up with an exponential time algorithm.

M&R suggested a probabilistic algorithm that given an integer $$n$$ and a precision value $$\epsilon$$ returns the correct answer for $$n's$$ primness with probability bigger then $$1-\epsilon$$.

### M&R Algorithm
0. >Inputs: $$n,\epsilon$$

1. >  Find an integer $$r$$ such that $$\frac{1}{2^{r}}<\epsilon$$

2. >  Repeat $$r$$ times:

3. >>  Choose a random number, $$k$$, between $$2$$ and $$n-1$$ (no repetitions)

4. >>  If $$[k^{n-1}]_{n}\neq1$$ return "not-Prime"

5. > return "Prime"

###  Proof of correctness:

#### $$n$$ is a prime 
$$\forall_{1\leq k\leq n-1}$$ $$GCD(p,k)=1$$ 

(no smaller number divides it) and from little ferma's theorem we have

$$[k^{n-1}]_{n}=1$$ 

so the condition (step 4.) in each iteration will be false and the algorithm will finally return "Prime" w.p 1.

#### $$n$$ is not a prime
In this case we'll show that (for most numbers) the probability of choosing 

$$2\leq k\leq n-1$$ 

that admits

$$[k^{n-1}]_{n}=1$$

is smaller then  $$\frac{1}{2}$$  and the probability of choosing such a number $$r$$ times in a row and returning "Prime" is smaller then $$\frac{1}{2^{r}}$$.

Lets look at the set

$$A_{n}=\{k|1\leq k\leq n-1,[k^{n-1}]_{n}=1\}$$. 

Appendix-1 shows that when equipped with the (mod n) multiplication operator this is a subgroup of $$\mathbb{Z}_{n}^{*}$$.

If so we can use Lemma-1 to say that 

$$|A_{n}|$$ divides $$|\mathbb{Z}_{n}^{*}|$$ 

i.e that  $$|\mathbb{Z}_{n}^{*}|=k_{n}|A_{n}|$$  for $$k\in\mathbb{N}$$. 

If we ignore for a moment numbers $$n$$ for which $$k_{n}=1$$ we can say that

$$|A_{n}|\leq\frac{|\mathbb{Z}_{n}^{*}|}{2}$$ 

and since $$|\mathbb{Z}_{n}^{*}|\leq n-1$$ we have 

$$|A_{n}|\leq\frac{n-1}{2}$$ 

so when sampling from $$U(2,n-1)$$ the probability of chosing a member of
$$|A_{n}|$$ is smaller then $$\frac{1}{2}$$ as desired.

We are left with dealing with those numbers $$n$$ for which $$k_n=1$$  i.e
$$|A_{n}|=|\mathbb{Z}_{n}^{*}|$$.

So what we'll do is be sloppy and just not deal with them.
Apparently these numbers are very rare and have their own name, [Carmichael numbers] (https://en.wikipedia.org/wiki/Carmichael_number). Although there are infinitely many of them there are only 2,163 are less than 25,000,000,000. For these numbers the algorithm wont work.

RSA
===

### Public-ky cryptosystems 

RSA is an implementation of a public-key cryptosystem. Basically it means that every member of a communication channel have a public
**encryption** key and a private **decryption** key. Every member of the channel can encrypt a message with some public key so that it could be decrypted only using the matching private key which is supposed to kept in secret.

Each key defines an encryption/decryption function:

$$E_k:M\longrightarrow M,$$

$$D_k:M\longrightarrow M.$$

where $k$ is a key, $M$ is the messages space and 

$$D_{k_d}(E_{k_e}(m))=m$$.

### RSA Scheme

In RSA both keys are pairs of numbers

$$k_{e}=(n,e),k_{d}=(n,d)$$ 
and both encryption and decryption is done in the same way:
$$E((n,e),m)=[m^{e}]_{n} \ \ E((n,d),m)=[m^{d}]_{n}$$ 
(Remember we are communicating strings of bits which can be looked at as numbers). 

The idea is to find such numbers, $$n,e,d$$ such that the decryption is correct, i.e 

$$[([m^{e}]_{n})^{d}]_{n}=m$$

and that the private key $$d$$ can not be reveled by knowing the public key $$(n,e).$$

The way to generate such numbers for communicating messages of $$b$$ bits is the following:

1. Choose two prime numbers $$p,q$$ which are  
 $$\frac{b}{2}$$ bits long and set $$n=p*q.$$

2. Set $$e$$ to be a small number such that $$GCD((p-1)(q-1),e)=1$$ (Just iterate over numbers until you get such one).

3. Set $$d$$ to be the inverse of $$e$$ in $$\mathbb{Z^{*}}_{(p-1)(q-1)}$$. 
    i.e $$[e*d]_{(p-1)(q-1)}=1$$

### Scheme corrrectness

Lets show that keys generated this way really form a public key cryptosystem. First, from
lemma-3 

$$[([m^{e}]_{n})^{d}]_{n}=[m^{ed}]_{n}$$ 

In order to show it equals $$m$$ it's enough to show that 

$$[m^{ed}]_{q}=m \land [m^{ed}]_{p}=m$$

because if both $$p,q$$ divide $$m^{ed}-m$$ then their product,$$n$$ does too. That is  $$[m^{ed}-m]_{n}=0$$ or $$[m^{ed}]_{n}=m$$ 

We will show $$[m^{ed}]_{p}=m$$ and the proof is identical for $$q.$$

We split into cases: Since $$p$$ is prime either $$GCD(p,m)=p$$ or $$GCD(p,m)=1$$

1.  If $$GCD(p,m)=p$$ then $$[m]_{p}=0:$$ so $$m$$ and $$0$$ are identical modulo $$p$$ and
    $$[m^{ed}]_{p}=[[m]_{p}^{ed}]_{p}=0=[m]_{p}.$$ i.e
    $m^{de}=m(\text{mod p)}$

2.  If $$GCD(p,m)=1$$ Fermma's lemma tells us that $$[m^{p-1}]_{p}=1$$ and since we chose $$d,e$$ such that $$[e*d]_{(p-1)(q-1)}=1$$ we have

> $$ed=k(p-1)(q-1)+1$$ for some $$k\in\mathbb{N}$$ 

> and $$m^{de}=m^{k(p-1)(q-1)+1}=m(m^{(p-1)})^{k(q-1)}=m1^{k(q-1)}=m$$

### Scheme secrecy

How is the secret key protected? Given that we know the public key $$e$$
requires knowing p,q in order to find its inverse, $$d$$, in
$$\mathbb{Z^{*}}_{(p-1)(q-1)}$$. Since $$p,q$$ are primes they form $$n's$$
[prime factorization](https://en.wikipedia.org/wiki/Integer_factorization) which is a task for which there is no known polynomial time algorithm (in length of the digital input $$log(n)$$). So if we choose sufficiently large $$p$$ and $$q$$ we ensure no one could reveal $$d$$ from $$n,e$$ in a reasonable time.

### Appendix 1: $$A_{n}\subseteq\mathbb{Z}_{n}^{*}$$
We need to show that the the set $$A_n$$ is contained in $$\mathbb{Z}_{n}^{*}$$ and that $$A_n$$ is closed under $$(mod\ n)$$ multiplication.
1. For $$a{\in A_n}$$ by definition $$[a^{n-1}]_n=1$$ so $$a^{n-1}=kn+1$$ or $$a^{n-1}+kn=1$$  for some $$k{\in}\mathbb{N}$$.
Suppose by contradiction that $$GCD(n,a)=d>1$$. This means that d divides 1 since it surely divides $$a^{n-1}+kn$$. So it must be that $$GCD(n,a)=1$$ i.e $$a{\in}\mathbb{Z}_{n}^{*}$$
2. For $$a,b{\in}A_n$$ we have 

$$[(ab)^{p-1}]_n=[a^{p-1}b^{p-1}]_n=[a^{p-1}]_n[b^{p-1}]_n=1*1=1$$

Therefore $$ab{\in}A_n$$