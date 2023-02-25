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
