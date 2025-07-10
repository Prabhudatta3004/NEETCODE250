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
