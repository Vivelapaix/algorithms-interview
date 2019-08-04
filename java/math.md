# Math

+ [Count Primes](#count-primes)

## Count Primes

https://leetcode.com/problems/count-primes/

Пометить все составные числа в промежутке, далее подсчитать количество оставшихся простых.

```java
public int countPrimes(int n) {
    if (n < 2) return 0;

    boolean[] isComposite = new boolean[n];
    int count = 0;
    for (int i = 2; i * i < n; i++) {
        if (!isComposite[i]) {
            for (int j = i; j * i < n; j++) {
                isComposite[i * j] = true;
            }
        }
    }

    for (int i = 2; i < n; i++) {
        if (!isComposite[i]) {
            count++;
        }
    }
    return count;
}
```

Либо вот так

```java
public int countPrimes(int n) {
    if (n < 2) return 0;

    boolean[] isComposite = new boolean[n];
    int count = 0;
    for (int i = 2; i < n; i++) {
        if (!isComposite[i]) {
            count++;
            for (int j = 2; j * i < n; j++) {
                isComposite[i * j] = true;
            }
        }
    }
    return count;
}
```
