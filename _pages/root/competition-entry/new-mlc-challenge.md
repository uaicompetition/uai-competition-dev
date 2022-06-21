---
title: "New Learned Multi-Label Classification Challenge!"
usemathjax: true
permalink: /competition-entry/new-mlc-challenge/
---

This year we are excited to include a new challenge of learning **multi-label classification**.<br/>
This challenge will have two phases: 
* an offline learning phase (for which you will use your own hardware)
* an online prediction phase (for which we will use our hardware).


## Offline Training Phase

For the offline training phase, you will be provided with the following:
1. Several Markov networks in the UAI format
2. For each Markov network, a partition of the set of variables \\(X\\) into three subsets:
  * hidden variables \\(H\\), 
  * observed or evidence variables \\(E\\)
  * query variables \\(Q\\) 
  * Namely, \\(H \\cup E \\cup Q = X\\), \\(H \\cap E = \\emptyset\\), \\(H \\cap Q = \\emptyset\\), and \\(H \\cap E = \\emptyset\\).
3. a data generator that takes a Markov network, a partition \\(H,E,Q)\\) and an integer \\(m\\) as input and outputs \\(m\\) pairs where each pair contains an example and a real number which provides a score for the example. Each example is an assignment of values to the observed and query variables and the score lies between \\(-\infty\\) and \\(\infty\\). Details on the data generator are given below.


For each Markov network, the partition will be stored in a separate file. 
This file has the same name as the original network file but with an added **.partition** suffix. 
For instance with respect to the UAI model format, _problem.uai_ will have evidence in _problem.uai.partition_.

The partition file has two lines. The first line will begin with the number of evidence variables, followed by the indexes of the evidence variables; and the second line will begin with the number of query variables, followed by the indexes of the query variables.

For example, given a Markov network having 10 variables, let the indices of the hidden, evidence and query variables be \\(0,2,3,9)\\), \\(1,4,7\\) and \\(5,6,8\\) respectively. Then the partition file would contain the following:

```
3 1 4 7
3 5 6 8

```


The Markov networks, partition of variables into query, hidden and evidence variables for each
network as well as the data generators will be published on the competition website. 

You can use your own computational resources (and therefore this phase is offline) to compile each Markov
network plus data to a relevant representation such that the following multi-label classification
task can be solved accurately and efficiently: *find the most-probable assignment (w.r.t. the Markov
network) to the query variables given an assignment of values to the evidence variables*.

Example representations include but are not limited to neural networks, graph neural networks,
auto-regressive models, junction trees, arithmetic circuits, AND/OR graphs, sum-product networks,
cutset networks, and probabilistic circuits. Note that you can use the Markov network
or the data or both to learn a *new representation*.

Technically, given a Markov network M, this task is equivalent to the marginal maximum-aposteriori
(MMAP) task when the subset \\(H\\) is not empty. When \\(H\\) is empty, the task is equivalent to the MAP task. 
Thus approaches that forgo the compilation phase and solve the prediction
task at test time using a MMAP solver can also participate in this task.

### Data Generator
The goal of the data generator is to provide a signal to your learning algorithm by sampling the Markov network. 
Given a Markov network representing the probability distribution \\(Pr(H,E,Q)\\). the data generator will generate \\(m\\)
samples from \\(Pr(E)\\) (via Gibbs sampling) and for each sampled assignment \\(E=e\\), perform local search to find an assignment \\(q\\) to
the query variables \\(Q\\) such that the probability \\(Pr(Q=q,E=e)\\) is maximized. Since the local search may not find an
optimal assignment to the query variables, the data generator will output a score for the assignment. 

For each assignment \\(q,e\\), the score is given by log10 Pr(q,e) + log10 Z where Z is the partition function of the Markov network. 

As mentioned earlier, the data generator will take as input the Markov network, the partition file, an integer m and a location for the output file. After termination, the output file will contain the samples over the evidence and query variables as well as a score for each sample in the *data format*. 

Note that you are free to write your own data generator or not use it at all. It is provided for convenience only.

## Online Prediction Phase

You will provide us your compiled/learned models \\(C_k\\) (if any) for
each Markov network \\(M_k\\) and a program which takes as input (1) your compiled model \\(C_k\\) (if a
compiled model is not provided, we will assume that \\(C_k = M_k\\)), (2) an evidence file, (3) a query file and a path to the result file. Your program should output the most-probable assignment to all the query variables given evidence and store the result in the result file (see result format).
