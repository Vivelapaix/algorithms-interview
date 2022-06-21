# Two pointers

+ [Partition Labels](#partition-labels)

## Partition Labels

https://leetcode.com/problems/partition-labels/

```java
public List<Integer> partitionLabels(String s) {
    List<Integer> result = new ArrayList<>();
    int[] letterEndIndex = new int[26];
    for (int i = 0; i < s.length(); i++) {
        letterEndIndex[s.charAt(i) - 'a'] = i;
    }

    int wordStartIndex = 0;
    while (wordStartIndex < s.length()) {
        int wordEndIndex = letterEndIndex[s.charAt(wordStartIndex) - 'a'];
        for (int j = wordStartIndex; j <= wordEndIndex; j++) {
            wordEndIndex = Math.max(wordEndIndex, letterEndIndex[s.charAt(j) - 'a']);
        }
        result.add(wordEndIndex - wordStartIndex + 1);
        wordStartIndex = wordEndIndex + 1;
    }

    return result;
}
```
