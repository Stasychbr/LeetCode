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
