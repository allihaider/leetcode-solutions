# Easy Problems

## [Two Sum](https://leetcode.com/problems/two-sum/)
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

## [Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
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

## [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/description/)
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

## [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/submissions/)
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

## [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
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
