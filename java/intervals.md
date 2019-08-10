# Intervals

+ [Insert Interval](#insert-interval)
+ [Merge Intervals](#merge-intervals)
+ [Non-overlapping Intervals](#non-overlapping-intervals)

## Insert Interval

https://leetcode.com/problems/insert-interval/

```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    int i = 0;
    List<int[]> res = new ArrayList<>();

    if (intervals.length == 0 || newInterval[0] < intervals[0][0]) {
        res.add(newInterval);
    }

    for (i = 0; i < intervals.length; i++) {
        if (newInterval[0] < intervals[i][0]) {
            break;
        }
        res.add(intervals[i]);
    }

    addInterval(res, newInterval);

    for (; i < intervals.length; i++) {
        addInterval(res, intervals[i]);
    }
    return res.toArray(new int[res.size()][2]);
}

private void addInterval(List<int[]> intervals, int[] newInterval) {
    if (intervals.get(intervals.size() - 1)[1] < newInterval[0]) {
        intervals.add(newInterval);
    } else {
        intervals.get(intervals.size() - 1)[1] = 
            Math.max(intervals.get(intervals.size() - 1)[1], newInterval[1]);
    }
}
```

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

## Non-overlapping Intervals

https://leetcode.com/problems/non-overlapping-intervals/

```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) return 0;

    Arrays.sort(intervals, new Comparator<int[]>(){
        @Override
        public int compare(int[] a, int[] b){
            return a[1]-b[1];
        }
    });
    //Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

    int prevIntervalEnd = intervals[0][1], countNonOverlap = 1;
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= prevIntervalEnd) {
            countNonOverlap++;
            prevIntervalEnd = intervals[i][1];
        }
    }
    return intervals.length - countNonOverlap;
}
```
