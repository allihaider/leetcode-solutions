# Easy Problems

## 1. [Two Sum](https://leetcode.com/problems/two-sum/)
Given an array of integers and a target, return indices of the two numbers such that they add up to the target.

### Approach
Traverse through the elements of the array and check if *target - current element* exists in the hashmap, otherwise store the current element in the hashmap.

### Code 
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        num_2_ind = {}

        for i in range(len(nums)):
            rem = target - nums[i]

            if rem in num_2_ind:
                return [i, num_2_ind[rem]]
            else:
                num_2_ind[nums[i]] = i

        return
```

## 2. [Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
Given roman numerals and their values, convert a given roman numeral to an integer.

### Approach
Traverse through the elements of the roman numeral and add the value of the element to a running sum. Will need to cater to some exceptions.

### Code 
```
class Solution:

    def romanToInt(self, s: str) -> int:

        rnum_2_value = {
        "I": 1,
        "V": 5,
        "X": 10,
        "L": 50,
        "C": 100,
        "D": 500,
        "M": 1000,
        "IV": 4,
        "IX": 9,
        "XL": 40,
        "XC": 90,
        "CD": 400,
        "CM": 900,
        }

        int_sum = 0
        skip_next = False

        for i in range(len(s) - 1):

            if skip_next:
                skip_next = False
                continue

            if s[i] == "I" and (s[i+1] == "V" or s[i+1] == "X"):
                int_sum += rnum_2_value[s[i:i+2]]
                skip_next = True

            elif s[i] == "X" and (s[i+1] == "L" or s[i+1] == "C"):
                int_sum += rnum_2_value[s[i:i+2]]
                skip_next = True

            elif s[i] == "C" and (s[i+1] == "D" or s[i+1] == "M"):
                int_sum += rnum_2_value[s[i:i+2]]
                skip_next = True

            else:
                int_sum += rnum_2_value[s[i]]

        if not skip_next:
            int_sum += rnum_2_value[s[len(s) - 1]]

        return int_sum
```

## 3. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/description/)
Find the longest common prefix string amongst an array of strings.

### Approach
Loop over the elements of the first string in the array. Inside, loop over all the other strings and check for the presence of the common prefix.

### Code 
```
class Solution:

    def longestCommonPrefix(self, strs: List[str]) -> str:

        common_prefix = ""

        for i in range(len(strs[0])):     
            alphabet_to_check = strs[0][i]

            for ind_str in strs[1:]:
                if i == len(ind_str) or ind_str[i] != alphabet_to_check:
                    return common_prefix 

            common_prefix += alphabet_to_check
        
        return common_prefix
```

## 4. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/submissions/)
Given an string of parentheses, determine if all the open parentheses are being properly closed.

### Approach
Traverse through the characters in the string and use a stack to keep track of any open braces that need to be closed. 

### Code 
```
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []

        if len(s) == 1:
            return False

        for brace in s:
            if brace in ["(", "[" , "{"]:
                stack.append(brace)
            elif len(stack) > 0:
                if (stack[-1] == "(" and brace == ")") or (stack[-1] == "[" and brace == "]") or (stack[-1] == "{" and brace == "}"):
                    stack.pop()
                else:
                    return False
            else:
                return False

        stack_empty = len(stack) == 0

        return stack_empty
```

## 5. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
Given the heads of 2 sorted linked list, merge them into 1 sorted linked list.

### Approach
Compare individual elements of the two linked lists using pointers. Move the pointer to the next element of a linked list once an item from that linked list is inserted into the final sorted linked list. 

### Code 
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:

        list1_is_empty = list1 == None
        list2_is_empty = list2 == None

        if list1_is_empty and list2_is_empty:
            return None
        if list1_is_empty:
            return list2
        if list2_is_empty:
            return list1

        sorted_list = ListNode()
        head = sorted_list

        while (list1 is not None) and (list2 is not None):
            if list1.val < list2.val:
                sorted_list.next = list1
                list1 = list1.next
                sorted_list = sorted_list.next
            else:
                sorted_list.next = list2
                list2 = list2.next
                sorted_list = sorted_list.next

        if list1 is None:
            sorted_list.next = list2
        else:
            sorted_list.next = list1

        return head.next
```

