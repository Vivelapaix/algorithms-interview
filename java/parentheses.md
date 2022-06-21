# Parenteses

+ [Remove Invalid Parentheses](#remove-invalid-parentheses)

## Remove Invalid Parentheses

https://leetcode.com/problems/remove-invalid-parentheses/

```java
public List<String> removeInvalidParentheses(String s) {
    Set<String> visited = new HashSet<>();
    Queue<String> q = new LinkedList<>();
    q.add(s);
    List<String> result = new ArrayList<>();

    while (!q.isEmpty()) {
        String cur = q.poll();

        if (isValid(cur)) {
            result.add(cur);
        }

        for (int i = 0; i < cur.length(); i++) {
            if (cur.charAt(i) != '(' &&  cur.charAt(i) != ')') {
                continue;
            }

            // remove bracket
            String first = cur.substring(0, i);
            String second = cur.substring(i + 1);
            String newCandidate = first + second;

            if (!visited.contains(newCandidate)) {
                if (result.isEmpty() || result.get(result.size() - 1).length() < newCandidate.length()) {
                    visited.add(newCandidate);
                    q.add(newCandidate);
                }
            }
        }

    }

    return result;
}

private boolean isValid(String s) {
    int n = 0;

    for (char c : s.toCharArray()) {
        if (c == '(') {
            n++;
        } else if (c == ')') {
            if (n == 0) {
                return false;
            }
            n--;
        }
    }
    return n == 0;
}
```
