# Arrays

+ [Max product of three](#max-product-of-three)
+ [Two sum](#two-sum)
+ [Contains duplicate](#contains-duplicate)
+ [Contains duplicate 2](#contains-duplicate-2)
+ [Maximum subarray](#maximum-subarray)
+ [Maximum product subarray](#maximum-product-subarray)
+ [Container with most water](#container-with-most-water)
+ [Best Time to Buy and Sell Stock](#best-time-to-buy-and-sell-stock)
+ [Closest Two Sum](#closest-two-sum)


## Max product of three

Дан массив с целыми числами. Найти самое большое произведение 3 чисел из этого массива.

Input: -10, -10, 1, 4, 2

Output: 400

https://tproger.ru/problems/max-multiplication-of-three-numbers/

```python
def max_product_of_3(nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    if len(nums) < 3:
        raise Exception('Less then 3 items')

    min_value = min(nums[0], nums[1])
    max_value = max(nums[0], nums[1])

    max_product_2 = nums[0] * nums[1]
    min_product_2 = nums[0] * nums[1]

    max_product_3 = nums[0] * nums[1] * nums[2]

    for cur_value in nums[2:]:
        max_product_3 = max(max_product_3,
                            cur_value * max_product_2,
                            cur_value * min_product_2)

        max_product_2 = max(max_product_2, cur_value * max_value)
        min_product_2 = min(min_product_2, cur_value * min_value)

        max_value = max(max_value, cur_value)
        min_value = min(min_value, cur_value)

    return max_product_3
```

## Two sum

Дан массив целых чисел. Найти индексы двух элементов, сумма которых равна target.

https://leetcode.com/problems/two-sum/

```python
def twoSum(nums, target):
    sort_nums = sorted([(num, i) for i, num in enumerate(nums)])

    left, right = 0, len(sort_nums) - 1
    while left < right:
        if sort_nums[left][0] + sort_nums[right][0] == target:
            break
        elif sort_nums[left][0] + sort_nums[right][0] > target:
            right -= 1
        else:
            left += 1

    return [sort_nums[left][1], sort_nums[right][1]]
```

## Contains duplicate

https://leetcode.com/problems/contains-duplicate/

1. Полный перебор всех пар.
2. Отсортировать, проверить соседние элементы.
3. 

```python
def containsDuplicate(nums):
    s = set()
    for num in nums:
        if num in s:
            return True
        s.add(num)
    return False
```

## Contains duplicate 2 

Дан массив. Найти дубликаты. Каждый элемент 1 <= nums[i] < n. 

https://medium.com/solvingalgo/solving-algorithmic-problems-find-a-duplicate-in-an-array-3d9edad5ad41

```python
def containsDuplicate(nums):
    '''Each integer is between 1 and n'''
    for i in nums:
        if nums[abs(i)] < 0:
            return abs(i)
        nums[i] *= -1
    return 0
```

## Maximum subarray

Выбрать максимум либо из подмассива, в котором всего один текущий элемент, либо из лучшего решения, полученного на предыдущих шагах.

https://leetcode.com/problems/maximum-subarray/

```python
def max_subarray(nums):
    if not nums: return
    elif len(nums) == 1: return nums[0]

    dp = [0] * len(nums)
    dp[0] = nums[0]
    for i in range(1, len(nums)):
        dp[i] = max(dp[i - 1] + nums[i], nums[i])
    return max(dp)
```

## Maximum product subarray

Выбрать максимум либо из подмассива, в котором всего один текущий элемент, либо из лучшего решения, полученного на предыдущих шагах. Так как произведение отрицательных чисел может дать больший результат, то необходимо хранить максимальное и минимальное произведения предыдущих шагов.

https://leetcode.com/problems/maximum-product-subarray/

```python
def max_product(nums):
    if not nums: return
    elif len(nums) == 1: return nums[0]

    max_product = nums[0]
    min_product = nums[0]
    res = nums[0]

    for i in range(1, len(nums)):
        tmp = [max_product * nums[i], min_product * nums[i], nums[i]]
        max_product = max(tmp)
        min_product = min(tmp)
        res = max(res, max_product)
    return res
```

## Container with most water

Два указателя. Вычисляем площадь, сдвигаем тот указатель, у которого значение высоты меньше.

https://leetcode.com/problems/container-with-most-water/

```python
def max_area(height):
    left, right = 0, len(height) - 1
    max_area = -1

    while left < right:
        max_area = max(max_area,
                       min(height[left], height[right]) * (right - left))
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1

    return max_area
```

## Best Time to Buy and Sell Stock

За один проход ищем минимальную стоимость и продаем в тот момент, когда разница между текущей ценой и минимальной ценой больше максимальной выгоды.

https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

```python
def maxProfit(self, prices):
    if not prices:
        return 0
    min_price = prices[0]
    max_profit = 0

    for price in prices:
        if price < min_price:
            min_price = price
        elif price - min_price > max_profit:
            max_profit = price - min_price

    return max_profit
```

## Closest Two Sum

Отсортировать массив, если требуется. Устанавливаем два указателя. На текущей итерации проверяем, что значения двух указателей ближе к искомому значению. Сдвигаем правый указатель, если сумма указателей больше искомого значения, иначе левый.

https://java2blog.com/given-sorted-array-number-x-find-pair-closest-to-x-array/

```python
def closestTwoSum(nums, target):
    '''Nums sorted'''

    if not nums or len(nums) < 2:
        raise Exception('Less than 2 values')
    # tmp_nums = sorted([(num, i) for i, num in nums])
    left, right = 0, len(nums) - 1
    diff_sum = abs(target - (nums[0] + nums[1]))
    min_diff_left, min_diff_right = 0, 1

    while left < right:
        cur_diff_sum = abs(target - (nums[left] + nums[right]))
        if cur_diff_sum < diff_sum:
            diff_sum = cur_diff_sum
            min_diff_left = left
            min_diff_right = right

        if nums[left] + nums[right] > target:
            right -= 1
        else:
            left += 1

    return [min_diff_left, min_diff_right]
```
