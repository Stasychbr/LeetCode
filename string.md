# Longest Substring Without Repeating Characters
https://leetcode.com/problems/longest-substring-without-repeating-characters/
```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int curMax = 0;
        unordered_map <char, int> slidingWindow;
        int i = 0, j = 0;
        while (i < s.length() && j < s.length()) {
            if (slidingWindow.find(s[j]) == slidingWindow.end()) {
                slidingWindow[s[j]] = j;
                j++;
                curMax = max(curMax, j - i);
            }
            else {
                slidingWindow.erase(s[i]);
                i++;
            }
        }
        return curMax;
    }
};
```

# Longest Repeating Character Replacement
https://leetcode.com/problems/longest-repeating-character-replacement/
```c++
class Solution {
public:
    int characterReplacement(string s, int k) {
        int curMax = 0;
        int i = 0;
        int count['Z' - 'A' + 1] = {0};
        int result = 0;
        for (int j = 0; j < s.size(); j++) {
          int subStringLength = j - i + 1;
          curMax = max(curMax, ++count[s[j] - 'A']);
          if (subStringLength - curMax > k) {
            count[s[i++] - 'A']--;
          } else {
            result = max(result, subStringLength);
          }
        }
        return result;
    }
};
```

# Minimum Window Substring
https://leetcode.com/problems/minimum-window-substring/
```c++
class Solution {
public:
    string minWindow(string s, string t) {
        if (s.empty() || t.empty() || s.length() < t.length()) {
            return "";
        }
        int left = 0, right = 0;
        int result[2] = {0, -1};
        map <char, int> alphabet;
        for (int i = 0; i < t.length(); i++) {
            alphabet[t[i]]++;
        }
        int formedLetters = 0;
        map <char, int> window;
        while (right < s.length()) {
            window[s[right]]++;
            if (alphabet.find(s[right]) != alphabet.end()) {
                if (alphabet[s[right]] == window[s[right]]) {
                    formedLetters++;
                }
            }
            while (left <= right && formedLetters == alphabet.size()) {
                if (result[1] == -1 || right - left + 1 < result[1]) {
                    result[1] = right - left + 1;
                    result[0] = left;
                }
                window[s[left]]--;
                if (alphabet.find(s[left]) != alphabet.end()) {
                    if (window[s[left]] < alphabet[s[left]]) {
                        formedLetters--;    
                    }
                }
                left++;
            }
            right++;
        }
        return result[1] == -1 ? "" : s.substr(result[0], result[1]);
    }
};
```

# Group Anagrams
https://leetcode.com/problems/group-anagrams/
```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
         if (strs.empty()) {
             return {};
         }
        unordered_map <string, vector <string>> table;
        for (string cur: strs) {
            string sorted(cur);
            sort(sorted.begin(), sorted.end());
            table[sorted].push_back(cur);
        }
        vector <vector<string>> result;
        for (auto i: table)
            result.push_back(i.second);
        return result;
    }
};
```

# Valid Parentheses 
https://leetcode.com/problems/valid-parentheses/
```c++
class Solution {
public:
    bool isValid(string s) {
        int bracketNumber[3][2] = {{0}};
        stack <char> openBrackets;
        for (char ch: s) {
            if (ch != '(' && ch != ')' 
                && ch != '{' && ch != '}'
                && ch != '[' && ch != ']') {
                continue;
            }
            if (ch == '(' || ch == ')') {
                if (ch == ')') {
                    if (openBrackets.empty() || openBrackets.top() != '(')
                        return false;
                    bracketNumber[0][1]++;
                    openBrackets.pop();
                }
                else {
                    openBrackets.push(ch);
                    bracketNumber[0][0]++;
                }
            }
            if (ch == '{' || ch == '}') {
                if (ch == '}') {
                    if (openBrackets.empty() || openBrackets.top() != '{')
                        return false;
                    bracketNumber[1][1]++;
                    openBrackets.pop();
                }
                else {
                    openBrackets.push(ch);
                    bracketNumber[1][0]++;
                }
            }
            if (ch == '[' || ch == ']') {
               if (ch == ']') {
                    if (openBrackets.empty() || openBrackets.top() != '[')
                        return false;
                    bracketNumber[2][1]++;
                   openBrackets.pop();
                }
                else {
                    openBrackets.push(ch);
                    bracketNumber[2][0]++;
                }
            }
        }
        if (bracketNumber[0][0] != bracketNumber[0][1] ||
                bracketNumber[1][0] != bracketNumber[1][1] ||
                bracketNumber[2][0] != bracketNumber[2][1]) {
                return false;    
            }
        return true;
    }
};
```

