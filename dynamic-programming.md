# Dynamic programming

+ [Backpack](#backpack)
+ [Climbing Stairs](#climbing-stairs)
+ [Coin Change](#coin-change)
+ [Longest Increasing Subsequence](#longest-increasing-subsequence)
+ [Longest Common Subsequence](#longest-common-subsequence)
+ [Word Break](#word-break)
+ [Unique Paths](#unique-paths)
+ [Unique Paths II](#unique-paths-ii)
+ [Jump Game](#jump-game)
+ [Jump Game II](#jump-game-ii)
+ [House Robber](#house-robber)
+ [House Robber II](#house-robber-ii)
+ [Decode Ways](#decode-ways)
+ [Coin Change 2](#coin-change-2)

## Backpack
https://stepik.org/lesson/13259/step/5?unit=3444
```c++
#include <iostream>
#include <vector>

int main() {
  int W, n;
  std::cin >> W >> n;
  std::vector <std::vector<int>> maxValue(n + 1, std::vector<int>(W + 1));
  for (int i = 1; i <= n; i++) {
    int num;
    std::cin >> num;
    for (int j = 1; j <= W; j++) {
      if (num > j) {
        maxValue[i][j] = maxValue[i - 1][j];
      }
      else {
        maxValue[i][j] = std::max(maxValue[i - 1][j], maxValue[i - 1][j - num] + num);
      }
    }
  }
  std::cout << maxValue[n][W];
  return 0;
}
```

## Climbing Stairs
https://leetcode.com/problems/climbing-stairs/

### Perebor
T∈O(2^n)
Does not pass the test (n >= 44 => Time limit exeeding)
```c++
class Solution {
public:
    int climbStairs(int n) {
        int solutions = 0;
        recursiveThing(0, n, solutions);
        return solutions;
    }
private:
    void recursiveThing(int steps, int n, int& solutions) {
        if (steps >= n) {
            solutions += steps == n ? 1 : 0;
            return;
        }
        else {
            recursiveThing(steps + 1, n, solutions);
            recursiveThing(steps + 2, n, solutions);
        }    
    }
};
```

### Iterative
T∈O(n)
```c++
class Solution {
public:
    int climbStairs(int n) {
        if (n < 2)
            return 1;
        int solutions[n] = {1, 2};
        for (int i = 2; i < n; i++) {
            solutions[i] = solutions[i - 1] + solutions[i - 2];
        }
        return solutions[n - 1];
    }
};
```

## Coin Change
https://leetcode.com/problems/coin-change/
```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int max = amount + 1;
        vector <int> dp(max, max);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int j = 0; j < coins.size(); j++) {
                if (coins[j] <= i) {
                    dp[i] = min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
};
```

## Longest Increasing Subsequence
https://leetcode.com/problems/longest-increasing-subsequence/
```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.empty()) {
            return 0;
        }
        vector <int> dp(nums.size());
        int res = 1;
        dp[0] = 1;
        for (int i = 1; i < dp.size(); i++) {
            int maxVal = 0;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    maxVal = max(maxVal, dp[j]);
                }
            }
            dp[i] = maxVal + 1;
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

## Longest Common Subsequence
https://leetcode.com/problems/longest-common-subsequence/
```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.length() + 1, vector<int>(text2.length() + 1));
        for (int i = 1; i <= text1.length(); i++) {
            for (int j = 1; j <= text2.length(); j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                }
                else {
                    dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        return dp.back().back();
    }
};
```

## Word Break
https://leetcode.com/problems/word-break/
```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
        unordered_map<string, bool> memo;
        return wordBreakHelp(s, wordSet, memo);
    }
private:
    bool wordBreakHelp(string& s, unordered_set<string>& wordSet,
                       unordered_map<string, bool>& memo) {
        if (memo.find(s) != memo.end()) {
            return memo[s];
        }
        if (wordSet.find(s) != wordSet.end()) {
            return memo[s] = true;
        }
        for (int i = 1; i < s.size(); i++) {
            string rightPart = s.substr(i);
            if (wordSet.find(rightPart) == wordSet.end()) {
                continue;
            }
            string leftPart = s.substr(0, i);
            if (wordBreakHelp(leftPart, wordSet, memo)) {
                return memo[s] = true;
            }
        }
        return memo[s] = false;
    }
};
```
## Unique Paths
https://leetcode.com/problems/unique-paths/
```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector <vector <int>> ways(m + 1, vector <int> (n + 1));
        ways[1][1] = 1;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                ways[i][j] += ways[i - 1][j] + ways[i][j - 1];
            }
        }
        return ways[m][n];
    }
};
```

## Unique Paths II
https://leetcode.com/problems/unique-paths-ii/
```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int n = obstacleGrid[0].size() + 1, m = obstacleGrid.size() + 1; 
        vector <vector <unsigned int>> ways(m, vector <unsigned int> (n));
        ways[1][1] = 1 - obstacleGrid[0][0];
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i - 1][j - 1] == 1) {
                    continue;
                }
                ways[i][j] += ways[i - 1][j] + ways[i][j - 1];
            }
        }
        return ways[m - 1][n - 1];
    }
};
```

## Jump Game
https://leetcode.com/problems/jump-game/
```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        vector <bool> tries(nums.size());
        tries[nums.size() - 1] = true;
        for (int i = nums.size() - 2; i >= 0; i--) {
            int furthest = min(i + nums[i], (int)nums.size() - 1);
            for (int j = i + 1; j <= furthest; j++) {
                if (tries[j] == true) {
                    tries[i] = true;
                    break;
                }
            }
        }
        return tries[0];
    }
};
```

## Jump Game II
https://leetcode.com/problems/jump-game-ii/
```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        int result = 0, lastReached = 0;
        int canReach = 0;
        for (int i = 0; i < nums.size() - 1; i++) {
            if (lastReached >= nums.size() - 1) {
              break;
            }
            canReach = max(canReach, i + nums[i]);
            if (i == lastReached) {
                result++;
                lastReached = canReach;
            }
        }
        return result;
    }
};
```

## House Robber
https://leetcode.com/problems/house-robber/
```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int cur = 0, prev = 0;
        for (int i = 0; i < nums.size(); i++) {
            int tmp = prev;
            prev = cur;
            cur = max(tmp + nums[i], prev);
        }
        return cur;
    }
};
```

## House Robber II
https://leetcode.com/problems/house-robber-ii/
```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 1) {
            return nums[0];
        }
        return max(robCircle(nums, 0, nums.size() - 1), robCircle(nums, 1, nums.size()));
        
    }
private:
    int robCircle(vector<int>& nums, int start, int end) {
        int cur = 0, prev = 0;
        for (int i = start; i < end; i++) {
            int tmp = prev;
            prev = cur;
            cur = max(tmp + nums[i], prev);
        }
        return cur;
    }
};
```

## Decode Ways
https://leetcode.com/problems/decode-ways/
```c++
class Solution {
public:
    int numDecodings(string s) {
        int prev = 0, cur = 1;
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '0') {
                cur = 0;
            }
            int tmp = cur + prev;
            if (cur == 0 && prev == 0) {
                return 0;
            }
            if (s[i] == '1' || s[i] == '2' && s[i + 1] <= '6') {
                prev = cur;
            }
            else
                prev = 0;
            cur = tmp;
        }
        return cur;
    }
};
```

## Coin Change 2
https://leetcode.com/problems/coin-change-2/
```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector <int> vars(amount + 1);
        vars[0] = 1;
        for (int i = 0; i < coins.size(); i++) {
            for (int j = coins[i]; j <= amount; j++) {
                vars[j] += vars[j - coins[i]];
            }
        }
        return vars[amount];
    }
};
```
