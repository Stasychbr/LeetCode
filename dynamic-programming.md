# Dynamic programming

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

# Coin Change
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

# Longest Increasing Subsequence
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

# Longest Common Subsequence
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

# Unique Paths
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

# Unique Paths II
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
