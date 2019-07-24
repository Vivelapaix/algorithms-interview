# Graphs

+ [Max product of three](#max-product-of-three)

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
