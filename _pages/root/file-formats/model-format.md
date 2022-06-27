---
title: "File Formats"
permalink: /file-formats/model-format/
---

# Model Format
We use a simple text-file format specified below to describe the graphical model neworks (Markov networks) for problem instances. 
The format is a generalization of the Ergo file format initially developed by Noetic systems Ergo software.
We use the suffix **.uai** to denote the model files.

## Structure
A file in the UAI format consists of the following two parts, in order:
```
<Preamble>
<Function tables>
```
which are described below.

## Preamble
The preamble consists of the following portions, in order:
```
<Graph Type>
<Variables and Domains>
<Function Scopes>
```
which we now describe...

### Graph Type

The Graph Type is a metalabel signifying the type of network being encoded.  Generally, this can be either BAYES for a Bayesian network or MARKOV for a Markov network. For our competition, the label will always be:
```
MARKOV
```


### Variables and Domains

This portion of the preamble contains two lines.

The first line consists of a single number, N, specifying the number of variables in the network.
Each variable is implicitly labled as 0, 1, ... , N.

The second line specifies the cardinalities of each variable separated by a whitespace (in order from 0, 1, ... , N).

An example of a network with three variables, the first two having a domain size of 2
and the last variable having a domain size of 3, would look like:
```
3
2 2 3
```

### Function Scopes

The last section of the preamble states the number of functions in the network as well as their respective scopes.

First is a single number specifying the number of functions in the network.

Next, there will be a line for each function.  Each successive line (which is for a single funciton)
states the number of variables that belong to the function's scope followed by the index identity of
each of the variable's in the function's scope.

For a network containing two functions, the first of which has a scope consisting of variable index 0 and variable index 1
and the second of which has a scope consisting of variable index 1 and variable index 2, the file would show:
```
2
2 0 1
2 1 2
```
(Note: The order of the list of varaibles in a function's scope is not restricted. When defining the function values, the ordering of variables within a factor will follow the order provided here).

### Preamble Summary
The preamble for our simple Markov network with three variables and two functions described above would look like:
```
MARKOV
3
2 2 3
2
2 0 1
2 1 2
```
In the example above, the first line denotes a Markov network, the second line tells us the problem consists of three variables, let's refer to them as X, Y, and Z (which will implicitly have indexes 0, 1, and 2, respectively). The third line tells us that X, Y, and Z's cardinalities are 2, 2, and 3 respectively. Line four specifies that there are 2 functions. Based on the final two lines, we know that the first function is defined over X and Y, while the second is defined over Y and Z.


## Function tables
Under the preample, 
each factor is specified by giving its full table by specifying value for each assignment. 
The order of the factor must be identical to the one in which they were introduced in the preamble.
The first variable have the role of the 'most significant’ digit. 

* For each factor table, the first the number of entries (row) is given, and this should be equal to the product of the domain sizes of the variables in the scope. 
* Then, one by one, separated by whitespace, the values for each assignment to the variables in the function's scope are enumerated. 
* Tuples are implicitly assumed in ascending order, with the last variable in the scope as the 'least significant’. 
* To illustrate, we continue with our Markov network example from above, let's assume the following conditional probability tables:

| X | P(X) |
| :--- | :----: | 
| 0 | 0.436 |
| 1 | 0.564 |


| X |	Y |	P(Y,X) |
| :--- | :--- | :----: | 
| 0 |	0 |	0.128  |
| 0 |	1 |	0.872 |
| 1 |	0 |	0.920 |
| 1 |	1 |	0.080 |


| Y | 	Z | 	P(Z,Y) | 
| :--- | :--- | :----: | 
| 0 | 	0 | 	0.210 | 
| 0 | 	1 | 	0.333 | 
| 0 | 	2 | 	0.457 | 
| 1 | 	0 | 	0.811 | 
| 1 | 	1 | 	0.000 | 
| 1 | 	2 | 	0.189 | 

```
2
 0.436 0.564

4
 0.128 0.872
 0.920 0.080

6
 0.210 0.333 0.457
 0.811 0.000 0.189
```

Note that line breaks and empty lines are effectively just a whitespace, exactly like plain spaces “ ”. 
They are used here to improve readability.

## Summary
In summary, a problem file consists of 2 sections: 
* the preamble
* the function tables.


For our Markov network example above, the full uai file will look like:
```
MARKOV
3
2 2 3
3
1 0
2 0 1
2 1 2

2
 0.436 0.564

4
 0.128 0.872
 0.920 0.080

6
 0.210 0.333 0.457
 0.811 0.000 0.189
```

