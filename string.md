# Valid Palindrome 
https://leetcode.com/problems/valid-palindrome/
```c++
class Solution {
public:
    bool isPalindrome(string s) {
        int i = 0;
        int j = s.length() - 1;
        while (i < j) {
            if (!isalnum(s[i]) || !isalnum(s[j])) {
                isalnum(s[i]) ? j-- : i++;
                continue;
            }
            if (tolower(s[i]) != tolower(s[j])) {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
};
```
