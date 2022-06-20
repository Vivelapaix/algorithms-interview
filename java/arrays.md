# Arrays

+ [Best Time to Buy and Sell Stock](#best-time-to-buy-and-sell-stock)
+ [Container With Most Water](#container-with-most-water)
+ [Subarray Sum Equals K](#subarray-sum-equals-k)
+ [Maximum subarray](#maximum-subarray)
+ [Maximum product subarray](#maximum-product-subarray)
+ [Find the Duplicate Number](#find-the-duplicate-number)
+ [Longest Consecutive Sequence](#longest-consecutive-sequence)
+ [Product of Array Except Self](#product-of-array-except-self)
+ [3Sum](#3sum)
+ [Closest Two Sum](#closest-two-sum)
+ [Max Consecutive Ones](#max-consecutive-ones)
+ [Max Consecutive Ones III](#max-consecutive-ones-iii)
+ [Two Sum](#two-sum)


## Container With Most Water

Два указателя. Вычисляем площадь, сдвигаем тот указатель, у которого значение высоты меньше.

https://leetcode.com/problems/container-with-most-water/

```java
public int maxArea(int[] height) {
    if (height == null || height.length == 0) return 0;

    int left = 0, right = height.length - 1;
    int maxAreaRes = 0;

    while (left < right) {
        maxAreaRes = Math.max(Math.min(height[left], height[right]) * (right - left),
                              maxAreaRes);
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    return maxAreaRes;
}
```

## Best Time to Buy and Sell Stock

Say you have an array for which the i'th element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

За один проход ищем минимальную стоимость и продаем в тот момент, когда разница между текущей ценой и минимальной ценой больше максимальной выгоды.

https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length == 0) return 0;

    int minPrice = prices[0];
    int maxProfit = 0;

    for (int i = 0; i < prices.length; i++) {
        if (prices[i] < minPrice) {
            minPrice = prices[i];
        } else if (prices[i] - minPrice > maxProfit) {
            maxProfit = prices[i] - minPrice;
        }
    }
    return maxProfit;
}
```

## Subarray Sum Equals K

Идея в том, чтобы сохранять в `map` на `i` итерации сумму элементов с `0` до `i` включительно.
По формуле `sum[j] - sum[i] = k`, где `j > i`, получим сумму элементов подмассива с `i + 1` до `j`. Если на `j` итерации значение `sum[j] - k` присутствует в `map`, то это значит, что был найден подмассив с `i + 1` до `j`, сумма элементов которого равна `k`.

https://leetcode.com/problems/subarray-sum-equals-k/

```java
public int subarraySum(int[] nums, int k) {
    if (nums == null || nums.length == 0) return 0;

    int sum = 0;
    int count = 0;
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, 1);

    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
        if (map.containsKey(sum - k)) {
            count += map.get(sum - k);
        }
        map.put(sum, map.getOrDefault(sum, 0) + 1);
    }

    return count;
}
```

или такое решение за `O(n*n)`

```java
public int subarraySum(int[] nums, int k) {
    if (nums == null || nums.length == 0) return 0;

    int count = 0;
    for (int startIndex = 0; startIndex < nums.length; startIndex++) {
        int sum = 0;
        for (int endIndex = startIndex; endIndex < nums.length; endIndex++) {
            sum += nums[endIndex];
            if (sum == k) {
                count++;
            }
        }
    }
    return count;
}
```

## Maximum subarray

Выбрать максимум либо из подмассива, в котором всего один текущий элемент, либо из лучшего решения, полученного на предыдущих шагах.

https://leetcode.com/problems/maximum-subarray/

```java
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];

    int sum = nums[0];
    int maxSum = nums[0];
    for (int i = 1; i < nums.length; i++) {
        sum = Math.max(sum + nums[i], nums[i]);
        if (sum > maxSum) {
            maxSum = sum;
        }
    }

    return maxSum;
}
```

## Maximum product subarray

Выбрать максимум либо из подмассива, в котором всего один текущий элемент, либо из лучшего решения, полученного на предыдущих шагах. Так как произведение отрицательных чисел может дать больший результат, то необходимо хранить максимальное и минимальное произведения предыдущих шагов.

https://leetcode.com/problems/maximum-product-subarray/

```java
public int maxProduct(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];

    int maxProduct = nums[0];
    int minProduct = nums[0];
    int res = nums[0];

    for (int i = 1; i < nums.length; i++) {
        int tmp[] = {maxProduct * nums[i], minProduct * nums[i], nums[i]};
        maxProduct = Math.max(Math.max(tmp[0], tmp[1]), tmp[2]);
        minProduct = Math.min(Math.min(tmp[0], tmp[1]), tmp[2]);
        res = Math.max(res, maxProduct);
    }

    return res;
}
```

## Find the Duplicate Number

https://leetcode.com/problems/find-the-duplicate-number/

```java
public int findDuplicate(int[] nums) {    
    for (int num : nums) {
        if (nums[Math.abs(num)] < 0) {
            return Math.abs(num);
        }
        nums[Math.abs(num)] *= -1;
    }
    return 0;
}
```

## Longest Consecutive Sequence

https://leetcode.com/problems/longest-consecutive-sequence/

https://leetcode.com/problems/longest-consecutive-sequence/discuss/351902/java.-clearly-and-easy-understand.

```java
public int longestConsecutive(int[] nums) {
    Set<Integer> set = new HashSet<Integer>();
    int currentNum;
    int currentLen;
    int maxLen = 0;

    for (int num : nums) set.add(num);

    for (int num : set) {
        if (!set.contains(num - 1)) {
            currentNum = num;
            currentLen = 1;
            while (set.contains(currentNum + 1)) {
                currentNum += 1;
                currentLen += 1;
            }
            maxLen = Math.max(maxLen, currentLen);
        }
    }
    return maxLen;
}
```

## Product of Array Except Self

https://leetcode.com/problems/product-of-array-except-self/

```java
public int[] productExceptSelf(int[] nums) {
    int[] res = new int[nums.length];
    if (res.length == 0) return res;

    res[0] = 1;
    for (int i = 1; i < nums.length; i++) {
        res[i] = res[i - 1] * nums[i - 1];
    }

    int rightProduct = 1;
    for (int i = nums.length - 1; i >= 0; i--) {
        res[i] = res[i] * rightProduct;
        rightProduct *= nums[i];
    }
    return res;
}
```


## 3Sum

https://leetcode.com/problems/3sum/

https://fizzbuzzed.com/top-interview-questions-1/

```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums == null || nums.length < 3) return res;
    Arrays.sort(nums);
    List<Integer> triplet;
    for (int i = 0; i < nums.length; i++) {

        if (i != 0 && nums[i] == nums[i - 1]) continue;
        int j = i + 1;
        int k = nums.length - 1;

        while (j < k) {
            if (nums[i] + nums[j] + nums[k] == 0) {
                triplet = new ArrayList<>();
                triplet.add(nums[i]);
                triplet.add(nums[j]);
                triplet.add(nums[k]);
                res.add(triplet);
                j++;

                while (j < k && nums[j] == nums[j - 1]) j++;
            } else if (nums[i] + nums[j] + nums[k] > 0) {
                k--;
            } else {
                j++;
            }
        }
    }
    return res;
}
```

## Closest Two Sum

Отсортировать массив, если требуется. Устанавливаем два указателя. На текущей итерации проверяем, что значения двух указателей ближе к искомому значению. Сдвигаем правый указатель, если сумма указателей больше искомого значения, иначе левый.

https://java2blog.com/given-sorted-array-number-x-find-pair-closest-to-x-array/

```java
public static int[] closestTwoSum(int[] nums, int target) {
    if (nums == null || nums.length == 0 || nums.length == 1) {
        return new int[0];
    }

    //Arrays.sort(nums);
    int left = 0, right = nums.length - 1;
    int diff = Math.abs(target - (nums[left] + nums[right]));
    int[] resIndices = {0, 1};

    while (left < right) {
        int sum = nums[left] + nums[right];
        
        if (Math.abs(target - sum) < diff) {
            diff = Math.abs(target - sum);
            resIndices[0] = left;
            resIndices[1] = right;
        }
        
        if (sum > target) {
            right--;
        } else {
            left++;
        }
    }

    return resIndices;
}
```

## Max Consecutive Ones

https://leetcode.com/problems/max-consecutive-ones/

```java
public int findMaxConsecutiveOnes(int[] nums) {
    int maxOnes = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) continue;
        int count = 1;
        while (i < nums.length - 1 && nums[i] == nums[i + 1]) {
            count++;
            i++;
        }
        maxOnes = Math.max(maxOnes, count);
    }
    return maxOnes;
}
```

## Max Consecutive Ones III

Given an array A of 0s and 1s, we may change up to K values from 0 to 1.
Return the length of the longest (contiguous) subarray that contains only 1s. 

Идея в том, чтобы использовать окно из двух указателей, постоянно вычисляя `right - left + 1`. В тот момент, когда встретятся `K + 1` нулей, начинаем двигать левый указатель, пока в окне не останется ровно `K` нулей.

https://leetcode.com/problems/max-consecutive-ones-iii/

```java
public int longestOnes(int[] A, int K) {
    if (A == null || A.length == 0) return 0;
    int left = 0, right = 0;
    int maxOnes = 0;
    int zerosCount = 0;

    while (right < A.length) {
        if (A[right] == 0) {
            zerosCount++;
        }
        while (zerosCount > K) {
            if (A[left] == 0) {
                zerosCount--;
            }
            left++;
        }
        maxOnes = Math.max(maxOnes, right - left + 1);
        right++;
    }
    return maxOnes;
}
```

## Two Sum

https://leetcode.com/problems/two-sum/

Solution 1:
```java
public int[] twoSum(int[] nums, int target) {
    for(int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
               return new int[] {i, j}; 
            }
        }
    }
    return new int[] {-1, -1};
}
```

Solution 2:
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for(int i = 0; i < nums.length; i++) {
        if (map.containsKey(target - nums[i])) {
            return new int[] {map.get(target - nums[i]), i};
        }
        map.put(nums[i], i);
    }
    return new int[] {-1, -1};
}
```
