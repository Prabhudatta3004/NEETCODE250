# CONTAINS DUPLICATE

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

My Intuition:

Brute Force:

We can traverse through the list and calculate count for each element and store the count, if we encounter count of 2 for any element we can return true

But this will have a time complexity of O(n^2)

Better Solution:

To make the time complexity better I can sort the array and when we traverse through the array we can check if I+1 is equal to I but I+1 should be in bounds of array.

This gives me a time complexity of O(nlgn)

OPTIMAL SOLUTION:

I can use a hashset to store all the elements and can have a look at that with O(1) TC and when I see that an element in the list is already in the hashset I can return True

This gives me a TC of O(n) and SC of O(n)

CODE:

 

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hashset = set()
        for n in nums:
            if n in hashset:
                return True
            else:
                hashset.add(n)
        return False
```

# VALID ANAGRAM

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false`otherwise.

**Example 1:**

**Input:** s = "anagram", t = "nagaram"

**Output:** true

My intuition:

Brute Force:

My intuition is I can sort both the strings and check if the strings are equal of not.

This will give me a time complexity of O(nlgn).

Optimal Solution:

Creating two separate hashtables and comparing them wether they are same.

creating a hash table of size of the alphabet(26) for each of the string. Now when we traverse through the string we can add the count of each element in the hash table and then compare them. 

To create the hashtable it will take O(n) + O(m) given the fact that n and m are the sizes of the two strings. Ideally if they are anagrams n should be == m

CODE:

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        hash_1 = [0] * 26
        hash_2 = [0] * 26

        for n in s:
            if n not in hash_1:
                hash_1[ord(n) - ord('a')] +=1
            else:
                hash_1[ord(n) - ord('a')] =1
        for n in t:
            if n not in hash_1:
                hash_2[ord(n) - ord('a')] +=1
            else:
                hash_2[ord(n) - ord('a')] =1
        
        return (hash_1 == hash_2)
        
```

# TWO SUM:

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

My intuition:

Brute Force:

we can go for each element in the array and traverse through the array to add all the elements and whichever element when added up gives us the target we can just return the indices.

This will give us a TC of O(n^2)

Better Force:

we can create a stack for the right subarray and push all the elements, and then keep popping each element and when we get the target we can have a count that can be added to the current ith value.

This will have extra space for the stack although push and pop will have tc of O(1), we have a SC of O(n)

Optimal Solution:

we can have all the elements in a hash table, since the result needs us to return the indices we can have key:value pair as element:index in the hash table, we can calculate the difference between the current element and the target and then get the next ele we can get that from the hash table and return the indices.

This will give us a TC of O(N) and SC of O(N)

CODE:

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        lookup = {}

        for i in range(len(nums)):
            diff = target - nums[i]
            if diff in lookup:
                return [lookup[diff],i]
            else:
                lookup[nums[i]] = i
        return None
```

# Group Anagrams

**Given an array of strings `strs`, group all *anagrams* together into sublists. You may return the output in any order.**

**An anagram is a string that contains the exact same characters as another string, but the order of the characters can be different.**

**Example 1:**

`Input: strs = ["act","pots","tops","cat","stop","hat"]

Output: [["hat"],["act", "cat"],["stop", "pots", "tops"]]`

My intuition:

Brute Force:

My intuition for brute force is that I go through the list and sort each string and then group them separately. But that will increase my time complexity.

TC will be O(m*n lgn) jhere m is the length of the list and n is the string length which is the largest sized string.

Optimal Solution:

This is a two tier problem where at first I have to unpack the list and then unpack the strings in the list. Now I know that I can calculate the hash table for each element. For the result list the index can be these hash table values in which we can append the strings which get the same hash table value.

CODE:

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res = defaultdict(list)
        for s in strs:
            lookup = [0] * 26 ## This the hash lookup for each string
            
            for i in s:
                    lookup[ord(i) - ord('a')] +=1
            res[tuple(lookup)].append(s)
        
        return list(res.values())

```

This is an unique solution where we need to create a defaultdict of type list where we can keep on appending the strings as per the lookup as index , but since lookup is a list we need to make it immutable to make an index out of it so we convert it into a tuple and later we just return a list of all values as the answer wants us to give,.

# ENCODE AND DECODE STRINGS:

**Encode and Decode Strings**Solved

**Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings.**

**Please implement `encode` and `decode`**

**Example 1:**

`Input: ["neet","code","love","you"]