Approach for sorting both linked lists by adding elements from the second linked list into the first (opposed to keeping a separate linked list for storing the sorted elements).

```
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:

        list1_is_empty = list1 == None
        list2_is_empty = list2 == None

        if list1_is_empty and list2_is_empty:
            return None
        if list1_is_empty:
            return list2
        if list2_is_empty:
            return list1

        final_head = list1
        prev = None

        while list1 is not None and list2 is not None:
            if list2.val < list1.val:
                temp = list2.next

                if prev is not None:
                    prev.next = list2
                else:
                    final_head = list2

                list2.next = list1
                prev = list2
                list2 = temp
            else:
                prev = list1
                list1 = list1.next

        if list1 is None:
            prev.next = list2
        else:
            prev.next = list1

        return final_head
```

## 6. [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
Given a sorted integer array, remove the duplicates in-place.

### Approach
Keep track of the index at which each insertion is to be made. Traverse through the array elements and insert an element if it is different from its predecessor.

### Code 
```
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        insertion_index = 0
        
        for i in range(1, len(nums)):
            if nums[i] != nums[i - 1]:
                insertion_index += 1
                nums[insertion_index] = nums[i]
                
        return insertion_index + 1

```

## 7. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
Given a string, check if it is a palindrome.

### Approach
Use two pointers starting from opposite ends of the string. Check character by character if the palindrome property is satisfied.

### Code 
```
class Solution:
    def isPalindrome(self, s: str) -> bool:
        
        s = s.lower()
        left_index = 0
        right_index = len(s) - 1

        while left_index < right_index:

            if not s[left_index].isalnum():
                left_index += 1
                continue

            if not s[right_index].isalnum():
                right_index -= 1
                continue
            
            if s[left_index] == s[right_index]:
                left_index += 1
                right_index -= 1
            else:
                return False

        return True
```

## 8. [Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)
Given two strings, return the index of the first occurence of the second string in the first string.

### Approach
Go over the first string element by element and check if it matches element by element with the second string.

### Code 
```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:

        if len(needle) == 0:
            return -1

        i = 0
        j = 0

        while i < len(haystack) and j < len(needle):
            if haystack[i] == needle[j]:
                if j == (len(needle) - 1):
                    return i - len(needle) + 1

                j += 1
                i += 1

            else:
                i -= j
                j = 0
                i += 1
    
        return -1
```

## 9. [Word Pattern](https://leetcode.com/problems/word-pattern/)
Given two strings, determine if the second string follows that same pattern as the first string.

### Approach
Go over both strings element by element and use a hashmap to keep track of the pattern being formed. If it is broken, return false.

### Code 
```
class Solution:
    def wordPattern(self, pattern: str, s: str) -> bool:
        words = s.split(" ")

        if len(pattern) != len(words):
            return False

        element_2_word = {}

        for pattern_element, word in zip(pattern, words):

            if pattern_element not in element_2_word.keys():

                if word not in element_2_word.values():
                    element_2_word[pattern_element] = word
                else:
                    return False

            else: 

                if element_2_word[pattern_element] != word:
                    return False
                else:
                    pass
        
        return True
```

## 10. [Summary Ranges](https://leetcode.com/problems/summary-ranges/)
Given a sorted unique integer array, return the smallest sorted list of inclusive ranges that cover all numbers in the array exactly.

### Approach
Iterate over the elements in the sorted array and keep building the range elements by checking for continuity between consecutive integer values.

