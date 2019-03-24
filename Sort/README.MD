# 排序

 - [例题](#例题)





## 例题
* [例1](#例1)
```java
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P
并将P对1000000007取模的结果输出。 即输出P%1000000007
例如输入：1,2,3,4,5,6,7,0
输出：7
```
* [例2](#例2)
```java
 给定一个数组arr， 和一个数num， 请把小于num的数放在数组的左边， 等于num的数放在数组的中间， 大于num的数放在数组的右边。
 要求额外空间复杂度O(1)， 时间复杂度O(N)
```


## 例1
使用归并排序
```java
public class Solution {
	public int InversePairs(int[] array) {
		if (array == null || array.length < 2)
			return 0;
		int[] copy = new int[array.length];
		int count = mergeSort(array, copy, 0, array.length - 1);
		return count;
	}

	public static int mergeSort(int[] arr, int[] copy, int L, int R) {
		if (L == R)
			return 0;
		int mid = L + ((R - L) >> 1);
		int left = mergeSort(arr, copy, L, mid); //使用递归，将数组分为左右两部分
		int right = mergeSort(arr, copy, mid + 1, R); //使用递归，将数组分为左右两部分
		return (left + right + mergeCount(arr, copy, L, mid, R)) % 1000000007;
	}

	public static int mergeCount(int[] arr, int[] copy, int l, int mid, int r) {
		int count = 0;
		int p1 = l; // 左侧第一个数
		int p2 = mid + 1; // 右侧第一个数
		int i = 0;
		// int copy[]=new int[r-l+1];
		while (p1 <= mid && p2 <= r) {
			if (arr[p1] < arr[p2]) {
				copy[i++] = arr[p1++];  // 左右对比，小的数放在辅助数组的左边，arr[p1] < arr[p2]不是逆序对
			} else {
				copy[i++] = arr[p2++]; // 左右对比，小的数放在辅助数组的左边
				count = (count + mid - p1 + 1) % 1000000007;  // 当arr[p1]>arr[p2]时，因为数组是从小到大排列，表明arr[p1]到arr[mid]之间的数都大于arr[p2]
			}
		}
		while (p1 <= mid) {
			copy[i++] = arr[p1++];
		}
		while (p2 <= r) {
			copy[i++] = arr[p2++];
		}
		for (i = 0; i < r - l + 1; i++) {
			arr[l + i] = copy[i];
		}
		return count % 1000000007;
	}
}
```
## 例2
```java
public class Test02 {
	public static void sort(int[] arr, int num) {
		int l = -1; //代表<num的区域
		int r = arr.length; //代表>num的区域
		int current = 0; //代表当前的位置
		while (current < r) { 
			if (arr[current] < num) { 
				swap(arr, ++l, current++); // l右边的数和当前数交换，<num的区域往右扩大一位
			} else if (arr[current] > num) {
				swap(arr, current, --r); // r左边的数和当前数交换，>num的区域往左边扩大一位，交换到当前位置的数需要再和num比较
			} else {
				current++; // 当前数和Num相等，就继续下一个数
			}
		}
	}

	public static void swap(int[] arr, int a, int b) {
		int temp = arr[a];
		arr[a] = arr[b];
		arr[b] = temp;
	}

	// for test
	public static int[] generateArray() {
		int[] arr = new int[20];
		for (int i = 0; i < arr.length; i++) {
			arr[i] = (int) (Math.random() * 5);
		}
		return arr;
	}

	// for test
	public static void printArray(int[] arr) {
		if (arr == null) {
			return;
		}
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}
		System.out.println();
	}

	public static void main(String[] args) {
		int[] test = generateArray();
		printArray(test);
		sort(test, 2);
		printArray(test);
	}
}
```