Output:["neet","code","love","you"]`

My intuition :

For me the first intuition the encoding is simple — I can combine all the mini string to create a new big string that can be sent for decoding. But for decoding we need to divide the bigger string. So in order to do that I can have the length of each string added along with a delimiter that marks the beginning of a string.

CODE:

```python
class Codec:
    def encode(self, strs: List[str]) -> str:
        """Encodes a list of strings to a single string.
        """
        final_str = ""
        for s in strs:
            final_str += str(len(s)) + "#" + s
        return final_str 
        

    def decode(self, s: str) -> List[str]:
        """Decodes a single string to a list of strings.
        """
        i = 0
        res = []
        while i < len(s):
            j = i
            while s[j] != "#":
                j +=1
            length = int(s[i:j])
            i = j + 1
            j = i + length
            res.append(s[i:j])
            i = j
        return res

# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.decode(codec.encode(strs))
```

# PRODUCT OF AN ARRAY EXCEPT ITSELF

**Given an integer array `nums`, return an array `output` where `output[i]` is the product of all the elements of `nums` except `nums[i]`.**

**Each product is guaranteed to fit in a 32-bit integer.**

**Follow-up: Could you solve it in O(n)*O*(*n*) time without using the division operation?**

**Example 1:**

`Input: nums = [1,2,4,6]

Output: [48,24,12,8]`

My Intuition:

BRUTE FORCE:

We can sweep through the list for each element and put a check that if the element is same we continue without multiplying and pushing the result into a result list

This will cost some Time Complexity of O(N^2) and a space complexity of O(N) due to the resultant array.

Better/Optimal Solution:

We need to store two things the prefix product and the postfix product, well we can return the prefix product and the postfix product which will exclude the current element.

This can be done in one sweep with extra space though

TC will be O(N) and SC would be O(N) too

basically we need to multiply the prefix value of the previous element and the postfix product of the next element

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prefix = [0] * len(nums)
        postfix = [0] * len(nums)
        res = [0] * len(nums)
        prefix[0] = postfix[len(nums)-1] = 1

        for i in range(1,len(nums)):
            prefix[i] = prefix[i-1] * nums[i-1]
        for j in range(len(nums)-2 , -1 , -1):
            postfix[j] = postfix[j+1] * nums[j+1]
        
        for i in range(len(nums)):
                res[i] = prefix[i] * postfix[i]
        return res
```

We can further optimize it’s space complexity

Lets take only the result array with all 1s of same size that of nums, Now we can make modifications on it to get the output

For prefix what we can do is we can use a variable, that will store the value of previous prefixes and that can be stored In the res and same can be done for postfix.

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        res = [1] * len(nums)

        prefix = 1
        for i in range(len(nums)):
            res[i] = prefix
            prefix *= nums[i]
        
        postfix = 1
        for i in range(len(nums)-1,-1,-1):
            res[i] *= postfix
            postfix *= nums[i]
        
        return res
       
```

# VALID SUDOKU

Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

My intuition :

The question says that we can check if there are no repetition 1. Rowwise 2. Columnwise 3. In 3*3 matrices of the sudoku board

For non repetition check we can put all element in a set and check if the element already exists if yes return False, since all the three has to be satisfied we need to have a check for rows, columns and smaller matrices. For storing the key:value pair per matrix we can do one thing divide the row value//3 and column value//3 this will yield in 0,1,2 indices that can be used as keys for the boxes. so the index will be [r//3,c//3]

This can be solved in O(1) TC and O(1) SC, the TC is O(1) because there is no parsing the board one by one rather we are doing lookup,

CODE:

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        cols = defaultdict(set)
        rows = defaultdict(set)
        squares = defaultdict(set)  

        for r in range(9):
            for c in range(9):
                if board[r][c] == ".":
                    continue
                if ( board[r][c] in rows[r]
                    or board[r][c] in cols[c]
                    or board[r][c] in squares[(r // 3, c // 3)]):
                    return False

                cols[c].add(board[r][c])
                rows[r].add(board[r][c])
                squares[(r // 3, c // 3)].add(board[r][c])

        return True
```

# LONGEST CONSECUTIVE SUBSEQUENCE:

**Given an array of integers `nums`, return *the length* of the longest consecutive sequence of elements that can be formed.**

**A *consecutive sequence* is a sequence of elements in which each element is exactly `1` greater than the previous element. The elements do *not* have to be consecutive in the original array.**

**You must write an algorithm that runs in `O(n)` time.**

**Example 1:**

`Input: nums = [2,20,4,10,3,4,5]

Output: 4`

CODE:
```
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        hashset=set()
        largest=0
        count = 0
        for n in nums:
            if n not in hashset:
                hashset.add(n)
        
        for n in nums:
            if n-1 not in hashset:
                count = 1
                while n+count in hashset:
                    count +=1
            largest = max(count,largest)
        return largest
```