### Code 
```
class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:

        if len(nums) == 0:
            return []

        ranges = []
        range_element = str(nums[0])

        for i in range(1, len(nums)):
            if nums[i] != nums[i - 1] + 1:

                if str(nums[i - 1]) != range_element:
                    range_element += f"->{nums[i - 1]}"

                ranges.append(range_element)
                range_element = str(nums[i])
                
        if nums[len(nums) - 1] == nums[len(nums) - 2] + 1:
            range_element += f"->{nums[len(nums) - 1]}"
        
        ranges.append(range_element)
        
        return ranges
```

## 11. [Same Tree](https://leetcode.com/problems/same-tree/)
Given the roots of two binary trees, determine if the trees are exactly the same or not.

### Approach
Do a recursive dfs traversal of the binary trees and at each step, check if the condition for equality is broken.

### Code 
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if p is None and q is None:
            return True
        elif p is None or q is None:
            return False
        else:
            if p.val == q.val:
                return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
            else:
                return False
        
        return True
```

## 12. [Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)
Given the root of a binary tree, return the averages values of the nodes at each level.

### Approach
Do a bfs traversal of the binary tree and calculate the average value at each level. The length of the bfs queue at different times can indicate the number of nodes at a certain level.

### Code 
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        bfs_queue = []
        bfs_queue.append(root)

        
        averages = []

        while len(bfs_queue) != 0:
            level_sum = 0

            for i in range(len(bfs_queue)):
                node = bfs_queue.pop(0)
                level_sum += node.val

                if node.left is not None:
                    bfs_queue.append(node.left)
                
                if node.right is not None:
                    bfs_queue.append(node.right)

            level_average = level_sum / (i+1)
            averages.append(level_average)
        
        return averages
```

## 13. [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)
Given an integer array that is sorted in ascending order, convert it into a height-balanced binary search tree.

### Approach
Given the ascending order of the array, the root for the tree will be the middle element. Subsequent subtree tree can be made using the same logic on only a part of the array. Use divide and conquer to build the binary search tree.

### Code 
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if len(nums) == 1:
            return TreeNode(nums[0])
        if len(nums) == 0:
            return None
        
        mid = len(nums) // 2
        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid + 1:])
        
        return root
```

## 14. [Search Insert Position](https://leetcode.com/problems/search-insert-position/)
Given a sorted array of distinct integers and a target, return the index if the target is found, else return the index where it would be if inserted in order.

### Approach
Perform binary search on the array to find the element. If not found, compare with the last element in the array with against which the item has not been checked to determine the insertion index.

### Code 
```
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        
        start_index = 0
        end_index = len(nums) - 1

        while end_index > start_index:
            middle_index = (end_index+start_index) // 2
            if target < nums[middle_index]:
                end_index = middle_index
            elif target > nums[middle_index]:
                start_index = middle_index + 1
            else:
                return middle_index
        
        if target <= nums[start_index]:
            return start_index
        else:
            return start_index + 1
```

## 15. [Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)
Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.

### Approach
The minimum absolute difference will be found between two consecutive values in the tree if the elements were sorted. An inorder traversal of a binary search tree visits the elements in sorted order. Use this idea to keep track of the minimum distance found so far during the search.

### Code 
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        prev_node = None
        min_difference = float("inf")

        def traverse_tree_inorder(node):
            nonlocal prev_node, min_difference

            if node.left is not None:
                traverse_tree_inorder(node.left)

            if prev_node is not None:
                min_difference = min(min_difference, abs(prev_node.val - node.val))
            
            prev_node = node
            
            if node.right is not None:
                traverse_tree_inorder(node.right)

            return
        
        traverse_tree_inorder(root)

        return min_difference
```

## 16. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
Given a binary representation of an unsigned integer, return the number of 1 bits it has.

### Approach
Involves using a numerical trick that counts that number of ones. The other option is to count bit by bit. That can be done by modding the number by 2 to check bit polarity and dividing by 2 to shift right.

### Code 
```
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0

        while n:
            n &= (n - 1)
            count += 1

        return count
```

