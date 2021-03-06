# 658-find-k-closest-elements

## Question {#question}

[https://leetcode.com/problems/find-k-closest-elements/description/](https://leetcode.com/problems/find-k-closest-elements/description/)

Given a sorted array, two integers k and x, find the k closest elements to x in the array. The result should also be sorted in ascending order. If there is a tie, the smaller elements are always preferred.

**Example 1:**

```text
Input: [1,2,3,4,5], k=4, x=3
Output: [1,2,3,4]
```

**Example 2:**

```text
Input: [1,2,3,4,5], k=4, x=-1
Output: [1,2,3,4]
```

**Note:**

1. The value k is positive and will always be smaller than the length of the sorted array.
2. Length of the given array is positive and will not exceed 104
3. Absolute value of elements in the array and x will not exceed 104

## Thought Process {#thought-process}

1. Binary Search and Expand Window
   1. Locate the closest element using modified binary search
   2. Scan left and right to find the window of length k, where it contains the closest elements to x
   3. Time complexity O\(nlogn\)
   4. Space complexity O\(1\)
2. Modified Binary Search
   1. To find the range of k sequence, we know the sequence has to start ranging from 0 to n - k, where n is length of the array
   2. We use two pointers, lo and hi to narrow our search for the starting point
   3. Then we compare x - arr\[mid\] with arr\[mid + k\] - x, if first difference is greater than the second's,  mid cannot be our starting point, so we move lo to mid + 1
   4. Time complexity O\(logn\)
   5. Space complexity O\(1\)

## Solution

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int id = binarySearch(arr, x);
        int l = id, r = id, n = arr.length;
        while (--k > 0) {
            if (l == 0) r++;
            else if (r == n - 1) l--;
            else if (Math.abs(arr[l - 1] - x) > Math.abs(arr[r + 1] - x)) r++;
            else l--;
        }
        List<Integer> res = new ArrayList<>(k);
        while (l <= r) res.add(arr[l++]);
        return res;
    }

    private int binarySearch(int[] arr, int x) {
        int n = arr.length;
        int i = Arrays.binarySearch(arr, x);
        if (i < 0) {
            i = - (i + 1);
            int leftDiff = i > 0 ? x - arr[i - 1] : Integer.MAX_VALUE;
            int rightDiff = i < n ? arr[i] - x : Integer.MAX_VALUE;
            i = leftDiff < rightDiff ? i - 1 : i;
        }
        return i;
    }
}
```

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int lo = 0, hi = arr.length - k;
        while (lo < hi) {
            int mi = lo + (hi - lo) / 2;
            if (x - arr[mi] > arr[mi + k] - x) lo = mi + 1;
            else hi = mi;
        }
        List<Integer> res = new ArrayList<>(k);
        while (k-- > 0) res.add(arr[lo++]);
        return res;
    }
}
```

## Additional {#additional}

