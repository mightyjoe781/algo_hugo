+++

title = "7-Linked list implementations of mergesort"

+++

### Linked list implementation of mergesort

Extra space has to be required for conception practical mergesort, se we may consider auxiliary array space for the linked list in form of links.

**Linked list merge**

````c++
link merge(link a, link b ){
    node dummy(0); link head = &dummy, c= head;
    while((a!=0) && (b!=0))
        if(a->item < b->item)
        {c->next = a; c= a; a = a->next;}
    	else
        {c->next = b ;c= b ; b= b->next;}
    c->next = (a==0) ? b :a;
    return head->next;
}
````

**Top-down list mergesort**

````c++
link mergesort(link c){
    if(c==0 || c->next == 0) return c;
    //splits link pointed by c into two lists a, b
    //then sorting two halves recursively
    //and finally merging them
    link a = c, b = c->next;
    while((b!=0) && (b->next!= 0))
    { c = c->next; b = b->next->next;}
    b = c->next; c->next = 0;
    return merge(mergesort(a),mergesort(b));
}
````

**Bottom-up list mergesort**

````c++
link mergesort(link t){
    QUEUE<link> Q(max);
    if(t==0 || t->next == 0) return t;
    for(link u =0 ; t!=0 ; t= u)
    {u = t->next ; t->next = 0; Q.put(t);}
    t = Q.get();
    while(!Q.empty())
    {Q.put(t); t = merge(Q.get(),Q.get());}
    return t;
}
````

