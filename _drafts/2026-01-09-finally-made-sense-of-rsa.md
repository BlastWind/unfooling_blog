---
layout: post
title: Bro I finally made RSA make sense to me
featured: true
date: "2026-01-09 12:30:00"
tags: cryptography
---

RSA is an asymmetric (uses a public/private key pair) cryptographic algorithm used for secure data transmission and digital signatures. With RSA, you can communicate your messages already. In contrast, Diffie-Hellman just establishes a shared key, but you can then use it to run symmetric communication protocols.

Start with a large number $N$. It is computationally infeasible to calculate a large $N$'s totient, $\phi(N)$.
The totient of $N$ counts the number of integers between $1$ and $N$ that are coprime to $N$.
This is computationally infeasible because the algorithm is at best linear. Therefore, $\phi(1000000)$ 
takes a thousand $\phi(1000)$-times to compute.

But if $N$ happened to be a product of two prime numbers $p, q$, then you can immeidately get $\phi(N) = (p-1) * (q-1)$. That is, if you can factor $N$, you get its totient for free. But, factoring $N$ is super hard! There is no 
known polynomial-in-$log_2 (N)$-time algorithm prime factorization algorithm. Emphasis on $log_2 (N)$!
There is of course a polynomial-in-raw-magnitude-of-$N$-time algorithm. But no polynomial-in-number-of-required-bits 
algorithm.

We have obtained our first tool: A large $N$'s totient, $\phi(N)$, is immediate to compute if we know its factors and impossible if not.

We now publish the public key $e$ and $N$, the product of two secretly chosen two primes $p, q$. **The private key $d$ will now be generated from the public key $e$ !**
$$
d \equiv e^{-1} \bmod \phi(N)
$$

The owner of $p, q$ can calculate this quickly because $\phi(N)$ is immediate. We have applied our first tool: Using the nature of the $\phi(N)$, owners of its prime factors can easily calculate and own the private key $d$ and those who don't can't (quantum aside).

This is the core of RSA. Now, let's apply RSA.

### Encrypting/Decrypting with RSA
Given
$$
d \equiv e^{-1} \bmod \phi(N)
$$
observe that
$$
ed \equiv 1 \bmod \phi(N)
$$

Further observe that
$$
m^{ed} \equiv m^{1 \bmod\phi(N)} \equiv m
$$

which means
$$
(m^e)^d \equiv m 
$$

There it is. You can now define encryption as taking the $e$-th exponent ($e$ is public) and decryption as taking the $d$-th exponent ($d$ is private).

Now, take your message $m$. The encryption operation is the operation of raising $m$ to the $ed$-th power, $m^{ed}$. 
Then, $m^{ed} = m^{1 \bmod \phi(N)} = m^{1 + k \pi}... TODO: Hmm, need Chiense remainder theorem and euler's.

### Signing/Verifying with RSA
raising to d first and then e actually here!
Message is public! If someone is able to raise the message such that a subsequent raising to e gets back message,
you win and cryptographically convince others you own the private key.

This is also because 