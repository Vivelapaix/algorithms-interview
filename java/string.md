# String

+ [Longest Substring Without Repeating Characters](#longest-substring-without-repeating-characters)
+ [Longest Repeating Character Replacement](#longest-repeating-character-replacement)
+ [Minimum Window Substring](#minimum-window-substring)

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
