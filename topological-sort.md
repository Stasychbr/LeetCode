# Topological sort (devil incarnate)
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
