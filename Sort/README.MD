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


## 例1
使用归并排序
```java
public class Solution {
    public int InversePairs(int [] array) {
        if (array == null || array.length < 2)
            return 0;
        int[] copy=new int[array.length];
        int count= mergeSort(array, copy,0, array.length - 1);
        return count;
    }
  
    public static int mergeSort(int[] arr,int[] copy, int L, int R) {
        if (L == R)
            return 0;
        int mid = L + ((R - L) >> 1);
        int left=mergeSort(arr, copy,L, mid);
        int right=mergeSort(arr,copy, mid + 1, R);
        return (left+right+mergeCount(arr, copy,L, mid, R))%1000000007;
    }
  
    public static int mergeCount(int[] arr,int[] copy, int l, int mid, int r) {
        int count = 0;
        int p1=l;
        int p2=mid+1;
        int i=0;
        //int copy[]=new int[r-l+1];
        while(p1<=mid && p2<=r){
            if(arr[p1]<arr[p2]){
                copy[i++]=arr[p1++];
            }else{
                copy[i++]=arr[p2++];
                count = (count+mid-p1+1)%1000000007;
            }
        }
        while(p1<=mid){
            copy[i++]=arr[p1++];
        }
        while(p2<=r){
            copy[i++]=arr[p2++];
        }
        for(i=0;i<r-l+1;i++){
            arr[l+i]=copy[i];
        }
        return count%1000000007;
    }
}
```
