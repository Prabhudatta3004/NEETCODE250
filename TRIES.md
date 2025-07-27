Trie is a data structure used for word searches, autofill and spell checker algorithms. It stores keys of strings which helps us with the retrieval. It has mainly 3-5 functionalities depending on their use case. The 3 basic functionalities are INSERT , SEARCH, STARTSWITH

 

**`PrefixTree()` Initializes the prefix tree object.`void insert(String word)` Inserts the string `word` into the prefix tree.`boolean search(String word)` Returns `true` if the string `word` is in the prefix tree (i.e., was inserted before), and `false` otherwise.`boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false`otherwise.**

- `PrefixTree()` Initializes the prefix tree object.
- `void insert(String word)` Inserts the string `word` into the prefix tree.
- `boolean search(String word)` Returns `true` if the string `word` is in the prefix tree (i.e., was inserted before), and `false` otherwise.
- `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false`otherwise.

# INSERTION()

Now the process of inserting into the trie is easy. We create two member variables of type trie class. One is an array of size 26 for the alphabet and the other is a flag. Now the process is let's say we need to insert the word apple . At first we will have a trie object and a flag set to false for the character a (root node). Now after we insert a, a will start pointing to its reference trie with the same object characteristics. Now while inserting p, we go to a’s reference trie see if p doesn't exist we insert p and therefore create another reference trie for p. This process continues till the end of the word apple and the last reference trie for the character e has the flag set to True stating that the word exists in the trie. Now when we try using any other word let's say apps, we check with the root trie does a exists : YES lets go to its reference trie does p exists : YES move to the next trie , Does p exists again : YES, move to its reference trie , does s exists: NO , then add s to the reference trie of the second p and create another reference trie for s and mark its flag yes stating that the word apps exists.

# SEARCH()

The search function is quite simple, we start from the root trie node and keep on checking for the

characters if we reach to the end flag being True then the word exists otherwise the word does not exist

# STARTSWITH()

The starts with function  checks if we have words that starts with a string. So when we traverse through the trie data structure we can visit the reference node for each character in the given string and in the end of that string, we will reach a trie node that has some other char but not NONE. If we hit NONE that means we don’t have the string that was given to us for checking if we have words that starts with those characters.

```python
class Trie:
    def __init__(self):
        self.reference = [None] * 26
        self.endflag = False
class PrefixTree:

    def __init__(self):
        self.root = Trie() ## At first we need to create the trie
        ## for root node

    def insert(self, word: str) -> None:
        ## Insert function starts from root
        current_node = self.root
        ## THen checks if the character is present or not
        for w in word:
            i = ord(w) - ord('a')
            if current_node.reference[i] == None: ## Char is not present
                current_node.reference[i] = Trie() ## creating a new trie object for the character
            current_node = current_node.reference[i]  ## moving to the new char reference node
        current_node.endflag = True ## Setting the last bool flag to be true

    def search(self, word: str) -> bool:
        curr = self.root
        for w in word:
            i  = ord(w) - ord('a')
            if curr.reference[i] == None:
                return False
            curr = curr.reference[i]
        return curr.endflag
    def startsWith(self, prefix: str) -> bool:
        curr = self.root
        for p in prefix:
            i = ord(p) - ord('a')
            if curr.reference[i] == None:
                return False
            curr = curr.reference[i]
        return True        
        
```

TC : O(N) where N is the size of the given string

SC: O(T) for each character in the string(s) we create reference trie nodes which is T 

# **Design Add and Search Word Data Structure**

**Design a data structure that supports adding new words and searching for existing words.**

**Implement the `WordDictionary` class:**

**`void addWord(word)` Adds `word` to the data structure.`bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.**

- `void addWord(word)` Adds `word` to the data structure.
- `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

**Example 1:**

`Input:
["WordDictionary", "addWord", "day", "addWord", "bay", "addWord", "may", "search", "say", "search", "day", "search", ".ay", "search", "b.."]

Output:
[null, null, null, null, false, true, true, true]

Explanation:
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("day");
wordDictionary.addWord("bay");
wordDictionary.addWord("may");
wordDictionary.search("say"); // return false
wordDictionary.search("day"); // return true
wordDictionary.search(".ay"); // return true
wordDictionary.search("b.."); // return true`

INTUITION:

This is a very similar question as that of the actual TRIE implementation. But here we are going to use a different way of creating a trie object instead of using a list of size 26, we are going to use a hash map that defines character : Trienode(). 

Now the Add word to the data structure is pretty similar , we can just start from the root node of the trie and check if characters in the word are present as nodes in the trie if not keep on adding them and taking the current to the new reference node after iterating through the entire word just return the flag as true for the end node. 

For the searching , it is a bit tricky, for normal searching what we were doing was that we took each character of the word to search and checked the trie if a node exists and if the reference node of the last word is None we returned True else false.

But now we have input ‘.’ in the question. This dot can be placed anywhere and can have any character. So whenever we encounter the . we should just use recursion and check all the children from the current node if the next or upcoming words are present or not.

 

```python
class Trie:
    def __init__(self):
        self.children = {}  # character -> Trie node
        self.finalflag = False  # True if this marks end of word

class WordDictionary:

    def __init__(self):
        self.root = Trie()  # Root node of the Trie

    def addWord(self, word: str) -> None:
        curr = self.root
        for c in word:
            if c not in curr.children:
                curr.children[c] = Trie()
            curr = curr.children[c]
        curr.finalflag = True

    def search(self, word: str) -> bool:
        def dfs(i, curr):
            if i == len(word):
                return curr.finalflag ## after we reach the end of the search word

            c = word[i] ## this c is our character to inspect

            if c == ".": ## if c is a dot , explore all the children recursively
                for child in curr.children.values():
                    if dfs(i + 1, child):
                        return True
                return False
            else:
                if c not in curr.children:
                    return False
                return dfs(i + 1, curr.children[c])

        return dfs(0, self.root)

```

# **Word Search II**

**Given a 2-D grid of characters `board` and a list of strings `words`, return all words that are present in the grid.**

**For a word to be present it must be possible to form the word with a path in the board with horizontally or vertically neighboring cells. The same cell may not be used more than once in a word.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/06435c8e-bac3-49f5-5df7-77fd5dd42800/public)

`Input:
board = [
  ["a","b","c","d"],
  ["s","a","a","t"],
  ["a","c","k","e"],
  ["a","c","d","n"]
],
words = ["bat","cat","back","backend","stack"]

Output: ["cat","back","backend"]`
