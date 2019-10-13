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
