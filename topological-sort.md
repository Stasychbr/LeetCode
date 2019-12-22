# Topological sort tasks

+ [Course Schedule](#course-schedule)
+ [Course Schedule II](#course-schedule-ii)
+ [Alien Dictionary](#alien-dictionary)

## Course Schedule
https://leetcode.com/problems/course-schedule/

T∈O(n + m)
``` C++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector <vector <int>> graph(numCourses);
        int colors[numCourses] = {0};
        for (auto relation: prerequisites)
            graph[relation[0]].push_back(relation[1]);
        for (int i = 0; i < numCourses; i++) {
            if (colors[i] == 0) {
                if (!deepDarkSearch(graph, colors, i))
                    return false;
            }
        }
        return true;
    }
private:
    bool deepDarkSearch(vector <vector <int>>& graph, int* colors, int parent) {
        colors[parent] = 1;
        for (auto child: graph[parent]) {
            if (colors[child] == 0) {
                if (!deepDarkSearch(graph, colors, child))
                    return false;
            }
            else if (colors[child] == 1) 
                return false;
        }
        colors[parent] = 2;
        return true;
    }
};
```

## Course Schedule II
https://leetcode.com/problems/course-schedule-ii/

T∈O(n + m)
``` C++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector <vector <int>> graph(numCourses);
        vector <int> order;
        int colors[numCourses] = {0};
        for (auto relation: prerequisites)
            graph[relation[0]].push_back(relation[1]);
        for (int i = 0; i < numCourses; i++) {
            if (colors[i] == 0) {
                if (!deepDarkSearch(graph, colors, order, i))
                    return vector <int> {};
            }
        }
        return order;
    }
private:
    bool deepDarkSearch(vector <vector <int>>& graph, int* colors, vector <int>& route, int parent) {
        colors[parent] = 1;
        for (auto child: graph[parent]) {
            if (colors[child] == 0) {
                if (!deepDarkSearch(graph, colors, route, child))
                    return false;
            }
            else if (colors[child] == 1) 
                return false;
        }
        colors[parent] = 2;
        route.push_back(parent);
        return true;
    }
};
```

## Alien dictionary
### LintCode (without lexicographically sorting)
https://www.lintcode.com/problem/alien-dictionary

T∈O(n + m) 
``` C++
class Solution {
public:
    /**
     * @param words: a list of words
     * @return: a string which is correct order
     */
    string alienOrder(vector<string> &words) {
        vector <vector <int>> dependences(26);
        int colors[26] = {0};
        string alphabet;
        unordered_map <int, bool> mask;
        for (int i = 0; i < words.size() - 1; i++) {
            compareWords(words[i + 1], words[i], dependences);
            enlargeAlphabet(words[i], mask);
            enlargeAlphabet(words[i + 1], mask);
        }
        for (int i = 0; i < dependences.size(); i++) {
            if (colors[i] == 0) {
                if (!deepDarkSearch(dependences, colors, alphabet, mask, i))
                    return {};
            }
        }
        return alphabet;
    }
private:
    void enlargeAlphabet(string word, unordered_map <int, bool>& mask) {
        for (auto letter: word) {
            mask[letter - 'a'] = true;
        }
    }
    void compareWords(string word1, string  word2, vector <vector <int>>& dependences) {
        int length = word1.length() < word2.length() ? word1.length() : word2.length();
        for (int i = 0; i < length; i++) {
            if (word1[i] != word2[i]) {
                dependences[word1[i] - 'a'].push_back(word2[i] - 'a');
                break;
            }
        }
    }
    bool deepDarkSearch(vector <vector <int>>& graph, int* colors, string& route, unordered_map <int, bool>& mask, int parent) {
        colors[parent] = 1;
        for (auto child: graph[parent]) {
            if (colors[child] == 0) {
                if (!deepDarkSearch(graph, colors, route, mask, child))
                    return false;
            }
            else if (colors[child] == 1) 
                return false;
        }
        colors[parent] = 2;
        if (mask.find(parent) != mask.end())
            route.push_back(parent + 'a');
        return true;
    }
};
```
### LeetCode
https://leetcode.com/problems/verifying-an-alien-dictionary/
```c++
class Solution {
public:
    bool isAlienSorted(vector<string>& words, string order) {
        int alphabet[26];
        for (int i = 0; i < order.length(); i++)
            alphabet[order[i] - 'a'] = i;
        for (int i = 0; i < words.size() - 1; i++) {
            string word1 = words[i];
            string word2 = words[i + 1];
            int length = min(word1.length(), word2.length());
            int flag = 0;
            for (int j = 0; j < length; j++) {
                if (word1[j] != word2[j]) {
                    if (alphabet[word1[j] - 'a'] > alphabet[word2[j] - 'a']) {
                        return false;
                    }
                    flag = 1;
                    break;
                }
            }
            if (!flag && word1.length() > word2.length())
                return false;
        }
        return true;
    }
};
```
