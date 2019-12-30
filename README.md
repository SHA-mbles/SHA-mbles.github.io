&nbsp;

We have computed the very first **chosen-prefix collision for SHA-1**. In a nutshell, this means a complete and practical break of the SHA-1 hash function, with actual very dangerous potential practical implications if you are still using this hash function. Check our paper **[here](TODO)** for more details. 
  
&nbsp;
&nbsp;


## Our Chosen-Prefix Collision Example

You can find here two PGP certification keys that lead to the same hash output through SHA-1:
- [alice.asc](https://raw.githubusercontent.com/SHA-mbles/SHA-mbles.github.io/master/alice.asc) 
- [bob.asc](https://raw.githubusercontent.com/SHA-mbles/SHA-mbles.github.io/master/bob.asc) 
   
   
&nbsp;
&nbsp;   
   

# Q&A

## What is a chosen-prefix collision ?

A classical collision for a hash function H is simply two messages M and M' that lead to the same hash output: H(M) = H(M'). Even though this security notion is fundamental in cryptography, exploiting a classical collision for attacks in practice is difficult.

A chosen-prefix collision is a more constrained (and much more difficult to obtain) type of collision, where two message prefixes P and P' are first given as challenge to the adversary, and his goal is then to compute two messages M and M' such that H(P \|\| M) = H(P' \|\| M'), where \|\| denotes concatenation.

With such an ability, the attacker can obtain a collision even though prefixes can be chosen arbitrarily (and thus potentially contain some meaningful information). This is particularly impactful when the hash function is used in a digital signature scheme, one of the most common usage of a hash function.

&nbsp; 
## Is SHA-1 really still used ? 

Unfortunately, SHA-1 is still present in a surprising number of security applications. It is supported in many secure channel protocols (TLS, SSH), and remains actually used for some fraction of the connections. It is also used for PGP key-certifications, and it is the foundation of GIT versioning system. There are probably a lot of less known or proprietary protocols that still use SHA-1, but this is more difficult to evaluate. 

&nbsp; 
##  What is affected ? 

Any usage where collision resistance is expected from SHA-1 is of course at high risk. We identified a few settings that are directly affected by chosen-prefix collisions:
- PGP keys can be forged if third parties generate SHA-1 key certifications
- X.509 certificates could be broken if some Certificate Authorities issue SHA-1 certificates with predictable serial numbers 

We note that classical collisions and chosen-prefix collisions do not threaten all usages of SHA-1. In particular,
HMAC-SHA-1 seems relatively safe, and preimage resistance (aka ability to invert the hash function) of SHA-1 remains unbroken as of today. Yet, as cryptographers we recommend to deprecate SHA-1 everywhere, even when there is no direct evidence that this weaknesses can be exploited. 

&nbsp; 
##  What about your PGP example ? 

We chose PGP/GnuPG Web of Trust as demonstration of our chosen-prefix collision attack against SHA-1. The Web of Trust is a trust model used for PGP that relies on users signing each other’s identity certificate, instead of using a central PKI. For compatibility reasons the legacy branch of GnuPG (version 1.4) still uses SHA-1 by default for key-certification signatures.

We were able to forge key-certification signatures using SHA-1 chosen-prefix collisions. More precisely, we created two PGP keys with different UserIDs, so that key B is a legitimate key for Bob (to be signed by the Web of Trust), but the signature can be transferred to key A which is a forged key with Alice’s ID. 

&nbsp; 
## What should I do ?

**Remove any use of SHA-1 in your product as soon as possible and use instead [SHA-256](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf) or [SHA-3](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.202.pdf)**. 

SHA-1 has been broken for 15 years, so there is no good reason to use this hash function in modern security software. Attacks only get better over time, and the goal of the cryptanalysis effort is to warn users so that they can deprecate algorithms before the attacks get practical. We actually expect our attack to cost just a couple thousand USD in a few years. 

&nbsp; 
## How much does the attack cost ?

By renting a GPU cluster online, the entire chosen-prefix collision attack on SHA-1 costed us about 75k USD. However, at the time of conputation, our implementation was not optimal and we lost some time (because research). Besides, computation prices went further down since then, so we estimate that our attack costs today about 45k USD. As computation costs continue to decrease rapidly, we evaluate that it should cost less than 10k USD to generate a chosen-prefix collision attack on SHA-1 by 2025.

As a side note, a classical collision for SHA-1 now costs just about 11k USD. 

&nbsp; 
## Wasn't there already a collision attack against SHA-1 ?

A classical collision has been computed for SHA-1 in late 2017, as you can see [here](https://shattered.io/). However, this is very different from a chosen-prefix collision, where any prefix pair can be challenged for the collision, which leads to a much more serious impact in practice.  

&nbsp; 
## Wasn't there already a chosen-prefix collision attack against SHA-1 ?

Last year, we announced a new chosen-prefix collision attack, as you can see [here](https://eprint.iacr.org/2019/459) ([some test code](https://github.com/Cryptosaurus/sha1-cp) is also available) and this work was published at the [Eurocrypt 2019](https://eurocrypt.iacr.org/2019/) conference. Here, we further improved these results up to a point where the attack becomes doable for a reasonable amount of money, and we wrote an actual implementation of the attack to compute the chosen-prefix collision against SHA-1.
  
&nbsp; 
## Can I try it out for myself ?
 
Since our attack on SHA-1 has pratical implications, in order to make sure proper countermeasures have been pushed we will wait for some time before releasing the entire source code that allows to generate SHA-1 chosen-prefix collisions.   

 
&nbsp;
&nbsp;   
 
- - -
# Contact

If you have any questions, feel free to contact us:  
  
**Gaëtan Leurent:** gaetan.leurent@inria.fr  
**Thomas Peyrin:** thomas.peyrin@ntu.edu.sg  
