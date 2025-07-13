# VALID PALINDROME

**Given a string `s`, return `true` if it is a palindrome, otherwise return `false`.**

**A palindrome is a string that reads the same forward and backward. It is also case-insensitive and ignores all non-alphanumeric characters.**

**Note: Alphanumeric characters consist of letters `(A-Z, a-z)` and numbers `(0-9)`.**

**Example 1:**

```java
Input: s = "Was it a car or a cat I saw?"

Output: true

```

My intuition:

Giving the input string it is a sentence with maxed case characters. the question mentions that they are case insensitive. I need to clear the input string of non alphanumeric characters. I can use alnum in py to do that.

Once I am done I can take I from 0 till end and J from end till 0 and check each character if there is a miss match I can return False otherwise I can return true. 

CODE:

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        str1 = ""
        for c in s:
            if c.isalnum():
                str1+=c.lower()

        i = 0
        j = len(str1) - 1

        while i<j:
            if str1[i] != str1[j]:
                return False
            j -=1
            i +=1 
        return True
```

# **Two Integer Sum II**

**Given an array of integers `numbers` that is sorted in non-decreasing order.**

**Return the indices (1-indexed) of two numbers, `[index1, index2]`, such that they add up to a given target number `target` and `index1 < index2`. Note that `index1` and `index2` cannot be equal, therefore you may not use the same element twice.**

**There will always be exactly one valid solution.**

**Your solution must use O(1)*O*(1) additional space.**

**Example 1:**

`Input: numbers = [1,2,3,4], target = 3

Output: [1,2]`

My intuition:

BRUTE FORCE:

The brute force solution is to store all the elements in a hash map with values as keys and indices as the values and check if the target is present. But to do so the TC will be O(N) and SC will be O(N)

Better approach:

We can use Binary search for this for each element in the right subarray but that will take TC to O(nlgn) and SC to O(1).

Optimal approach:

We can use a two pointer approach since the array is sorted we can move I from start till end and j from end till start. if their sum is < target then we increment I if more then increment J if same I can return the value.

CODE:

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        
        i = 0
        j = len(numbers)-1
        while i<j:
            if (numbers[i] + numbers[j]) < target:
                i +=1
            elif (numbers[i] + numbers[j]) > target:
                j -=1
            elif (numbers[i] + numbers[j]) == target:
                return [i+1, j+1]
```

# 3 SUM

**Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` where `nums[i] + nums[j] + nums[k] == 0`, and the indices `i`, `j` and `k` are all distinct.**

**The output should *not* contain any duplicate triplets. You may return the output and the triplets in any order.**

**Example 1:**

`Input: nums = [-1,0,1,2,-1,-4]

Output: [[-1,-1,2],[-1,0,1]]`

My intuition:

Brute force:

first intuition is to take one element each and run two loops I and j to get the sum, if I get a sum of 0 append it to resultant.

Better/optimal

sort the array

take one element and from element + 1 to end two pointer

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []

        for i in range(len(nums)):
            
            first = nums[i]
            if first > 0:
                break
            if i>0 and nums[i] == nums[i-1]:
                continue
            
            l = i+1
            r = len(nums)-1

            while l<r :
                three = nums[i] + nums[l] + nums[r]
                if three > 0:
                    r -= 1
                elif three < 0:
                    l += 1
                else:
                    res.append([nums[i], nums[l], nums[r]])
                    l += 1
                    r -= 1
                    while nums[l] == nums[l - 1] and l < r:
                        l += 1
                        
        return res
```

# Container with Water:

**You are given an integer array `heights` where `heights[i]` represents the height of the ith*ith* bar.**

**You may choose any two bars to form a container. Return the *maximum*amount of water a container can store.**

```python
class Solution:
    def maxArea(self, heights: List[int]) -> int:
        left = 0
        right  = len(heights)-1
        result = 0
        area = 0
        while left<right:
            area = min(heights[left], heights[right]) * (right-left)
            result = max(result,area)
            if heights[left] <= heights[right]:
                left +=1
            else:
                right -=1
        return result
```
