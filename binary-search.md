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

# Search in Rotated Sorted Array
https://leetcode.com/problems/search-in-rotated-sorted-array/
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.empty())
            return -1;
        int right = nums.size() - 1, left = 0, middle;
        int shift = findShift(nums);
        while (right - left > 1) {
            middle = (left + right) / 2;
            if (nums[(shift + middle) % nums.size()] == target)
                return (shift + middle) % nums.size();
            else if (nums[(shift + middle) % nums.size()] < target)
                left = middle;
            else
                right = middle;
        }
        if (target != nums[(shift + left) % nums.size()] 
            && target != nums[(shift + right) % nums.size()])
            return -1;
        else
            return target == nums[(shift + left) % nums.size()] ? 
            (shift + left) % nums.size() : (shift + right) % nums.size();
    }
private:
    int findShift(vector <int>& nums) {
        int right = nums.size() - 1, left = 0, middle;
        if (nums[right] > nums[left])
            return left;
        while (right - left > 1) {
            middle = (left + right) / 2;
            if (nums[middle] < nums[0])
                right = middle;
            else
                left = middle;
        }
        if (nums[left] < nums[right])
            return left;
        else
            return right;
    }
};
```

# Find Minimum in Rotated Sorted Array
https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
``` c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int right = nums.size() - 1, left = 0, middle;
        if (nums[right] > nums[left])
            return nums[left];
        while (right - left > 1) {
            middle = (left + right) / 2;
            if (nums[middle] < nums[0])
                right = middle;
            else
                left = middle;
        }
        return nums[left] < nums[right] ? nums[left] : nums[right];
    }
};
```
