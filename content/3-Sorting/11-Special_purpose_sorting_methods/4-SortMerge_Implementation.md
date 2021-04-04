+++

title = "4-SortMerge Implementation"

+++

### Sort-Merge Implementations

General strategy in earlier section is effective in practice. Two improvements that we can use to lower the costs.

1. Replacement Selection
2. Polyphase Merging



*Property 5* : For random keys, the runs produced by replacement selection are about twice the size of the heap used.

This improvement is not noticeable for big P values.

The major weakness of balanced multiway merging is that only about one-half the devices are actively in use during the merges.

 A clever technique which keeps all external devices busy is polyphase merging.

Basic Idea is to distribute the sorted blocks produced by replacement selection somewhat unevenly among the available tape units (leaving one empty) and then to apply a merge until-empty strategy. Since tapes being merged are unequal in size so one of them will run out early and it can be used as output.

***Property 6*** : With three external devices and internal memory sufficient to hole M records, a sort-merge that is based on replacement selection followed by a two way polyphase merge takes $ 1 + \lceil \log_{\phi} (N/2M) \rceil / \phi$ effective passes.

Many modern computer system provide a large virtual memory capability a more general abstract model for accessing external storage than the one we have been using. We could implement sort-merge directly or even simpler, could use a internal sorting methods such as quicksort/mergesort. Heapsort and radix sort are bad option cause they will cause *thrashing*