## 17. [Plus One](https://leetcode.com/problems/plus-one/)
Given an integer represented as an array of digits, increment the resulting array of digits if the integer was incremented by 1.

### Approach
Loop over the array, chaging the digits as necessary of one was added to the digit. Break once there is no more carry.

### Code 
```
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        carry = True

        for i in range(len(digits)):
            if digits[len(digits) - (i+1)] == 9 and carry:
                digits[len(digits) - (i+1)] = 0
                carry = True
            elif carry:
                digits[len(digits) - (i+1)] += 1
                carry = False
                break
            else:
                digits[len(digits) - (i+1)] += 1
                carry = False
                break

        if carry:
            digits = [1] + digits
        
        return digits
```

## 18. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
You can climb either one or two steps at a time. Calculate the different ways to climb n steps.

### Approach
The problem follows the same pattern as fibonacci numbers. Add results for base cases and generate results for further cases using those from previous cases.

### Code 
```
class Solution:
    def climbStairs(self, n: int) -> int:
        steps_permutations = dict()
        steps_permutations[1] = 1
        steps_permutations[2] = 2

        for i in range(3, n + 1):
            steps_permutations[i] =  steps_permutations[i-1] + steps_permutations[i-2]

        return steps_permutations[n]
```

## 19. [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
You are given two integer arrays sorted in non-decreasing order. The length of the first array is that of the combined array. Add elements into the first array so the result is the sorted and combined array.

### Approach
Keep track of the index for each individual array while inserting and decrement the index for the array from which the insertion was made. Starting insertion from the end of the first array will ensure that you do not need to shift elements while insertion.

### Code 
```
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        i = m - 1
        j = n - 1
        k = m + n - 1

        while i >= 0 and j >= 0:
            if nums1[i] > nums2[j]:
                nums1[k] = nums1[i]
                i -= 1
                k -= 1
            else:
                nums1[k] = nums2[j]
                j -= 1
                k -= 1
        
        while j >= 0:
            nums1[k] = nums2[j]
            j -= 1
            k -= 1
```

## 20. [Remove Element](https://leetcode.com/problems/remove-element/)
Given an integer array and element, remove all occurences of the element from the array in-place.

### Approach
Using an insertion index, keep inserting elements into the array and skip when you encounter the target element.

### Code 
```
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        
        if len(nums) == 0:
            return 0
        
        insertion_index = 0

        for element in nums:
            if element != val:
                nums[insertion_index] = element
                insertion_index += 1
        
        return insertion_index
```

## 21. [Majority Element](https://leetcode.com/problems/majority-element/)
Given an integer array, return the majority element/ the element that occurs more than floor(n/2) times where n is the length the array.

### Approach
Use Moore's voting algorithm to keep a count and a candidate. Traverse through the array and increment or decrement the count if the same or a different element is encountered.

### Code 
```
class Solution:
    def majorityElement(self, nums: List[int]) -> int:

        candidate = None
        count = 0

        for number in nums:

            if count == 0:
                candidate = number
                count = 1  
            else:
                if number == candidate:
                    count += 1
                else:
                    count -= 1
        
        return candidate
```

# Medium Problems

## 1. [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
Given an array of integers and a target integer, return the minimum possible subarray size that sums to a value greater than or equal to the target integer.

### Approach
Use a sliding window to consider the possible subarrays and keep track of the minimum length found so far.

### Code 
```
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        left_index = 0
        right_index = 0
        subarray_sum = 0
        min_subarray_length = float("inf")

        while right_index < len(nums):
            subarray_sum += nums[right_index]

            while subarray_sum >= target:
                min_subarray_length = min(min_subarray_length, right_index - left_index + 1)
                subarray_sum -= nums[left_index]
                left_index += 1
            
            right_index += 1
        
        if min_subarray_length != float("inf"):
            return min_subarray_length
        else:
            return 0
```
