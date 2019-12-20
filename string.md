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
