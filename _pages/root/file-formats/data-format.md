---
title: "File Formats"
permalink: /file-formats/data-format/
---

## Data Format (for MLC task only)
Data is specified in a separate file. This file has the same name as the original network file but with an added **.data** suffix. 
For instance, _problem.uai_ will have training data in _problem.uai.data_ file.

The evidence file consists of a _T+3_ lines where _T_ is the number of data points.
* The first line in the file will specify the number of data points.
* The second line in the file will begin with the number of observed or evidence variables followed by the indexes of the observed variables. The indexes correspond to the ones implied by the original problem file.
* The third line in the file will begin with the number of query variables (or labels) followed by the indexes of the query variables. Again, the indexes correspond to the ones implied by the original problem file.
* The remaining _T_ lines will specify the data points. Each line will contain an assignment _(q,e)_ to the query and observed variables followed by the weight of the assignment. The weight of the assignment _(q,e)_ is given by _log10 Pr(q,e) + log10 Z_ where _Z_ is the partition function of the Markov network. 

For example, given a Markov network having _10_ variables, let the indices of the evidence (observed) and query variables be (_1,4,7_) and (_5,6,8_) respectively. Given the following _2_ data points:

* The first data point is an assignment of values _(0,2,1)_ and _(2,1,0)_ to the evidence variables (_1,4,7_) and (_5,6,8_) respectively. The weight of the assignment is -48.21
* The second data point is an assignment of values _(1,0,1)_ and _(1,0,3)_ to the evidence variables (_1,4,7_) and (_5,6,8_) respectively. The weight of the assignment is -76.27

the data file will contain the following:

```
2
3 1 4 7
3 5 6 8
1 0 4 2 7 1 5 2 6 1 8 0 -48.21
1 1 4 0 7 1 5 1 6 0 8 3 -76.27
```
