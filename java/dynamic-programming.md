# Dynamic Programming

+ [Climbing Stairs](#climbing-stairs)
+ [Coin Change](#coin-change)

## Climbing Stairs

https://leetcode.com/problems/climbing-stairs/

```java
public int climbStairs(int n) {
    if (n == 1) {
        return 1;
    }
    int[] dp = new int[n + 1];
    dp[1] = 1;
    dp[2] = 2;
    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```

## Coin Change

https://leetcode.com/problems/coin-change/

Write a function to compute the fewest number of coins that you need to make up that amount.

```java
public int coinChange(int[] coins, int amount) {
    int[] coinCount = new int[amount + 1];
    Arrays.fill(coinCount, amount + 1);
    coinCount[0] = 0;

    for (int curAmount = 1; curAmount <= amount; curAmount++) {
        for (int coin : coins) {
            if (coin <= curAmount) {
                coinCount[curAmount] = Math.min(
                        coinCount[curAmount],
                        coinCount[curAmount - coin] + 1
                );
            }
        }
    }
    return coinCount[amount] > amount ? -1 : coinCount[amount];
}
```