# Generate Parentheses
https://leetcode.com/problems/generate-parentheses/
```c++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector <string> result;
        buildSequence(result, "", 0, 0, n);
        return result;
    }
private:
    void buildSequence(vector <string>& result, string current, int openNum, int closeNum, int n) {
        if (current.length() == 2 * n) {
            result.push_back(current);
            return;
        }
        if (openNum < n) {
            buildSequence(result, current + '(', openNum + 1, closeNum, n);
        }
        if (closeNum < openNum) {
            buildSequence(result, current + ')', openNum, closeNum + 1, n);
        }
    }
};
```

# Valid Palindrome 
https://leetcode.com/problems/valid-palindrome/
```c++
class Solution {
public:
    bool isPalindrome(string s) {
        int i = 0;
        int j = s.length() - 1;
        for (int i = 0; s[i]; i++) {
            s[i] = tolower(s[i]);
        }
        while (i < j) {
            if (!isalnum(s[i]) || !isalnum(s[j])) {
                isalnum(s[i]) ? j-- : i++;
                continue;
            }
            if (s[i] != s[j]) {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
};
```

# Longest Palindromic Substring
https://leetcode.com/problems/longest-palindromic-substring/
```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int maxLength = 0;
        if (s.empty())
            return "";
        int palStart = 0, palEnd = 0;
        for (int i = 0; i < s.length(); i++) {
            int palLenght1 = findPalindrome(s, i, i + 1);
            int palLenght2 = findPalindrome(s, i, i);
            int curMax = max(palLenght1, palLenght2);
            if (curMax > maxLength) {
                palStart = i - (curMax - 1) / 2;
                maxLength = curMax;
            }
        }
        return s.substr(palStart, maxLength);
    }
private:
    int findPalindrome(string s, int left, int right) {
        int curLeft = left, curRight = right;
        while (curLeft >= 0 && curRight < s.length() && s[curLeft] == s[curRight]) {
            curLeft--;
            curRight++;
        }
        return curRight - curLeft - 1;
    }
};
```

# Is Subsequence
https://leetcode.com/problems/is-subsequence/
```c++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int lastPosition = -1;
        for (int i = 0; i < s.length(); i++) {
            bool flag = false;
            for (int j = lastPosition + 1; j < t.length(); j++) {
                if (s[i] == t[j]) {
                    lastPosition = j;
                    flag = true;
                    break;
                }
            }
            if (!flag) {
                return false;
            }
        }
        return true;
    }
};
```

# Palindromic Substrings
https://leetcode.com/problems/palindromic-substrings/
```c++
class Solution {
public:
    int countSubstrings(string s) {
        int result = 0;
        for (int i = 0; i < s.length(); i++) {
            int palNum1 = findPalindrome(s, i, i);
            int palNum2 = findPalindrome(s, i, i + 1);
            result += palNum1 + palNum2;
        }
        return result;
    }
private:
    int findPalindrome(string s, int left, int right) {
        int curLeft = left, curRight = right;
        int palindromeNumber = 0;
        while (curLeft >= 0 && curRight < s.length() && s[curLeft] == s[curRight]) {
            curLeft--;
            curRight++;
            palindromeNumber++;
        }
        return palindromeNumber;
    }
};
```
