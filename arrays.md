# Tasks with the arrays
+ [Two Sum](#two-sum)
+ [Three Sum](#three-sum)
+ [Subarray Sum Equals K](#subarray-sum-equals-k)
+ [Maximum Subarray](#maximum-subarray)
+ [Maximum Product Subarray](#maximum-product-subarray)
## Two sum
https://leetcode.com/problems/two-sum/
### Dumb solution 
T∈O(n^2)
``` C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector <int> result(0);
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                if (nums[i] + nums[j] == target) {
                    result.push_back(i);
                    result.push_back(j);
                    return result;
                }
            }
        }
        return result;
    }
};
```
### Solution with STL map
T∈O(n*log n)
``` C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector <int> result(0);
        map <int, int> vectMap;
        for (int i = 0; i < nums.size(); i++) {
            if (vectMap.find(target - nums[i]) == vectMap.end())
                vectMap[nums[i]] = i;
            else {
                result.push_back(vectMap[target - nums[i]]);
                result.push_back(i);
                break;
            }
        }
        return result;
    }
};
```

## Three sum
https://leetcode.com/problems/3sum/

T∈O(n^2)
``` C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector <vector <int>> result;
        if (nums.empty())
            return result;
        vector <int>:: iterator left = nums.begin(), right = nums.end() - 1, inside;
        int triplet;
        sort(nums.begin(), nums.end());
        for (left = nums.begin(); left + 2 < nums.end(); left++) {
            if (left != nums.begin() && *left == *(left - 1))
                continue;
            inside = left + 1;
            right = nums.end() - 1;
            while (inside < right) {
                triplet = *left + *inside + *right;
                if (triplet == 0) {
                    result.push_back(vector <int> {*left, *inside, *right});
                    while (inside < right && *inside == *(inside + 1))
                        inside++;
                    while (inside < right && *right == *(right - 1))
                        right--;   
                    right--;
                    inside++;
                }
                else if (triplet < 0)
                    inside++;
                else
                    right--;
            }
        }
        return result;
    }
};
```

## Subarray Sum Equals K
https://leetcode.com/problems/subarray-sum-equals-k/

T∈O(n*log n)
``` C++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int sum = 0, result = 0;
        map <int, int> sumMap;
        sumMap[0] = 1;
        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i];
            if (sumMap.find(sum - k) != sumMap.end())
                result += sumMap[sum - k];
            sumMap[sum]++;
        }
        return result;
    }
};
```

# Maximum Subarray
https://leetcode.com/problems/maximum-subarray
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.empty())
            return 0;
        int curSum = nums[0], maxSum = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            curSum = max(nums[i] + curSum, nums[i]);
            maxSum = max(maxSum, curSum);
        }
        return maxSum;
    }
};
```

## Maximum Product Subarray
https://leetcode.com/problems/maximum-product-subarray/
```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        if (nums.empty())
            return 0;
        int res = nums[0];
        int maxProd = 1;
        int minProd = 1;
        for (int i = 0; i < nums.size(); i++) {
            int tmp = minProd;
            minProd = min(min(minProd * nums[i], maxProd * nums[i]), nums[i]);
            maxProd = max(max(tmp * nums[i], maxProd * nums[i]), nums[i]);
            res = max(maxProd, res);
        }
        return res;
    }
};
```
