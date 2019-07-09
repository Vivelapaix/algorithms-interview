# Sort

+ [Быстрая сортировка](#быстрая-сортировка)

## Быстрая сортировка

Реализовать алгоритм быстрой сортировки

```java
public static void quickSort(int[] arr, int left, int right) {
    int i = left;
    int j = right;
    int pivot = arr[i + (right - left) / 2];

    while (i <= j) {
        while (arr[i] < pivot) i++;
        while (arr[j] > pivot) j--;

        if (i <= j) {
            int tmp = arr[i];
            arr[i] = arr[j];
            arr[j] = tmp;

            i++;
            j--;
        }

        if (i < right) quickSort(arr, i, right);
        if (j > left) quickSort(arr, left, j);
    }
}
```
