+++

title = "7-Strings and Vectors"

+++

### Strings and Vectors

we can directly use our previous implementations for strings but the problem is with the `strcmp` function which always takes time proportional to the number of leading characters that match.

When using quicksort in later stages while partitioning stages of quicksort, when keys are close together, this match might be long.

Also since algorithm is recursive so all the cost incur at the later stages of the recursion.

e.g. discreet, discredit, discrete, discrepancy, and discretion

new information happens at 6th character.

Three way partitioning procedure that we procedure that we saw last time provides an elegant way to take advantage of his observation. At each partitioning stage, we examine only one character ( say at position `d`), assuming that the keys to be sorted are equal in position `0` through `d-1` . We do a three - way partition with keys whose `dth` character is equal to the `dth` character of the partitioning element on the right. Then we proceed as usual,except that we sort middle subfile, starting at character `d+1` . It is not difficult to see that this method leads to a proper sort on strings, which turns out to be very efficient.