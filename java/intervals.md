# Intervals

+ [Merge Intervals](#merge-intervals)

## Merge Intervals

https://leetcode.com/problems/merge-intervals/

```java
public class Interval {
    int start;
    int end;

    public Interval(int start, int end) {
        this.start = start;
        this.end = end;
    }
}

public class IntervalComparator implements Comparator<Interval> {
    @Override
    public int compare(Interval a, Interval b) {
        return a.start < b.start ? -1 : a.start == b.start ? 0 : 1;
    }
}

public int[][] merge(int[][] intervals) {
    List<Interval> lst = new ArrayList<>();
    for (int[] interval : intervals) {
        lst.add(new Interval(interval[0], interval[1]));
    }

    List<Interval> mergedIntervals = mergeIntervals(lst);
    int[][] res = new int[mergedIntervals.size()][2];
    int i = 0;
    for (Interval interval : mergedIntervals) {
        res[i][0] = interval.start;
        res[i][1] = interval.end;
        i++;
    }
    return res;
}

private List<Interval> mergeIntervals(List<Interval> intervals) {
    Collections.sort(intervals, new IntervalComparator());
    LinkedList<Interval> merged = new LinkedList<>();
    for (Interval interval : intervals) {
        if (merged.isEmpty() || merged.getLast().end < interval.start) {
            merged.add(interval);
        } else {
            merged.getLast().end = Math.max(merged.getLast().end, interval.end);
        }
    }
    return merged;
}
```
