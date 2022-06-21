---
title: "File Formats"
permalink: /file-formats/data-format/
---

## Data Format (for MLC task only)
Data is specified in a separate file. This file has the same name as the original network file but with an added **.data** suffix. 
For instance, _problem.uai_ will have training data in _problem.uai.data_ file.

The evidence file consists of a _T+3_ lines where _T_ is the number of data points.
The first line in the file will specify the number of data points.
The second line in the file will begin with the number of observed variables followed by the indexes of the observed variables. The indexes correspond to the ones implied by the original problem file.
The third line in the file will begin with the number of query variables (or labels) followed by the indexes of the query variables. Again, the indexes correspond to the ones implied by the original problem file.
The remaining _T_ lines will specify the data points. Each line will contain an assignment to the query and observed variables followed by weight of the assignment where the weight of the assignment _(q,e)_ is given by _log10 Pr(q,e) + log10 Z_. Here _Z_ is the partition function of the Markov network. 

For our example Markov network given [Model Format](./model-format.md), 
if Y is an observed variable and X is the query variable, then the data file may contain

```
2
1 1
1 2
1 0 2 1 -13.32
1 1 2 0 -23.22
```
