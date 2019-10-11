# Interval tasks
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

