+++

title = "0-Introduction"

+++

### Introduction

Sorting methods are critical components of many applications systems, and it is not unusual for special measures to be taken to make a sort as fast as possible or capable of handling huge files. We often have sorting methods that are designed to run efficiently on various different kinds of machines.

Any new computer architecture is eventually going to need to support an efficient sorting method. Indeed sorting has historically served as one testbed for evaluating new architectures, because it is so important and so well understood.

We consider low-level models where the only allowed operation is the compare-exchange operation. At the other end of spectrum, we shall consider high-level models where we read and write large blocks of data to a slow external medium or among independent parallel processor.

First we look at a version of mergesort known as Batcher's odd-even mergesort. It is based on a divide-and-conquer merge algorithm that uses only compare-exchange operations, with perfect shuffle and perfect-unshuffle operation for data movement.

Next we examine Batcher's Method as a sorting network. Networks consists of interconnected comparators., which are modules capable of performing compare-exchange operation.

Another important abstract sorting problem is external -sorting problem, where the file to be sorted is far too large to fit in memory. Cost of accessing individual records can be prohibitive, so we shall use an abstract model, where records are transferred to and from external devices in large blocks and use model to compare them.

Finally we consider parallel sorting, for the case when the file to be sorted is distributed among independent model, then examine how Batcher's method provides a effective solution.

