---
title: "Mutation Testing to uncover Zero Days"
tags: [ "Security Testing", "0-day"]
date: 2017-12-18
cover: "/media/MutationMagic.gif"
---

## Mutation Test

The Mutation Test is a technique that was proposed by De-Millo and it consists on creating a set of faulty versions of the test program called mutants. The goal for the tester is then to write a series of tests that can distinguish the original program from all its mutants. This technique not only help in generate very good data set for testing but also help in uncovering dark corners of software.

A score of a mutation that corresponds amount of <span style="background-color: #FF0000">bloodshed</span> possibility via the test case data set against all mutants. If test set kill all mutants then score will be 100%. This score is used as
a quality index of a test set. If a test T killed m mutants and there are a total of M mutants, the mutation score test T
is :

```math
SM (T) = m / M
```

A fault from security perspective is seen as a security flaw. It can be converted to exploit or may not that's all together different issue. Therefore, the injection of a fault in the security logic of any application is not only an injection, but at this time, it is a security flaw. This latter is generally based on the intrusion in the application environment using the interaction points of the application such as variables, files and processes.
The mutants are the variants of the original application which has released some of its protective mechanisms. A security
mutant differs from the reference application by the introduction of errors in one or many of its security mechanisms or logic. Thus, we can couple an error in the input user treatment with an error in the implementation of the security policy. We are therefore looking for creating mutants by mutating the control code of input (these mutants can successfully test for XSS, injections & many other attacks). This is not limited to input code only, we can create a mutant that allow a user to login without credentials and then check the efficacy of remaining security check aka application logical flow. This surely helps in validating the security controls at a deeper level and remove the vulnerability before it get lost in K-kilos of code lines.



**Mutpy Sample**

![ Mutation Testing Python](/media/MutationTesting.gif)


##### **References**:
1. [Mutation Testing](https://en.wikipedia.org/wiki/Mutation_testing)
2. [GitHub](https://pypi.python.org/pypi/MutPy/0.5.1) 
3. [Web application security assessment by fault injection and behavior monitoring](https://dl.acm.org/citation.cfm?id=775174)

