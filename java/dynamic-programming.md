# Dynamic Programming

+ [Climbing Stairs](#climbing-stairs)
+ [Coin Change](#coin-change)
+ [Longest Increasing Subsequence](#longest-increasing-subsequence)
+ [Longest Common Subsequence](#longest-common-subsequence)
+ [Unique Paths](#unique-paths)
+ [Unique Paths Two](#unique-paths-ii)
+ [Jump Game](#jump-game)
+ [Jump Game Two](#jump-game-ii)
+ [House Robber](#house-robber)
+ [House Robber Two](#house-robber-two)
+ [Decode Ways](#decode-ways)

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

## Longest Increasing Subsequence

https://leetcode.com/problems/longest-increasing-subsequence/

Создаем массив `dp`, где `dp[i]` - длина наибольшей возрастающей подпоследовательности, включая `i` элемент в `nums`.

```java
public int lengthOfLIS(int[] nums) {
    if (nums == null || nums.length == 0) return 0;

    int[] dp = new int[nums.length];
    dp[0] = 1;
    int maxLen = 1;
    int maxVal;

    for (int i = 1; i < nums.length; i++) {
        maxVal = 0;
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                maxVal = Math.max(maxVal, dp[j]);
            }
        }
        dp[i] = maxVal + 1;
        maxLen = Math.max(maxLen, dp[i]);
    }

    return maxLen;
}
```

## Longest Common Subsequence

https://leetcode.com/problems/longest-common-subsequence/

https://www.youtube.com/watch?time_continue=1&v=NnD96abizww

```java
public int longestCommonSubsequence(String text1, String text2) {
    if (text1 == null || text2 == null) return 0;
    if (text1.length() == 0 || text2.length() == 0) return 0;

    int[][] dp = new int[text1.length() + 1][text2.length() + 1];

    for (int i = 1; i < text1.length() + 1; i++) {
        for (int j = 1; j < text2.length() + 1; j++) {
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                dp[i][j] = 1 + dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[text1.length()][text2.length()];
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

## Unique Paths Two

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

## Jump Game

https://leetcode.com/problems/jump-game/

```java
public boolean canJump(int[] nums) {
    int lastPos = nums.length - 1;
    for (int i = nums.length - 1; i >= 0 ; i--) {
        if (i + nums[i] >= lastPos) {
            lastPos = i;
        }
    }
    return lastPos == 0;
}
```

## Jump Game Two

https://leetcode.com/problems/jump-game-ii/

```java
public static int jump(int[] nums) {
    int reachable = 0, lastReachable = 0, steps = 0;

    for (int i = 0; i < nums.length - 1; i++) {
        reachable = Math.max(reachable, i + nums[i]);
        if (i == lastReachable) {
            steps++;
            lastReachable = reachable;
            if (reachable >= nums.length - 1) break;
        }
    }
    return lastReachable >= nums.length - 1 ? steps : 0;
}
```

## House Robber

https://leetcode.com/problems/house-robber/

```java
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];

    int[] dp = new int[nums.length];

    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);

    for (int i = 2; i < nums.length; i++) {
        dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
    }

    return dp[nums.length - 1];
}
```

## House Robber Two

https://leetcode.com/problems/house-robber-ii/

```java
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];

    int[] robFirst = new int[nums.length];
    int[] noRobFirst = new int[nums.length];

    robFirst[0] = nums[0];
    robFirst[1] = Math.max(nums[0], nums[1]);

    noRobFirst[0] = 0;
    noRobFirst[1] = nums[1];

    for (int i = 2; i < nums.length; i++) {
        robFirst[i] = Math.max(robFirst[i - 2] + nums[i], robFirst[i - 1]);
        noRobFirst[i] = Math.max(noRobFirst[i - 2] + nums[i], noRobFirst[i - 1]);
    }

    return Math.max(robFirst[nums.length - 2], noRobFirst[nums.length - 1]);
}
```

## Decode Ways

https://leetcode.com/problems/decode-ways/

https://www.youtube.com/watch?v=cQX3yHS0cLo

Каждая ячейка массива содержит максимальное количество способов раскодировать строку длины `i`. Если длина строки `i`, то индексы в этой строке от `0` до `i - 1`. 

```java
public int numDecodings(String s) {
    int[] dp = new int[s.length() + 1];

    dp[0] = 1;
    dp[1] = s.charAt(0) == '0' ? 0 : 1;

    // max ways to decode string with length i
    for(int i = 2; i <= s.length(); i++) {
        char cur = s.charAt(i - 1);
        char prev = s.charAt(i - 2);

        // one digit
        if (cur > '0') {
            dp[i] += dp[i - 1];
        }

        // two digits
        if (prev == '1' && cur <= '9' || prev == '2' && cur <= '6') {
            dp[i] += dp[i - 2];
        }
    }

    return dp[s.length()];
}
```
