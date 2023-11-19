+++

title = "Tries"
date = 2021-05-25T12:25:25+05:30
weight = 3

+++

Problems related to finding the strings or traversing the multiple string come under the topic tries.

Tries means retrieval.

- Ordered tree is more like DS.
- Dictionary word searches / spell checking/ search engine.

Problem Statement

- Given a lot of strings : find an associative property among all strings
- Pros : Retrieval time is quite less than hash table and BST

- Cons :  Complex, and requires a lot of memory

[Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/)

````c++
class Trie {
    public:
    Trie() { root = new trieNode();}
    void insert(string word) {
        link temp = root; 
        for(char c:word){
            if(temp->next[c-'a'] == NULL) temp->next[c-'a'] = new trieNode();
            temp = temp->next[c-'a'];
        }
        temp->isEndHere = true;
    }

    /** Returns if the word is in the trie. */
    bool search(string word) {
        link temp = root;
        for(char c:word){
            if(temp->next[c-'a'] == NULL) return false;
            temp = temp->next[c-'a'];
        }
        return temp->isEndHere;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        link temp = root;
        for(char c:prefix){
            if(temp->next[c-'a'] == NULL) return false;
            temp = temp->next[c-'a'];
        }
        return true; 
    }
    private:
    // * Private DS - never expose them * //
    struct trieNode{
        trieNode* next[26] = {NULL};
        bool isEndHere = false; // optional and keeps on changing
    }; 
    typedef trieNode* link;
    link root;  
};
````



[Design Add and Search Words Data Structures](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

````c++
class WordDictionary {
    public:
    /** Initialize your data structure here. */
    struct trieNode{
        trieNode* next[26] = {NULL};
        bool isEndHere = false;
    };
    typedef trieNode* link;
    link root;
    WordDictionary() {
        root = new trieNode();
    }

    void addWord(string word) {
        link tmp = root;
        for(char c : word){
            if(tmp->next[ c-'a'] == NULL) tmp->next[c-'a'] = new trieNode();
            tmp = tmp->next[c-'a'];
        }
        tmp->isEndHere = true;
    }

    bool wildCardSearch(string suffix,link curr){
        for(int i = 0; i < suffix.size(); i++){
            char c = suffix[i];
            if(c!='.'){
                if(curr->next[c-'a'] == NULL) return false;
                curr = curr->next[c-'a'];
            } else {
                // if '.' explore all 26 possibilities given next
                // node exists.
                bool found = false;
                int j;
                for(j = 0; j < 26; j++){
                    if(curr->next[j] != NULL) 
                		found = wildCardSearch(suffix.substr(i+1),curr->next[j]);
                    if(found) return true;
                }
                // Exhausted a
                if(j==26) return false;
            }
        }
        return curr->isEndHere;
    }

    bool search(string word) {
        return wildCardSearch(word,root);
    }
};
````

