+++

title = "9-Sorting of Linked Lists"

+++

### Sorting of Linked Lists

****



**Linked-list-type interface definition**

````c++
//sorting program uses a overloaded operator< to compare items
struct node {
    Item item ; node* next ;
    node(Item x)
    	{ item = x; next = 0 ;}
};
typedef node *link;
link randlist(int);
link scanlist(int&);
void showlist(link);	//prints list
link shortlist(link);
````

**One liner driver program for scanning and sorting using above interface**

````c++
main(int argc, int *argv[]){
    showlist(sortlist(scanlist(atoi(argv[])))); }
````

Note this interface of linked list is a low-level one that doesn't make a distinction between a link (a pointer to a node) and  a linked list(a pointer that is either 0 or a pointer to a node containing a pointer to a list).

**Linked-list selection sort**

````c++
link listselection(link h){
    node dummy(0); link head = &dummy, out = 0 ;
    head -> next = h;
    while(head->next != 0 ){
        link max = findmax(head) , t = max->next;
        max -> next = t->next;
        t->next = out; out = t;
    }
    return out;
}
````

