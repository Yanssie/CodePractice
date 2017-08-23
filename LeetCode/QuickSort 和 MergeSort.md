## QuickSort
先整体有序再局部有序
``` java
int left = start, right = end;
// key point 1: pivot is the value, not the index
int pivot = A[(start + end) / 2];

// key point 2: every time you compare left & right, it should be 
// left <= right not left < right
// partition
        while (left <= right) {
            while (left <= right && A[left] < pivot) {
                left++;
            }
            while (left <= right && A[right] > pivot) {
                right--;
            }
            // 交换
            if (left <= right) {
                int temp = A[left];
                A[left] = A[right];
                A[right] = temp;

                left++;
                right--;
            }
        }

// recursive
// 此时left已经全部在pivot右边，right已经全部在pivot左边
        quickSort(A, start, right);
        quickSort(A, left, end);
```
## MergeSort
先左右 分别recursive 再merge two
```java
public void sortIntegers2(int[] A) {
        // Write your code here
        if(A == null || A.length == 0) {
            return;
        }

        int[] temp = new int[A.length];
        mergeSort(A, 0, A.length - 1, temp);
    }

    private void mergeSort(int[] A, int start, int end, int[] temp) {
        if(start >= end) {
            return;
        }
        int middle = (start + end) / 2;
        mergeSort(A, start, middle, temp);
        mergeSort(A, middle + 1, end, temp);
        merge(A, start, end, temp);
    }

    // merge two sorted array
    private void merge(int[] A, int start, int end, int[] temp) {
        int middle = (start + end) / 2;
        int leftIndex = start;
        int rightIndex = middle + 1;
        int index = leftIndex;

        while(leftIndex <= middle && rightIndex <= end) {
            if(A[leftIndex] <= A[rightIndex]) {
                temp[index++] = A[leftIndex++];
            } else {
                temp[index++] = A[rightIndex++];
            }
        }

        // 可能有一边没有结束
        while(leftIndex <= middle) {
            temp[index++] = A[leftIndex++];
        }

        while(rightIndex <= end) {
            temp[index++] = A[rightIndex++];
        }
        // copy temp
        for(int i = start; i <= end; i++) {
            A[i] = temp[i];
        }
    }
```
