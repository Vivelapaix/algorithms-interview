# String

+ [Longest Substring Without Repeating Characters](#longest-substring-without-repeating-characters)
+ [Longest Repeating Character Replacement](#longest-repeating-character-replacement)
+ [Minimum Window Substring](#minimum-window-substring)
+ [Group Anagrams](#group-anagrams)
+ [Valid Parentheses](#valid-parentheses)
+ [Generate Parentheses](#generate-parentheses)
+ [Valid Palindrome](#valid-palindrome)
+ [Longest Palindromic Substring](#longest-palindromic-substring)
+ [Palindromic Substrings](#palindromic-substrings)
+ [Is Subsequence](#is-subsequence)
+ [Valid Anagram](#valid-anagram)
+ [Find All Anagrams in a String](#find-all-anagrams-in-a-string)
+ [Permutation in String](#permutation-in-string)
+ [Reverse Words in a String](#reverse-words-in-a-string)

## Longest Substring Without Repeating Characters

https://leetcode.com/problems/longest-substring-without-repeating-characters/

Основная идея в том, чтобы запоминать последнюю позицию символа. Допустим, встретился символ, который уже есть в map.
Извлекаем индекс данного символа (мы запоминаем специально `j + 1`, т.е. следующая позиция). Индекс `i` служит для запоминания
последней позиции произведенного вычисления ответа. Например, `abcba`. Когда встретится `b`, вычислим длину строки `cb` (запоминали `j + 1`), 
когда встретится `a`, в `i` записана информация, с какого индекса надо сравнивать, т.е. `cba`. 

```java
public int lengthOfLongestSubstring(String s) {
    int n = s.length(), ans = 0;
    Map<Character, Integer> map = new HashMap<>(); // current index of character
    //int[] index = new int[128];
    // try to extend the range [i, j]
    for (int j = 0, i = 0; j < n; j++) {
        if (map.containsKey(s.charAt(j))) {
            i = Math.max(map.get(s.charAt(j)), i);
        }
        // i = Math.max(index[s.charAt(j)], i);
        ans = Math.max(ans, j - i + 1);
        map.put(s.charAt(j), j + 1);
    }
    return ans;
}
```

В предыдущем решении неявно используется окно, в котором происходит сравнение. 
Здесь же увеличиваем окно с уникальными символами (двигаем правую границу). Как только встретился повторяющийся символ, 
удаляем его слева и двигаем левую границу.

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int ans = 0, i = 0, j = 0;
        while (i < n && j < n) {
            // try to extend the range [i, j]
            if (!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j - i);
            }
            else {
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```

## Longest Repeating Character Replacement

https://leetcode.com/problems/longest-repeating-character-replacement/

Подсчитать количество вхождений наиболее часто встречающегося символа в окне, вычесть эту величину из размера окна.
Тем самым мы найдем наименьшее количество замен, которые необходимы, чтобы в данном окне все символы были одинаковыми.

Если вдруг `replaceCount > k`, то необходимо сдвинуть окно вправо и уменьшить `count`. Например, есть строка `ABAACA`. Окно есть `ABAAC`, мы его сдвигаем и уменьшаем `count`, чтобы получить окно `BAACA`.

```java
public int characterReplacement(String s, int k) {
    int maxCount = 0;
    int maxLength = 0;
    int[] count = new int[26];

    for (int left = 0, right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        maxCount = Math.max(maxCount, ++count[c - 'A']);
        int windowSize = right - left + 1;
        int replaceCount = windowSize - maxCount;
        if (replaceCount > k) {
            // invalid window
            count[s.charAt(left) - 'A']--;
            left++;
        } else {
            maxLength = Math.max(maxLength, windowSize);
        }
    }
    return maxLength;
}
```

## Minimum Window Substring

https://leetcode.com/problems/minimum-window-substring/

1. Вычислить количество повторений каждого символа в `t`.
2. Объявляем два указателя и начинаем двигаться.
3. На каждой итерации увеличиваем число повторений символа из `s`.
4. `formed` используется, чтобы сигнализировать, что в окно входят все символы из `t` с равным количеством повторений.
5. Как только строка сформирована, мы начинаем двигать левую границу `left`, уменьшая количество повторений символа из `s`. Если оказалось так, что число повторений символа не совпадает, то окно становится недействительным, поэтому начинаем сдвигать правую границу `right`.

```java
public String minWindow(String s, String t) {
    if (s == null || t == null ||
        s.length() == 0 || t.length() == 0) {
        return "";
    }

    Map<Character, Integer> charFreqInT = new HashMap<>();
    int count;
    for (int i = 0; i < t.length(); i++) {
        count = charFreqInT.getOrDefault(t.charAt(i), 0);
        charFreqInT.put(t.charAt(i), count + 1);
    }

    int left = 0;
    int right = 0;
    Map<Character, Integer> charFreqInWindow = new HashMap<>();

    int required = charFreqInT.size();
    int formed = 0;

    int[] ans = {Integer.MAX_VALUE, 0, 0};
    // 0 - length, 1 - leftIndex, 2 - rightIndex

    while (right < s.length()) {
        char c = s.charAt(right);

        count = charFreqInWindow.getOrDefault(c, 0);
        charFreqInWindow.put(c, count + 1);

        if (charFreqInT.containsKey(c) &&
            charFreqInT.get(c).intValue() == charFreqInWindow.get(c).intValue()) {
            formed++;
        }

        while (left <= right && formed == required) {
            if (right - left + 1 < ans[0]) {
                ans[0] = right - left + 1;
                ans[1] = left;
                ans[2] = right;
            }

            c = s.charAt(left);
            left++;

            charFreqInWindow.put(c, charFreqInWindow.get(c) - 1);
            if (charFreqInT.containsKey(c) &&
                charFreqInWindow.get(c) < charFreqInT.get(c)) {
                formed--;
            }
        }
        right++;
    }

    return ans[0] == Integer.MAX_VALUE ? "" : s.substring(ans[1], ans[2] + 1);
}
```

## Group Anagrams

https://leetcode.com/problems/group-anagrams/

1. Можно сортировать каждую строку и использовать отсортированный вид для ключа в словаре. В итоге в значениях будут анаграммы.
2. Зная, что алфавит состоит только из 26 символов, можно ускорить решение. Будем подсчитывать в каждой строке частоту встречаемости каждой буквы, а потом из этих частот формировать ключ для словаря.

Solution 1:
```java
public List<List<String>> groupAnagrams(String[] strs) {
    int[] freq = new int[26];
    Map<String, List<String>> res = new HashMap<>();

    for (String s : strs) {
        // first solution
        // char[] ca = s.toCharArray();
        // Arrays.sort(ca);
        // String key = String.valueOf(ca);
    
        Arrays.fill(freq, 0);
        for (char c : s.toCharArray()) {
            freq[c - 'a']++;
        }

        StringBuilder sb = new StringBuilder("");
        for (int i = 0; i < 26; i++) {
            sb.append('#').append(freq[i]);
        }

        String key = sb.toString();
        if (!res.containsKey(key)) {
            res.put(key, new ArrayList<>());
        }

        res.get(key).add(s);
    }

    return new ArrayList(res.values());
}
```

Solution 2:
```java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> result = new HashMap<>();
    for (String str : strs) {
        String key = getSortedStringOptimized(str);
        if (result.containsKey(key)) {
            result.get(key).add(str);
        } else {
            List<String> values = new ArrayList<>();
            values.add(str);
            result.put(key, values);
        }
    }
    return new ArrayList<>(result.values());
}

private String getSortedString(String str) {
    char[] ch = str.toCharArray();
    Arrays.sort(ch);
    return new String(ch);
}

private String getSortedStringOptimized(String str) {
    int[] freqs = new int[26];
    // Arrays.fill(freq, 0);

    for (char c : str.toCharArray()) {
        freqs[c - 'a']++;
    }

    StringBuilder sb = new StringBuilder("");
    for (int i = 0; i < 26; i++) {
        sb.append('#').append(freqs[i]);
    }
    return sb.toString();
}
```

## Valid Parentheses

https://leetcode.com/problems/valid-parentheses/

```java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    Map<Character, Character> mapping = new HashMap<>();
    mapping.put('}', '{');
    mapping.put(']', '[');
    mapping.put(')', '(');

    for (char c : s.toCharArray()) {
        if (mapping.containsKey(c)) {
            char topElement = stack.isEmpty() ? '#' : stack.pop();
            if (topElement != mapping.get(c)) {
                return false;
            }
        } else {
            stack.push(c);
        }
    }
    return stack.isEmpty();
}
```

## Generate Parentheses

https://leetcode.com/problems/generate-parentheses/

```java
public List<String> generateParenthesis(int n) {
    List<String> ans = new ArrayList();
    backtrack(ans, "", 0, 0, n);
    return ans;
}

public void backtrack(List<String> ans, String cur, int open, int close, int max){
    if (cur.length() == max * 2) {
        ans.add(cur);
        return;
    }

    if (open < max)
        backtrack(ans, cur + "(", open + 1, close, max);
    if (close < open)
        backtrack(ans, cur + ")", open, close + 1, max);
}
```

или вот так

```java
public List<String> generateParenthesis(int n) {
    char[] str = new char[n * 2];
    List<String> ans = new ArrayList<>();
    backtrack(ans, str, n, n, 0);
    return ans;
}

public void backtrack(List<String> ans, char[] str, int open, int close, int count) {
    if (open < 0 || close < open) return;
    
    if (open == 0 && close == 0) {
        ans.add(String.copyValueOf(str));
        return;
    }

    if (open > 0) {
        str[count] = '(';
        backtrack(ans, str, open - 1, close, count + 1);
    }
    if (close > open) {
        str[count] = ')';
        backtrack(ans, str, open, close - 1, count + 1);
    }
}
```

## Valid Palindrome

https://leetcode.com/problems/valid-palindrome/

```java
private static boolean isAlphaNumeric(char c) {
    return (c >= 'a' && c <= 'z') ||
            (c >= 'A' && c <= 'Z') ||
            (c >= '0' && c <= '9');
}

private static int toLowerCase(char c) {
    return c >= 'A' && c <= 'Z' ? c + ('a' - 'A') : c;
}

public boolean isPalindrome(String s) {
    if (s == null || s.isEmpty() || s.length() == 1) {
        return true;
    }

    int i = 0;
    int j = s.length() - 1;
    while (i < j) {
        if (!isAlphaNumeric(s.charAt(i))) {
            i++;
        }
        else if (!isAlphaNumeric(s.charAt(j))) {
            j--;
        }
        else if (toLowerCase(s.charAt(i)) != toLowerCase(s.charAt(j))) {
            return false;
        }
        else {
            i++;
            j--;
        }
    }

    return i >= j;
}
```

## Longest Palindromic Substring

https://leetcode.com/problems/longest-palindromic-substring/

```java
public String longestPalindrome(String s) {
    if (s == null || s.length() < 1) return "";

    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);
        int len2 = expandAroundCenter(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start + 1) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    while (left >= 0 && right < s.length()
            && s.charAt(left) == s.charAt(right)) {
        left--;
        right++;
    }
    return right - left - 1;
}
```

## Palindromic Substrings

https://leetcode.com/problems/palindromic-substrings/

```java
public int countSubstrings(String s) {
    if (s == null) return 0;

    int count = 0;
    for (int i = 0; i < s.length(); i++) {
        count += expandAroundCenter(s, i, i);
        count += expandAroundCenter(s, i, i + 1);
    }
    return count;
}

private static int expandAroundCenter(String s, int left, int right) {
    int count = 0;
    while (left >= 0 && right < s.length()
            && s.charAt(left) == s.charAt(right)) {
        count++;
        left--;
        right++;
    }
    return count;
}
```

Либо такое решение

```java
public int countSubstrings(String S) {
    int N = S.length(), ans = 0;
    for (int center = 0; center <= 2*N-1; ++center) {
        int left = center / 2;
        int right = left + center % 2;
        while (left >= 0 && right < N && S.charAt(left) == S.charAt(right)) {
            ans++;
            left--;
            right++;
        }
    }
    return ans;
}
```

## Is Subsequence

https://leetcode.com/problems/is-subsequence/

```java
public boolean isSubsequence(String s, String t) {
    if (s == null || t == null) return false;

    int j = 0;
    for (int i = 0; i < t.length() && j < s.length(); i++) {
        if (t.charAt(i) == s.charAt(j)) {
            j++;
        }
    }
    return j >= s.length();
}
```


## Valid Anagram

https://leetcode.com/problems/valid-anagram/

```java
public boolean isAnagram(String s, String t) {
    return getSortedStringOptimized(s).equals(getSortedStringOptimized(t));
}

public String getSortedString(String str){
    char[] strChars = str.toCharArray();
    Arrays.sort(strChars);
    return new String(strChars);
}

public String getSortedStringOptimized(String str){
    int[] freqs = new int[26];
    for (char c : str.toCharArray()) {
        freqs[c - 'a']++;
    }

    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < 26; i++) {
        sb.append('#').append(freqs[i]);
    }
    return sb.toString();
}
```


## Find All Anagrams in a String

https://leetcode.com/problems/find-all-anagrams-in-a-string/

```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> result = new ArrayList<>();
    int[] pFreq = getCharFreqs(p, 0, p.length());
    int[] sFreq = null;
    for (int i = 0; i < s.length() && i + p.length() - 1 < s.length(); i++) {
        if (isFreqEquals(getCharFreqs(s, i, i + p.length()), pFreq)) {
            result.add(i);
        }
    }
    return result;
}

public int[] getCharFreqs(String s, int start, int end){
    int[] freqs = new int[26];

    for (int i = start; i < s.length() && i < end; i++) {
        freqs[s.charAt(i) - 'a']++;
    }
    return freqs;
}

public boolean isFreqEquals(int[] sFreq, int[] pFreq) {
    for (int i = 0; i < sFreq.length; i++) {
        if (sFreq[i] != pFreq[i]) {
            return false;
        }
    }
    return true;
}
```

## Permutation in String

https://leetcode.com/problems/permutation-in-string/

```java
public boolean checkInclusion(String pattern, String str) {

    Map<Character, Integer> map = new HashMap<>();
    for (char c : pattern.toCharArray())
        map.put(c, map.getOrDefault(c, 0) + 1);

    int matched = 0;
    int start = 0, end = 0;
    while (end < str.length()) {
        char c = str.charAt(end);
        if (map.containsKey(c)) {
            map.put(c, map.get(c) - 1);
            if (map.get(c) == 0) matched += 1;
        }

        if (matched == map.size()) return true;

        if (end - start + 1 == pattern.length()) {
            char startChar = str.charAt(start);
            if (map.containsKey(startChar)) {
                if (map.get(startChar) == 0) matched--;
                map.put(startChar, map.get(startChar) + 1);
            }
            start += 1;
        }
        end += 1;
    }
    return false;
}
```

## Reverse Words in a String

https://leetcode.com/problems/reverse-words-in-a-string/

```java
public String reverseWords(String s) {
    StringBuilder sb = new StringBuilder();
    int i = s.length() - 1, wordStart = 0;
    boolean isWordBefore = false;
    while (i >= 0) {
        if (!isWordPart(s.charAt(i))) {
            i--;
            continue;
        }

        if (isWordBefore) {
            sb.append(" ");
            isWordBefore = false;
        }
        wordStart = passWord(s, i);
        sb.append(s.substring(wordStart + 1, i + 1));
        if (i - wordStart > 0) {
            isWordBefore = true;
        }
        i = wordStart;
    }

    return sb.toString();
}

public int passWord(String s, int start) {
    int i = start;
    while (i >= 0 && isWordPart(s.charAt(i))) {
        i--;
    }
    return i;
}

public boolean isWordPart(char t) {
    return t >= 'A' && t <= 'Z' || t >= 'a' && t <= 'z' || t >= '0' && t <= '9';
}
```
