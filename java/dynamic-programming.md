# Dynamic Programming

+ [Climbing Stairs](#climbing-stairs)
+ [Coin Change](#coin-change)
+ [Unique Paths](#unique-paths)
+ [Unique Paths 2](#unique-paths-ii)

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

## Unique Paths

https://leetcode.com/problems/unique-paths/

```java
public static int uniquePaths(int m, int n) {
    if (m == 0 || n == 0) return 1;

    int[][] paths = new int[m][n];

    for (int i = 0; i < n; i++) {
        paths[0][i] = 1;
    }

    for (int i = 0; i < m; i++) {
        paths[i][0] = 1;
    }

    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            paths[i][j] = paths[i - 1][j] + paths[i][j -1];
        }
    }

    return paths[m - 1][n - 1];
}
```

## Unique Paths 2

https://leetcode.com/problems/unique-paths-ii/

```java
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    if (obstacleGrid == null) return 0;

    int m = obstacleGrid.length;
    int n = obstacleGrid[0].length;

    if (obstacleGrid[0][0] == 1) {
        return 0;
    }

    obstacleGrid[0][0] = 1; // 1 path

    for (int i = 1; i < m; i++) {
        obstacleGrid[i][0] = (obstacleGrid[i][0] == 0
                && obstacleGrid[i - 1][0] == 1) ? 1 : 0;
    }

    for (int i = 1; i < n; i++) {
        obstacleGrid[0][i] = (obstacleGrid[0][i] == 0
                && obstacleGrid[0][i - 1] == 1) ? 1 : 0;
    }

    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (obstacleGrid[i][j] == 0) {
                obstacleGrid[i][j] =
                        obstacleGrid[i - 1][j] + obstacleGrid[i][j - 1];
            } else {
                obstacleGrid[i][j] = 0;
            }
        }
    }

    return obstacleGrid[m - 1][n - 1];
}
```
