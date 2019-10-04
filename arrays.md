# Tasks with the sum
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
