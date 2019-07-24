# Arrays

+ [Max product of three](#max-product-of-three)
+ [Two sum](#two-sum)
+ [Contains duplicate](#contains-duplicate)
+ [Contains duplicate 2](#contains-duplicate-2)
+ [Maximum subarray](#maximum-subarray)
+ [Maximum product subarray](#maximum-product-subarray)


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
