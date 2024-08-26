---
layout: page
permalink: /projects/
title: Projects
description: #Materials for courses you taught. Replace this text with your description.
nav: true
nav_order: 5
---

There are three main projects that I work on:
- [SpotOn: Generator-Based Fuzzer with Type-Targeted Mutation](#spoton-generator-based-fuzzer-with-type-targeted-mutation)
- [Java Ranger: A Path-Merging Tools for Java Program](#java-ranger-a-path-merging-tools-for-java-program)
- [ContractDR: A Contract Discovery and Repair Tool for Reactive Components](#contractdr-a-contract-discovery-and-repair-tool-for-reactive-components)



### SpotOn: Generator-Based Fuzzer with Type-Targeted Mutation

SpotOn is a Java fuzzer that uses the program types to generate mutants for fuzzing. SpotOn is built on top of Zest a generator-based fuzzer that uses programmable generators to construct inputs. In general, generator-based fuzzers like Zest do not have direct control over the program input, instead, they control the input to the generators which in turn controls the program input.

SpotOn presents novel technique that can focus the fuzzer to cover application code by mutating substructures in the input that are likely to trigger the intended code target. 
This approach is well-suited for a statically-typed object-oriented language like Java, where type information is directly
available in the bytecode (or source code) for both the input generators
and the program under test.

The core idea of our approach is to use type information to link input generation to
code in the program under test.
We do this connection in two steps: a static step, and a dynamic one.
In the static phase, we collect for each branch in the 
program under test, a ranked list of
*influencing types* of the objects that are data-flow predecessors
of the branch condition. This process is done offline before the running of the fuzz testing process. 

The dynamic phase is the fuzz testing process. 
In this step, we associate types with segments of the FCI, namely the return types
of input generator methods. 
Since generator methods commonly call
other generator methods, our implementation uses a simple form
of execution indexing to record the type information
for input-generating code in the context of its call stack.
Finally, to direct the fuzzer toward mutating likely useful types, our system attempts to match the statically identified influencing types of uncovered branches with the dynamic types associated with segments of the FCI. If a match is found, then GBF focuses on mutating those segments within the FCI with the matched type.

SpotOn supports fuzzing of serverless application on the cloud. In particular, it supports fuzzing of AWSLambda applications. Evaluating SpotOn on a test suit from GitHUb, shows significant improvements (20% or higher) over baseline generator-based fuzzers. 

[<img src="../assets/img/tryit.png" alt="img" width="35"/>](https://github.com/Fuzz-Testing/SpotOn) Try it on GitHub.
[<img src="../assets/img/read.jpeg" alt="img" width="35"/>](../assets/pdf/Generator-Based_Fuzzers_with_Type-Based_Targeted_M.pdf) Read our main Paper.

### Java Ranger: A Path-Merging Tools for Java Program

Java Ranger (JR): a path-merging tool for Java programs. We show how a Java region of code can be summarized into a disjunctive constraint.
Java Ranger first identifies a region of code where branching is about to happen. Then, it attempts to collapse the region of code representing the branching into a predicate that describes the region. To do that, Java Ranger uses a sequence of *transformations* that each takes a representation of a region of interest and attempts to systematically translate one language feature. For example,  _high-order_ transformation removes method invocation by inlining the method's definitions. The _field_ transformation removes references by creating a series of static single assignments of local variables that reflect the reference value.  Similarly, _single-path cases_ transformation allows partial summarization of a region of code which allows non-summarized regions behavior to be executed dynamically. 
% d exceptions as unsummarized paths and which are constitute the region's _exit-points_.

We evaluated JR over nine Java benchmarks. Results show that JR can reduce the number of execution paths and the running time by 71% and 38%, respectively, when compared to the baseline tool that does not perform path merging.
JR won top place (Gold Medal) in a large software verification competition in 2020, 2021, and a bronze medal in 2022.


[<img src="../assets/img/tryit.png" alt="img" width="35"/>](https://github.com/vaibhavbsharma/java-ranger) Try it on GitHub.
[<img src="../assets/img/read.jpeg" alt="img" width="35"/>](../assets/pdf/java-ranger-FSE2020-final-version.pdf) Read our main Paper.


### ContractDR: A Contract Discovery and Repair Tool for Reactive Components

ContrctDR is a contract discovery and a repair tool that takes as an input the implementation of a reactive component and repairs its contract using the component's implementation as a reference. The algorithm that ContractDR is sound and terminating but boundedly minimal. 

The process starts by translating the component\rq s implementation into a dataflow representation amenable to model-checking. 
If the hypothesized contract was invalid with respect to the implementation,  our technique attempts to repair it.
 To do that, we punch holes (identifying repair points) in the given contract to be repaired. 
 Then we repeatedly find _candidate contracts_, possible repaired contracts that are not yet checked to be valid over the implementation. Once our technique finds an _initial repaired contract_, the first validated contract over the implementation, it attempts to find a tighter repaired contract. In general, our technique can find multiple _repaired contracts_, candidate contracts that were checked and found valid over the implementation.
The goal of our technique is to find a _minimal repaired contract_, a repaired contract that cannot be further tightened.

To generate replacement expressions needed for the repair, we use the _Sketching_ technique. Generally, sketching is a program synthesis technique that synthesizes pieces of the partially detailed program using another implementation as a reference. Our usage for the sketch technique for repairing contracts is similar, but for synthesizing a partially detailed contract, as opposed to a partially detailed program; our process tries to synthesize partial contracts using the component's implementation as a reference. 

Results show that our technique has a successful repair rate of 82%, with 21% of the repairs matching the manually written contracts and a further 61% of the repairs describing non-trivial valid contracts. 

[<img src="../assets/img/tryit.png" alt="img" width="35"/>](https://github.com/sohah/ContractDR) Try it on GitHub.
[<img src="../assets/img/read.jpeg" alt="img" width="35"/>](../assets/pdf/FormaliSE2022.pdf) Read our main Paper.
