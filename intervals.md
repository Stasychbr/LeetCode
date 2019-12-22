# Interval tasks
+ [Non-overlapping Intervals](#non-overlapping-intervals)
+ [Merge Intervals](#merge-intervals)
+ [Insert Interval](#insert-interval)
## Non-overlapping Intervals
https://leetcode.com/problems/non-overlapping-intervals/

T∈O(n*log n)
``` C++
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.empty())
            return 0;
        multimap <int, int> ends;
        for (auto i: intervals)
            ends.insert(make_pair(i[1], i[0]));
        auto i = ends.begin();
        int lastEnd = i->first;
        int isolatedInts = 1;
        for (i++; i != ends.end(); i++) {
            if (i->second >= lastEnd) {
                isolatedInts++;
                lastEnd = i->first;
            }
        }
        return intervals.size() - isolatedInts;
    }
};
```

## Merge Intervals
https://leetcode.com/problems/merge-intervals/

T∈O(n*log n)
``` C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector <vector <int>> result;
        sort(intervals.begin(), intervals.end(), 
            [](const vector <int> &a, const vector <int> &b) {return a[0] < b[0];});
        for (int i = 0; i < intervals.size(); i++) {
            int begin = intervals[i][0], end = intervals[i][1];
            while (i < intervals.size() - 1 && end >= intervals[i + 1][0]) {
                i++;
                if (end <= intervals[i][1])
                    end = intervals[i][1];
            }
            result.push_back(vector <int> {begin, end});
        }
        return result;
    }
};
```

## Insert Interval
https://leetcode.com/problems/insert-interval/

T∈O(n)
``` C++
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        if (intervals.empty())
            return vector <vector <int>> {newInterval};
        for (auto i = intervals.begin(); i != intervals.end(); i++) {
            if ((*i)[0] > newInterval[0]) {
                intervals.insert(i, newInterval);
                break;
            }
            if (i == intervals.end() - 1) {
                intervals.push_back(newInterval);
                break;
            }
        }
        return merge(intervals);
    }
private:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector <vector <int>> result;
        for (int i = 0; i < intervals.size(); i++) {
            int begin = intervals[i][0], end = intervals[i][1];
            while (i < intervals.size() - 1 && end >= intervals[i + 1][0]) {
                i++;
                if (end <= intervals[i][1])
                    end = intervals[i][1];
            }
            result.push_back(vector <int> {begin, end});
        }
        return result;
    }
};
```
