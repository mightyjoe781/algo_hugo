+++

title = "4-Small Subfiles"

+++

### Small Subfiles

we can change the test at the beginning of the recursive routine from a return to a call on insertion sort, as follows.

` if ( r -l <= M ) insertion(a, l, r);`

Here , $M$ is parameter whose exact value depends upon implementation.

Between about 5 to 25, performance is almost same but quite better than $ M  = 1 $ ( 10% better ).

 A slightly easier way to handle small subfiles, which is also efficient than above is

` if (r - l <= M ) return ;`

We can later sort the M files using insertion sort. This method is fail proof in the sense even the quicksort doesn't do its job but insertion sort will definitely do its job.

