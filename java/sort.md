# Sort

+ [Быстрая сортировка](#быстрая-сортировка)
+ [Cортировка слиянием](#сортировка-слиянием)

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

## Сортировка слиянием

Реализовать алгоритм сортировки слиянием

```java
public static void mergeSort(int[] arr) {
    if (arr == null) {
        return;
    }

    if (arr.length < 2) {
        return;
    }

    int mid = arr.length / 2;
    int[] left = new int[mid];
    int[] right = new int[arr.length - mid];

    for(int i = 0; i < mid; i++) left[i] = arr[i];
    for(int i = mid; i < arr.length; i++) right[i - mid] = arr[i];

    mergeSort(left);
    mergeSort(right);

    merge(arr, left, right);
}

public static void merge(int[] arr, int[] left, int[] right) {
    int i = 0;
    int j = 0;
    int k = 0;

    while(i < left.length && j < right.length) {
        if (left[i] < right[j]) {
            arr[k++] = left[i++];
        } else {
            arr[k++] = right[j++];
        }
    }

    while (i < left.length) arr[k++] = left[i++];
    while (j < right.length) arr[k++] = right[j++];
}
```
