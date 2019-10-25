# Fibonacci numbers
https://leetcode.com/problems/fibonacci-number
## Recursive method
T∈O(φ^n)
``` C++
class Solution {
public:
    int fib(int N) {
        return SlowRecu(N);
    }
private:
    int SlowRecu(int n) {
        if (n <= 1)
            return n;
        else
            return SlowRecu(n - 2) + SlowRecu(n - 1);
    }
};
```
## Recursive with memoization
T∈O(n)
``` C++
class Solution {
public:
    int fib(int N) {
        int a[N + 1];
        if (!N)
            return 0;
        else 
            MemoRecu(N, a);
        return a[N];
    }
private:
    void MemoRecu(int n, int* a) {
        if (n == 1) {
            a[0] = 0;
            a[1] = 1;
            return;
        }
        MemoRecu(n - 1, a); 
        a[n] = a[n - 2] + a[n - 1];
    }
};
```
## Iterative method
T∈O(n)
``` C++
class Solution {
public:
    int fib(int N) {
        return IterativeMeth(N);
    }
private:
    int IterativeMeth(int n) {
        int seq[2] = {0, 1}, i;
        for (i = 2; i <= n; i++) {
            seq[i % 2] = seq[0] + seq[1];
        }
        return seq[n % 2];
    }
};
```
## Pavel Olegovich's method
T∈O(log n)
``` C++
class Solution {
public:
    int fib(int N) {
        return PaulsHeritage(N);
    }
private:
    int PaulsHeritage(int n) {
        int p = 0, q = 1, r;
        int a = 0, b = 1, c;
        if (n <= 0)
            return a;
        else
            n--;
        while (n) {
            if (n % 2) {
              c = (a + b) * q + b * p;
              a = a * p + b * q;
              b = c;
            }
            r = (q + 2 * p) * q;
            p = p * p + q * q;
            q = r;
            n /= 2;
        }
        return b;
   }
};
```
