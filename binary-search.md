# Binary Search
https://leetcode.com/problems/binary-search/
``` c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int right = nums.size() - 1, left = 0, middle;
        while (right - left > 1) {
            middle = (left + right) / 2;
            if (nums[middle] == target)
                return middle;
            else if (nums[middle] < target)
                left = middle;
            else
                right = middle;
        }
        if (target != nums[left] && target != nums[right])
            return -1;
        else
            return target == nums[left] ? left : right;
    }
};
```
