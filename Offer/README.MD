# 剑指Offer
  * [位运算](#位运算)
  * [Array](#array)
  * [String](#string)
  * [LinkedList](#linkedlist)
  * [StackQueue](#stackqueue)
  * [DP](#dp)
  * [Tree](#tree)





## 位运算
* 整值的整数次方
```java
给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

循环：
public double Power(double base, int exponent) {
        if(exponent==0) return 1;
        if(exponent==1) return base;
        double sum=base;
        if(exponent<0){
            exponent=Math.abs(exponent);
            for(int i=2;i<=exponent;i++){
                sum*=base;
            }
            return 1/sum;
        }else{
            for(int i=2;i<=exponent;i++){
            sum*=base;
        }
             return sum;
  }
}

递归：
public double Power(double base, int exponent) {
        if(base==0) return 0;
        if(exponent==0) return 1;
        if(exponent==1) return base;
        boolean flag = true;
        if(exponent<0){
            exponent=-exponent;
            flag=false;
        }
        double res=Power(base,exponent>>1);
        res*=res;
        if((exponent&1)==1){
            res*=base;
        }
        return flag?res:1/res;
  }

快速求幂算法：
例如：
3 ^ 999
= 3 ^ (512 + 256 + 128 + 64 + 32 + 4 + 2 + 1)
= (3 ^ 512) * (3 ^ 256) * (3 ^ 128) * (3 ^ 64) * (3 ^ 32) * (3 ^ 4) * (3 ^ 2) * 3
把999转为2进制数：1111100111，其各位就是要乘的数。这提示我们利用求二进制位的算法：
   public double Power(double base, int exponent) {
        if(base==0) return 0;
        if(exponent==0) return 1;
        if(exponent==1) return base;
        boolean flag = true;
        double res=1;
        if(exponent<0){
            exponent=-exponent;
            flag=false;
        }
        while(exponent>0){
            if(exponent%2 ==1){
                res*=base;
            }
            exponent=exponent>>1;
            base*=base;
        }
        return flag?res:1/res;
  }
```
* 整数中1出现的次数
```java
求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的
数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。
ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1到n中1出现的次数）。

这个方法可以求1~n的所有整数中(1,2,3,4,5,6,7,8,9出现的所有次数）:
通过使用一个位置乘子 m 遍历数字的位置, m 分别为1,10,100,1000…etc.（m<=n）
例如n=3141592，对于每个位置来说，分成前后两部分，比如在百位上，分为a=31415 和 b=92 ；
百位上的数字是5,大于1，那么在百位上的数字为1的时候，它的前面有0-3141种取值，它的后面有0-99种取值，所以一共有3142*100种取值。
再比如在千位上，分为a=3141 和 b=592；百位上的数字正好是1,那么它的前面取值为0-313时，后面可以有0-999种取值，
但是当前面取值为314时，后面最大只能取到592,所以一共有314*1000+593种取值。
如何判断各位置上的数是否大于1？假设百位数为x，若（x+8）/10等于1，则大于1，若（x+8）/10等于0，则小于等于1。
因此前缀可用（n/m + 8）/10 *m来计算(若计算2的个数，可以改为（n/m + 7）/10*m,若计算3的个数，改为（n/m + 6）/10*m，…以此类推)。
注意：只有n的第m位为1时需要计算后缀，后缀计算为 （n/m%10==1）*(b+1)，即（n/m%10==1）判断第m位是否为1，若为1，则加上（b+1），
若不为1，则只计算前缀。（若计算2的个数，可以改为（n/m%10==2）*(b+1)，若计算3的个数，可以改为（n/m%10==3）*(b+1)…以此类推）
public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) {
        int count=0;
        int m=1;
        int a=0;
        int b=0;
        for(;m<=n;m*=10){
            a = n/m;
            b = n%m;
            count+= a%10==1?(a+8)/10*m+(b+1):(a+8)/10*m;
	    //(a+8)/10来判断当前位置是否大于1，后面乘以m 就是后面的数取0-999的所有情况
	    //然后如果当前位置的数等于1，就加上后缀的所有情况
        }
        return count;
    }
}
```
* 丑数
```java
把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 
习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

丑数=2^x * 3^y * 5^z  所以一个丑数一定是另一个丑数乘以2/3/5而得到的，
丑数的顺序就是2*丑数1、3*丑数2、5*丑数3 中的最小值。
public class Solution {
    public int GetUglyNumber_Solution(int index) {
        if(index<7) return index;
        int[] arr = new int[index+1];
        arr[1] = 1;
        int t2=1;
        int t3=1;
        int t5=1;
        for(int i =2;i<arr.length;i++){
            arr[i] = Math.min(arr[t2]*2,Math.min(arr[t3]*3,arr[t5]*5));
            if(arr[i]==arr[t2]*2) t2++;//如果这个丑数是第x个丑数乘以2得到的，那么就继续用第x+1个丑数乘以2去比较
            if(arr[i]==arr[t3]*3) t3++;
            if(arr[i]==arr[t5]*5) t5++;
        }
        return arr[index];
    }
}
```
* 二进制中1的个数
```java
输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

(n%1)==1说明最后一位是1，然后使用无符号的右移 >>> 判断下一位
（不能用 >> 是带符号的右移，因为如果n为负数，右移时会在最高为补1，就会死循环）
 public int NumberOf1(int n) {
        if(n==0) return 0;
        int count = 0;
        while(n!=0){
            if((n&1)==1){
                count++;
            }
            n=n>>>1;
        }
        return count;
    }

n先&1,然后1每次左移一位，n不变：
public int NumberOf1(int n) {
        if(n==0) return 0;
        int count = 0;
        int m =1;
        while(m!=0){
            if((n&m)!=0){
                count++;
            }
            m=m<<1;
        }
        return count;
    }

如果一个整数不为0，那么这个整数至少有一位是1。
如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1。其余所有位将不会受到影响。
举个例子：一个二进制数1100，从右边数起第三位是处于最右边的一个1。减去1后，第三位变成0，
它后面的两位0变成了1，而前面的1保持不变，因此得到的结果是1011.我们发现减1的结果是把最右边的一个1开始的所有位都取反了。
这个时候如果我们再把原来的整数和减去1之后的结果做与运算，从原来整数最右边一个1那一位开始所有位都会变成0。
如1100&1011=1000.也就是说，把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0.
那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。
public class Solution {
    public int NumberOf1(int n) {
        if(n==0) return 0;
        int count = 0;
        while(n!=0){
            count++;
            n = n&(n-1);
        }
        return count;
    }
}
```
* 求1+2+3+...+n
```java
求1+2+3+…+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）

使用&&短路的特性
当n>0时，执行sum+=Sum_Solution(n-1)，实现递归计算Sum_Solution(n)。
当n=0时，结束递归。
public class Solution {
    public int Sum_Solution(int n) {
        int sum = n; 
        boolean flag = (n>0) && ((sum+=Sum_Solution(n-1))>0); //在n不为0时，可以计算sum的值
        return sum;
    }
}

利用异常结束递归
public class Solution {
    public int Sum_Solution(int n) {
        try{
            int i =1/n;  //当n=0时，进入catch
            return n+Sum_Solution(n-1);
        }catch(Exception e){
            return 0;  //当n=0时，结束递归
        }
    }
}
```
* 不用加减乘除做加法
```java
写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

由于题目要求不能使用四则运算，那么就需要考虑使用位运算。两个数相加可以看成两个数的每个位先相加，但不进位，
然后在加上进位的数值，如12+8可以看成1+0=1 2+8=0，由于2+8有进位，所以结果就是10+10=20
二进制中可以表示为1000+1100 先每个位置相加不进位，则0+0=0 0+1=1 1+0=1 1+1=0这个就是按位异或运算^.
对于1+1出现进位，我们可以使用按位与运算&然后再将结果左移一位。最后将上面两步的结果相加，
相加的时候依然要考虑进位的情况，直到不产生进位。
public class Solution {
    public int Add(int num1,int num2) {
        while(num2!=0){
            int temp = num1^num2;
            num2 = (num1&num2)<<1;
            num1 = temp;
        }
        return num1;
    }
}
```



















## Array

* 旋转数组的最小数字
```java
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 
例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

从头遍历，逐个比较：
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        if(array.length==0) return 0;
        for(int i=1;i<array.length;i++){
            if(array[0]>array[i])
                return array[i];
        }
        return array[0];
    }
}

二分查找：
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        int low = 0 ; 
        int high = array.length - 1;   
        while(low < high){
            int mid = low + (high - low) / 2;        
            if(array[mid] > array[high]){
                low = mid + 1;
            }else if(array[mid] == array[high]){
                high = high - 1;
            }else{
                high = mid;
            }   
        }
        return array[low];
    }
}
```

* 二维数组中的查找
```java
在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。
请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

从右上角开始找：O（M+N）   M行N列
public boolean Find(int target, int [][] array) {
        int n = 0;
		int m = array[0].length - 1;
		while (m > -1 && n < array.length){
			if (array[n][m] == target) {
				//System.out.println("第" + n + "行" + "第" + m + "列");
				return true;
			} else if (array[n][m] > target) { 
				m--;
			} else {
				n++; 
			}
		}
		return false;
    }

对每一行都二分查找： O（MlogN）     只有M很小的时候，这种方法时间复杂度才更低  
public boolean Find(int [][] array,int target) {
         
        for(int i=0;i<array.length;i++){
            int low=0;
            int high=array[i].length-1;
            while(low<=high){
                int mid=(low+high)/2;
                if(target>array[i][mid])
                    low=mid+1;
                else if(target<array[i][mid])
                    high=mid-1;
                else
                    return true;
            }
        }
        return false;
    }
```

* 调整数组顺序使奇数位于偶数前面
```java
输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，
并保证奇数和奇数，偶数和偶数之间的相对位置不变。

从头遍历，新开一个数组用来存偶数，然后再把偶数填入原数组：
public void reOrderArray(int [] array) {
        int[] temp=new int[array.length];
        int c1=0;//偶数个数
        int c2=0;//奇数个数
        for(int i=0;i<array.length;i++){
            if(array[i]%2==0){
                temp[c1++]=array[i]; //存偶数
            }else{
                array[c2++]=array[i];//奇数直接挨个填
            }
        }
        c1=0;
        for(;c2<array.length;c2++){//把偶数填入原数组
            array[c2]=temp[c1++];
        }
    }
    
类似冒泡排序，前偶后奇就交换
public class Solution {
    public void reOrderArray(int [] array) {
        for (int i = 0; i < array.length;i++)
                {
                    for (int j = array.length - 1; j>i;j--)
                    {
                        if (array[j] % 2 == 1 && array[j - 1]%2 == 0)
                        {
                            swap(array,j, j-1);
                        }
                    }
                }
    }
```
* 顺时针打印矩阵
```java
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字
这个题，关键是要注意边界

先打印最外面一层，再向里一层一层的打印，每一层都是从左-右-下-左-上的打印顺序:
public static void spiralOrderPrint(int[][] matrix) {
		int tR = 0; // 左上角坐标
		int tC = 0; // 左上角坐标
		int dR = matrix.length - 1; // 右下角坐标
		int dC = matrix[0].length - 1; // 右下角坐标
		while (tR <= dR && tC <= dC) {
		// 先打印最外面一层，再把圈缩小一层，打印第二层……依次打印
		    printEdge(matrix, tR++, tC++, dR--, dC--); 
		}
	}

public static void printEdge(int[][] m, int tR, int tC, int dR, int dC) {
		if (tR == dR) { // 如果行数相等，说明只有一行
			for (int i = tC; i <= dC; i++) {
				System.out.print(m[tR][i] + " ");
			}
		} else if (tC == dC) {// 如果列数相等，说明只有一列
			for (int i = tR; i <= dR; i++) {
				System.out.print(m[i][tC] + " ");
			}
		} else {
			int curC = tC;
			int curR = tR;
			while (curC != dC) {// 当前列不等于右边的列
				System.out.print(m[tR][curC] + " ");// 从左到右打印最上边的行
				curC++;
			}
			while (curR != dR) {// 当前行不等于下边的行
				System.out.print(m[curR][dC] + " ");// 从上到下打印最右边的列
				curR++;
			}
			while (curC != tC) {
				System.out.print(m[dR][curC] + " ");// 从右往左打印最下边的行
				curC--;
			}
			while (curR != tR) {
				System.out.print(m[curR][tC] + " ");// 从下到上打印最左边的列
				curR--;
			}
		}
	}

public ArrayList<Integer> printMatrix(int [][] matrix) {
        ArrayList<Integer> list=new ArrayList<Integer>();
        int row=matrix.length;
        int col=matrix[0].length;
        if(row==0||col==0) return null;
        int top=0;
        int left=0;
        int bottom=row-1;
        int right=col-1;
        while(top<=bottom && left<=right){
		//左到右
            for(int i=left;i<=right;++i)
                list.add(matrix[top][i]);
		//上到下
            for(int i=top+1;i<=bottom;++i)
                list.add(matrix[i][right]);
		//右到左
            for(int i=right-1;i>=left && top<bottom;--i)
                list.add(matrix[bottom][i]);
		//下到上
            for(int i=bottom-1;i>top && right>left;--i)
                list.add(matrix[i][left]);
            top++;
            left++;
            bottom--;
            right--;
        }
        return list;
    }
```
* 数组中出现次数超过一半的数字
```java
数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。
例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。
如果不存在则输出0。

使用HashMap存储出现过的数字和次数，返回最大的：
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        if(array==null || array.length==0) return 0;
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<array.length;i++){
            Integer temp=map.get(array[i]);
            if(temp==null){
                map.put(array[i],1);
                temp=1;
            }else{
                map.put(array[i],++temp);
            }
            if(temp>array.length/2){
                    return array[i];
                }
        }
        return 0;
    }
}

将数组排序之后，相同的数字都在相邻位置，
直接判断数组当前位置的数字与（数组位置+数组一半长度位置）的数字是否相等，相等则输出该数字。
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        if(array==null || array.length==0) return 0;
        Arrays.sort(array);
        int L=0;
        int R=L+array.length/2;
        while(R<array.length){
            if(array[L]==array[R]){
                return array[L];
            }else{
                L++;
                R++;
            }
        }
        return 0;           
    }
}

采用阵地攻守的思想：
第一个数字作为第一个士兵，守阵地；count = 1；遇到相同元素，count++;
遇到不相同元素，即为敌人，同归于尽,count--；
当遇到count为0的情况，又以新的i值作为守阵地的士兵，继续下去，到最后还留在阵地上的士兵，有可能是主元素。
再加一次循环，记录这个士兵的个数看是否大于数组一半即可
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        if(array==null || array.length==0) return 0;
        int count = 1;
        int aim=0;
        for(int i=1;i<array.length;i++){
            if(count==0){
                count = 1;
                aim=i;
            }else if(array[aim]==array[i]){
                count++;
            }else {
                count--;
            }
        }
        count = 0;
        for(int i=0;i<array.length;i++){
                if(array[i]==array[aim])
                    count++;
                }
        if(count>array.length/2){
                return array[aim];
           }
        return 0;
    }
}

输入是数字，介于0～9之间。所以可以首先建立一个含有10个元素的数组，
然后用输入的数字作为下标以统计每个数字出现的次数。再对这个数组与长度的一半进行比较，输出结果
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        int[] res=new int[10];
        for(int i=0;i<array.length;i++){
            res[array[i]]++;
        }
        for(int i=0;i<res.length;i++){
            if(res[i]>array.length/2)
                return i;
        }
        return 0;
    }
}
```
* 最小的k个数
```java
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4

使用快速排序，如果等于区域包含k位置，就返回0-k位置的数：
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> list = new ArrayList<>();
        if(input==null || input.length==0 || k>input.length) return list;
        int L=0;
        int R=input.length-1;
        while(L<=R){
            int[] res=partition(input,L,R);//获得等于区域的下标
            if(k-1>=res[0] && k-1<=res[1]){ //如果k-1（表示第k小的数的下标）位于等于区域，直接添加0-k位置的数即可
                for(int i=0;i<k;i++){
                        list.add(input[i]);
                }
                return list;
            }else if(k-1<res[0]){
                    R=res[0]-1;
            }else if(k-1>res[1]){
                    L=res[1]+1;
                }
        }
          return list;
  }
    public int[] partition(int[] arr,int L,int R){
        int less=L-1;
        int big=R+1;
        int temp=arr[L];
        while(L<big){
            if(arr[L]<temp){
                swap(arr,++less,L++);
            }else if(arr[L]>temp){
                swap(arr,L,--big);
            }else{
                L++;
            }
        }
        return new int[]{less+1,big-1};
    }
    public void swap(int[] arr,int i,int j){
        int temp=arr[i];
        arr[i]=arr[j];
        arr[j]=temp;
    }
}

类似冒泡排序，每次把最小的数放到最后，放k个就可以返回，不需要全部排序：
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> list = new  ArrayList<>();
        if(input==null || input.length==0 || k>input.length ) return list;
        for(int i=0;i<k;i++){
            for(int j=1;j<input.length-i;j++){
                if(input[j-1]<input[j]){
                    swap(input,j-1,j);
                }
            }
            list.add(input[input.length-1-i]);
        }
        return list;
    }
    public void swap(int[] arr,int i,int j){
        int temp = arr[i];
        arr[i]=arr[j];
        arr[j]=temp;
    }
}

建立大根堆，i位置的数比堆顶小，就移除堆顶，添加这个数：
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> list = new ArrayList<>();
        if(input==null || input.length==0 || k>input.length || k==0) return list;
        int[] heap = new int[k];
        for(int i=0;i!=k;i++){
            heapInsert(heap,input[i],i);//0-k建立大根堆
        }
        for(int i=k;i!=input.length;i++){//再从k开始逐个比较，如果比大根堆的堆顶小，就替换这个堆顶，再调整成大根堆
            if(heap[0]>input[i]){
                heap[0]=input[i]; //如果比大根堆的堆顶小，就替换这个堆顶
                heapSort(heap,0,k);//再调整成大根堆
            }
        }
        for(int i=0;i!=heap.length;i++){
            list.add(heap[i]);
        }
        return list;
         
    }
    public void heapInsert(int[] arr,int val,int i){ //建立大根堆
        arr[i] = val;
        while(i!=0){
            int parent = (i-1)/2;
            if(arr[parent] < arr[i]){
                int temp=arr[parent];
                arr[parent] = arr[i];
                arr[i]=temp;
                i=parent;
            }else{
                break;
            }
        }
    }  
    public void heapSort(int[] arr,int index,int size){//调整大根堆的过程，与左右子树比较
        int left = 2*index+1;
        int right= 2*index+2;
        int largest = index;
        while(left<size){
            if(arr[index]<arr[left]){
                largest = left;
            }
            if(right<size && arr[right]>arr[largest]){
                largest = right;
            }
            if(largest!=index){
                int temp=arr[index];
                arr[index] = arr[largest];
                arr[largest]=temp;
            }else{
                break;
            }
            index=largest;
            left = 2*index+1;
            right=2*index+2;
        }
    }
}

直接用优先级队列实现大根堆结构：
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> list = new ArrayList<>();
        if(input==null || input.length==0 || k>input.length || k==0) return list;
        PriorityQueue<Integer> queue = new PriorityQueue<>(k,new Comparator<Integer>(){
            public int compare(Integer a,Integer b){
                return b-a;
            }
        });
        for(int i=0;i<k;i++){
            queue.offer(input[i]);
        }
        for(int i=k;i<input.length;i++){
            if(queue.peek()>input[i]){
                queue.poll();
                queue.offer(input[i]);
            }
        }
        for(Integer i:queue ){
            list.add(i);
        }
        return list;
    }
}
```
* 连续子数组的最大和
```java
计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？
例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。
给一个数组，返回它的最大连续子序列的和(子向量的长度至少是1)

使用sum记录i-1位置的累加和，如果大于0，就加上i位置的数。如果sum小于0，就设置sum为i位置上的数
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        int sum=array[0];
        int max=sum;
        for(int i=1;i<array.length;i++){
            if(sum>=0){
                sum+=array[i];
            }else{
                sum=array[i];
            }
            max = Math.max(max,sum);
        }
        return max;
    }
}
```
* 数组中的逆序对
```java
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。
输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007
示例：输入 1,2,3,4,5,6,7,0   输出 7

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
* 数字在排序数组中出现的次数
```java
统计一个数字在排序数组中出现的次数。

使用二分查找，找到这个数的下标i，然后统计i的前面有多少个这个数，再统计i的后面有多少这个数：
public class Solution {
    public int GetNumberOfK(int [] array , int k) {
        if(array==null || array.length==0) return 0;
        if(k>array[array.length-1] || k<array[0]) return 0;
        int L=0;
        int R=array.length-1;
        int mid = 0;
        int count =0;
        while(L<R){
            mid = L +(R-L)/2;
            if(array[mid]>k){
                R=mid-1;
            }else if(array[mid]<k){
                L=mid+1;
            }else{
                break;
            }
        }
        if(array[mid]!=k) return 0;
        for(int i = mid-1;i>=0;i--){
            if(array[i]==array[mid]){
                count++;
            }else{
                break;
            }
        }
        for(int i =mid+1;i<array.length;i++){
            if((array[i]==array[mid])){
                count++;
            }else{
                break;
            }
        }
        return count+1;
    }
}

使用二分查找，找到k-0.5应该插入的位置，和k+0.5的位置，这两个位置之间全部都是k
public class Solution {
    public int GetNumberOfK(int [] array , int k) {
        if(array==null || array.length==0) return 0;
        if(k>array[array.length-1] || k<array[0]) return 0;
        int L = 0;
        int R = array.length-1;
        int index1 = getIndex(L,R,array,k-0.5);
        int index2 = getIndex(index1,R,array,k+0.5);
        return index2-index1;
    }
    public int getIndex(int L,int R,int[] arr,double k){
        int mid = L;
        while(L<=R){
            mid = L +(R-L)/2;
            if(arr[mid]>k){
                R = mid-1;
            }else {
                L = mid+1;
            }
        }
        return L;
    }
}
```
* 数组中只出现一次的数字
```java
一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

用一个HashSet,遍历数组，如果arr[i]没在set中，就存储，如果set中有这个数，就删除，最后set中剩余的就是只出现一次的数字。
public class Solution {
    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        HashSet<Integer> set = new HashSet<Integer>();
        for(int i = 0; i<array.length; i++){
            if(!set.contains(array[i])){
                set.add(array[i]);
            }else{
                set.remove(array[i]);
            }
        }
        num1[0] = (Integer)set.toArray()[0];
        num2[0] = (Integer)set.toArray()[1];
    }
}

对数组中的数逐个使用异或运算，最后得到的结果一定是那两个只出现一次的数字的异或结果。但光从这个结果肯定无法得到这个两个数字。
因此我们来分析下简化版中“异或”解法的关键点，这个关键点也相当明显：数组只能有一个数字出现1次。
设题目中这两个只出现1次的数字分别为A和B，如果能将A，B分开到二个数组中，那显然符合“异或”解法的关键点了。
因此这个题目的关键点就是将A，B分开到二个数组中。由于A，B肯定是不相等的，因此在二进制上必定有一位是不同的。
根据这一位是0还是1可以将A，B分开到A组和B组。而这个数组中其它数字要么就属于A组，要么就属于B组。
再对A组和B组分别执行“异或”解法就可以得到A，B了。而要判断A，B在哪一位上不相同，只要根据A异或B的结果就可以知道了，
这个结果在二进制上为1的位就说明A，B在这一位上是不相同的。
public class Solution {
    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        int x = 0;
        for(int i =0; i<array.length;i++){
            x^=array[i];
        }
        int n = 1;
        while((x&n)==0){
            n=n<<1;
        }
        for(int i =0;i<array.length;i++){
            if((array[i]&n)==0){
                num1[0]^=array[i];
            }else{
                num2[0]^=array[i];
            }
        }
    }
}
```
* 和为S的连续正数序列
```java
小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。
但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。
没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。
现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列?
输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

使用窗口结构，如果窗口内的和大于S，则L++，如果窗口内的和小于S，则R++:
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> list = new ArrayList<ArrayList<Integer>>();
        int L = 1;
        int R = 2;
        while(R>L){
            int res = (L+R)*(R-L+1) / 2;
            if(res==sum){
                ArrayList<Integer> in = new ArrayList<Integer>();
                for(int i =L;i<=R;i++){
                    in.add(i);
                }
                list.add(in);
                L++;
            }else if(res>sum){
                L++;
            }else{
                R++;
            }
        }
        return list;
    }
}

利用S/n得到平均值（n为连续数字的个数）。
如果n为奇数个，那么S/n得到的正好是中间的那个数，往前推（n-1）/2个就是这个序列的开头；
  (n&1)==1) && sum%n==0 表示n为奇数时，sum可以被n个数得到
如果n为偶数个，那么S/n得到的是中间2个数的平均值，往前推（n-1）/2个就是这个序列的开头。
   sum%n*2 ==n 表示n为偶数时，中间2个数的平均值一定带小数0.5，也就是说sum%n得到的结果一定是0.5n 
由题可知n>= 2，我们完全可以将n从2到S全部遍历一次，但是大部分遍历是不必要的。
根据等差数列的求和公式：S = (1 + n) * n / 2，得到n<根号下2S
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> list = new ArrayList<ArrayList<Integer>>();
        for(int n=(int)Math.sqrt(2*sum);n>=2;n--){
            if(((n&1)==1) && sum%n==0 || sum%n*2 ==n){
                ArrayList<Integer> in = new ArrayList<Integer>();
                for(int j = 0, k = (sum/n) - (n-1)/2;j<n;j++,k++){
                    in.add(k);
                }
                list.add(in);
            }
        }
        return list;
    }
}
```
* 和为S的两个数字
```java
输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。
输出描述:对应每个测试案例，输出两个数，小的先输出。

头尾2个数相加，2个数离的越远，乘积就越小。因此第一对和为sum的就是乘积最小的。
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        if(array==null || array.length==0) return list;
        int L = 0;
        int R = array.length-1;
        while(L<R){
            if(array[L]+array[R] == sum){
                list.add(array[L]);
                list.add(array[R]);
                break;
            }else if (array[L]+array[R]>sum){
                R--;
            }else{
                L++;
            }
        }
        return list;
    }
}
```
* 扑克牌顺子
```java
一副扑克牌，大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。
如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0

如果是顺子，则需要满足
1.数组长度为5
2.除了0之外，没有重复的数字
3.如果要组成顺子，就必须满足max-min<=4
public class Solution {
    public boolean isContinuous(int [] numbers) {
	        if(numbers==null || numbers.length!=5) return false;
	        Arrays.sort(numbers);
	        int joker = 0;
	        int i =0;
	        for(;i<numbers.length;i++){
	        	if(numbers[i]==0){
	        		joker++;
	        	}else{
	        		break;
	        	}
	        }
	        for(int j=i;j<numbers.length-1;j++){
	        	if(numbers[j]==numbers[j+1]){
	        		return false;
	        	}
	        }
	        if(numbers[numbers.length-1]-numbers[i]<=4){
	        	return true;
	        }else{
	        	return false;
	        }
	}
}

不使用排序的方法
public class Solution {
 public static boolean isContinuous(int [] numbers) {
            if(numbers==null || numbers.length!=5) return false;
            int[] count = new int [14];
            int max=-1;
            int min=14;
            for(int i =0;i<5;i++){
                count[numbers[i]]++;
                if(count[numbers[i]]>1 && numbers[i]!=0){
                    return false;
                }
                if(numbers[i]>max){
                    max=numbers[i];
                }
                if(numbers[i]!=0 && numbers[i]<min){
                    min=numbers[i];
                }
            }
            if(max-min<5){
                return true;
            }else{
                return false;
            }
        }
}
```
* 数组中重复的数字
```java
在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

使用HashSet,遍历数组，使用HashSet来存储，出现重复就直接返回true.
public class Solution {
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        if(numbers==null || length==0) return false;
        HashSet<Integer> set = new HashSet<>();
        for(int i =0;i<length;i++){
            if(set.contains(numbers[i])){
                duplication[0]=numbers[i];
                return true;
            }else{
                set.add(numbers[i]);
            }
        }
        return false;
    }
}

选择排序,在排序对比的过程中，如果出现相等，就说明有重复数字，直接返回true
public class Solution {
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        if(numbers==null || length<1) return false;
        for(int i = 1;i<length;i++){
            for(int j=i;j>0;j--){
                if(numbers[j]==numbers[j-1]){
                    duplication[0] = numbers[j];
                    return true;
                }else if(numbers[j]<numbers[j-1]){
                    swap(numbers,j,j-1);
                }
            }
        }
        return false;
    }
    public void swap(int[] arr,int i,int j){
        int temp=arr[i];
        arr[i]=arr[j];
        arr[j]=temp;
    }
}

使用一个辅助数组
使用一个byte数组（每个位置占1个字节），用numbers数组中的数字作为下标，出现过的数字，
将byte[numbers[i]]设为1（原始为0）。遍历过程中，如果是1，就表明是重复出现的，直接返回true
public class Solution {
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        if(numbers==null || length<1) return false;
        byte[] bt = new byte[length];
        for(int i =0;i<length;i++){
            if(bt[numbers[i]]==1){
                duplication[0]=numbers[i];
                return true;
            }
            bt[numbers[i]]=1;
        }
        return false;
    }
}

将数字移动到对应的坐标
因为数组长度为n，而数字为0-n-1，所以以当前的数字numbers[i]为下标，都不会越界。
因此，把number[i]移动到numbers[numbers[i]]位置上，表示坐标i与该坐标上的数字numbers[i]是相等的。
利用这个方法，将每个数字都移动到相对应的坐标位置，移动时如果对应位置上的数已经等于了numbers[i]，就表示出现了重复的数字。
public class Solution {
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        if(numbers==null || length<1) return false;
        for(int i =0 ;i<length;i++){
            while(numbers[i]!= i){
                if(numbers[i] == numbers[numbers[i]]){
                    duplication[0] = numbers[i];
                    return true;
                }
                swap(numbers,i,numbers[i]);
            }
        }
        return false;
    }
    public void swap(int[] arr ,int i,int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}

在相应位置做标记
跟上一种方法类似，用当前的数做坐标index=numbers[i]，在相应的位置numbers[index]做标记，减去length（变为了负数）
如果要做标记的位置，已经是负数，表明出现了重复的数字。
注意：在遍历到numbers[index]位置时，如果这个位置被做了标记，那么要判断原来这个位置上的数字所对应位置的状态，
需要加上length才能得到原来的数字。
public class Solution {
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        if(numbers==null || length<1) return false;
        for(int i=0;i<length;i++){
            int index = numbers[i];
            if(index<0){
                index+=length;
            }
            if(numbers[index]<0){
                duplication[0]=index;
                return true;
            }
            numbers[index]-=length;
        }
        return false;
    }
    public void swap(int[] arr ,int i,int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```
* 构建乘积数组
```java
给定一个数组A[0,1,…,n-1],请构建一个数组B[0,1,…,n-1],其中B中的元素B[i]=A[0] * A[1]*…*A[i-1] * A[i+1] * … * A[n-1]。
不能使用除法

暴力解法：
public class Solution {
    public int[] multiply(int[] A) {
        if(A==null || A.length<=1) return null;
        int len = A.length;
        int[] B = new int[len];
        B[0] = res(A,1,len-1);
        B[len-1] = res(A,0,len-2);
        for(int i =1; i+1<len; i++){
            B[i] = res(A,0,i-1)*res(A,i+1,len-1);
        }
        return B;
    }
    public int res(int[] arr,int L,int R){
        int res = 1;
        while(L<=R){
            res*=arr[L];
            L++;
        }
        return res;
    }
}

第一个循环,先从左往右算，得到每个数左边的所有乘积，
第二个循环，从右往左算，得到每个数右边的所有乘积，使用temp记录
public class Solution {
    public int[] multiply(int[] A) {
        if(A==null || A.length<=1) return null;
        int len = A.length;
        int[] B = new int[len];
        B[0] = 1 ;
        for(int i = 1 ;i<len;i++){
            B[i] = B[i-1] * A[i-1];
        }
        int temp = 1;
        for(int i = len-1;i>=0;i--){
            B[i]*=temp;
            temp*=A[i];
        }
        return B;
    }
}
```
* 矩阵中的路径
```java
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。
路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。
如果一条路径经过了矩阵中的某一个格子，则之后不能再次进入这个格子。 
例如 a b c e s f c s a d e e 这样的3 X 4 矩阵中包含一条字符串"bcced"的路径，
但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

分析：
先在matrix[]中找到str[0]所在的位置i，然后从i位置开始，向上、下、左、右方向去找，
看与str的下一位是否匹配，如果匹配了，就将matrix相应的位置设为‘-’,
表示经过了这个格子，后面不能再次进入。
然后再重复向上、下、左、右方向去找的过程，直到str匹配完毕，返回true。
匹配过程中，如果发现i越界、当前位置匹配不上、i位置已经是’-’ ，就返回false。
有可能matrix[]中存在多个str[0]，可能以这个开头不行，但是以另一个开头是可以的。
因此需要在matrix[]中找到下一个str[0]，再重复上面匹配的过程。（这里注意，要将matrix中的’-'符号还原）

public class Solution {
    public boolean hasPath(char[] matrix, int rows, int cols, char[] str){
        char[] ch = copy(matrix); //先copy一个matrix，方便后面还原
        for(int i=0; i<matrix.length; i++){
            if(matrix[i] == str[0]){ //如果能找到str[0]所在的位置i
                if(match(matrix, i, rows, cols, str , 0)){ //如果能完全匹配，返回true
                    return true;
                }else{ // 上面的匹配失败，还原matrix，再去找下一个开头
                    matrix = copy(ch);
                }
            }
        }
        return false;
    }
    boolean match(char[] matrix, int i, int rows, int cols, char[] str, int j){
        if(j==str.length) //如果str匹配完了，返回true
            return true;
        if(i<0 || i>matrix.length-1 || matrix[i]!=str[j] || matrix[i]=='-') //如果越界、匹配不上、i位置已经匹配过了
            return false;
        matrix[i]='-'; //没有越界并且匹配上了，就将当前位置设为‘-’
        if(match(matrix, i-1, rows, cols, str, j+1) || //去匹配上、下、左、右的位置
           match(matrix, i+1, rows, cols, str, j+1) ||
           match(matrix, i+cols, rows, cols, str, j+1)||
           match(matrix, i-cols, rows, cols, str, j+1)){
            return true;
        }
        return false;
    }
    char[] copy(char[] matrix){
        char[] ch = new char[matrix.length];
        for(int i=0;i<ch.length; i++){
            ch[i]=matrix[i];
        }
        return ch;
    }
}

使用一个boolean矩阵来标记访问过的位置
public class Solution {
public boolean hasPath(char[] matrix, int rows, int cols, char[] str){
        boolean[] visited = new boolean[matrix.length];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (searchFromHere(matrix,rows,cols,i,j,0,str,visited))
                    return true;
            }
        }
        return false;
    }
    public boolean searchFromHere(char[] matrix,int rows,int cols,int r,int c,int index,char[] str,boolean[] visited){
        if (r < 0 || r >= rows || c < 0 || c >= cols || matrix[r * cols + c] != str[index] || visited[r * cols + c])
            return false;
        if (index == str.length - 1)    return true;
        visited[r * cols + c] = true;
        if (searchFromHere(matrix,rows,cols,r - 1,c,index + 1,str,visited) ||
                searchFromHere(matrix,rows,cols,r,c -1,index + 1,str,visited) ||
                searchFromHere(matrix,rows,cols,r + 1,c,index + 1,str,visited) ||
                searchFromHere(matrix,rows,cols,r,c + 1,index + 1,str,visited))
            return true;
        visited[r * cols + c] = false;
        return false;
    }
}
```
* 机器人的运动范围
```java
地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，
但是不能进入行坐标和列坐标的数位之和大于k的格子。 
例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。
但是，它不能进入方格（35,38），因为3+5+3+8 = 19。
请问该机器人能够达到多少个格子？

构建一个辅助数组flag[][]，如果当前位置可以到达，就将对应位置标记为1，表示已经到达，避免重复计算。
从(0,0)开始，如果可以到达，则可以到达的个数为 (1 + 上下左右方向) 可以到达的个数。
public class Solution {
    public int movingCount(int threshold, int rows, int cols){
        byte[][] flag = new byte[rows][cols];
        return count( threshold, 0, 0, rows, cols, flag);
    }
    private int count(int k, int R, int C, int rows, int cols, byte[][] flag){
        if(R > rows-1 || R<0 || C>cols-1 || C<0 || numberSum(R,C)>k || flag[R][C]==1)
            return 0;
        flag[R][C] = 1;
        return 1 + count(k, R-1, C, rows, cols, flag) +
                   count(k, R+1, C, rows, cols, flag) +
                   count(k, R, C-1, rows, cols, flag) +
                   count(k, R, C+1, rows, cols, flag);
    }
    private int numberSum(int R,int C){
        int sum1 = 0;
        int sum2 = 0;
        while(R>0){
            sum1 += (R%10);
            R /= 10;
        }
        while(C>0){
            sum2 += (C%10);
            C /= 10;
        }
        return sum1 + sum2;
    }
}
```
























## String

* 替换空格  
```java
请实现一个函数，将一个字符串中的每个空格替换成“%20”。
例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy

String的replaceAll方法：
public String replaceSpace(StringBuffer str) {
       return  str.toString().replaceAll(" ", "%20");
}

StringBuffer的append方法：
public static String replaceSpace(StringBuffer str) {
	       StringBuffer sb = new StringBuffer();
	       for(int i=0;i<str.length();i++){
	    	   if(str.charAt(i)==' '){
	    		   sb.append("%20");
	       }else{
	    	   sb.append(str.charAt(i));
	       }
	    }
	       return sb.toString();
	}
    
先计算出扩充后的新长度，然后从尾到头逐个添加，如果是空格，就直接添加3个字符，如果不是，就正常添加：
public static String replaceSpace(StringBuffer str) {
        int count=0;
        for(int i=0;i<str.length();i++){
            if(str.charAt(i)==' '){
                count++;
            }
        }
        int oldIndex=str.length()-1;
        int newIndex=str.length()+2*count-1;
        str.setLength(newIndex+1);
        for(;oldIndex>=0;--oldIndex){
            if(str.charAt(oldIndex)==' '){
                str.setCharAt(newIndex--, '0');
                str.setCharAt(newIndex--, '2');
                str.setCharAt(newIndex--, '%');
            }else{
                str.setCharAt(newIndex--,str.charAt(oldIndex));
            }
        }
        return str.toString();
    }
```
* 把数组排成最小的数
```java
输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。
例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323

public class Solution {
    public String PrintMinNumber(int [] numbers) {
        ArrayList<Integer> list = new ArrayList<>();
        for(int i=0;i<numbers.length;i++){
            list.add(numbers[i]);
        }
        Collections.sort(list,new MyComparator());
        StringBuffer res = new StringBuffer();
        for(Integer j:list){
            res.append(j);
        }
        return res.toString();
    }   
    public class MyComparator implements Comparator<Integer> {
        public int compare(Integer a, Integer b) {
            String s1=a.toString();
            String s2=b.toString();
            return (s1 + s2).compareTo(s2 + s1);
        }
    }
}
```
* 第一个只出现一次的字符
```java
在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置,
如果没有则返回 -1（需要区分大小写）

遍历字符串，使用HashMap存储出现的次数，然后再遍历一次字符串，取出第一个只出现一次的字符即可：
（注意，如果用HashMap则不能遍历HashMap来取，因为存的时候会打乱字符的顺序，可以用LinkedHashMap来存和取，
或者使用HashMap存，然后遍历原字符串的顺序来取）
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        if(str==null || str.length()==0) return -1;
        LinkedHashMap<Character,Integer> map = new LinkedHashMap<Character,Integer>();
        for(int i =0 ;i<str.length();i++){
            if(!map.containsKey(str.charAt(i))){
                map.put(str.charAt(i),1);
            }else{
                int c = map.get(str.charAt(i));
                map.put(str.charAt(i),c+1);
            }
        }
        for(int i=0;i<str.length();i++){
            if(map.get(str.charAt(i))==1){
                return i;
            }
        }
        return -1;
    }
}

使用数组，直接用字母相对应的下标来存储：
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        int[] ch = new int['z'+1];
        for(int i=0;i<str.length();i++){
            ch[str.charAt(i)- 0]++;
        }
        for(int i=0;i<str.length();i++){
            if(ch[str.charAt(i)-0]==1){
                return i;
            }
        }
        return -1;
    }
}
```
* 字符串的排列
```java
输入一个字符串,按字典序打印出该字符串中字符的所有排列。
例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。
输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

递归：
(1) 遍历出所有可能出现在第一个位置的字符（即：依次将第一个字符同后面所有字符交换）；
     在有重复值的情况下，每个数分别与它后面非重复出现的字符交换。
(2) 固定第一个字符，求后面字符的排列（即：在第1步的遍历过程中，插入递归进行实现）。
     从每个子串的第二个字符开始，依次与第一个字符交换，然后继续处理子串。
需要注意的几点：
(1) 递归的出口，就是只剩一个字符的时候。
(2) 输出的排列可能不是按字典顺序排列的，输出前需要排序。
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashSet;
public class Solution {
    public ArrayList<String> Permutation(String str) {
        ArrayList<String> list = new ArrayList<>();
        if(str!=null && str.length()>0){
            Permutation2(str.toCharArray(),0,list);
            Collections.sort(list);
        }
        return list;
    }
    //i表示i之前的字符固定不动，从i开始与后面的交换
    public void Permutation2(char[] ch,int i,ArrayList<String> list){
        if(i==ch.length-1){
            list.add(String.valueOf(ch));
        }else{
            HashSet<Character> hash = new HashSet<>();
            for(int j=i;j<ch.length;j++){ //j表示每次与i交换的位置，从i一直到末尾
                if(j==i || !hash.contains(ch[j])){ //遇到非重复的字符，才交换
                    hash.add(ch[j]);
                    swap(ch,i,j);
                    Permutation2(ch,i+1,list); //然后保持i之前的不变，从i+1开始交换
                    swap(ch,j,i);//再把i换回来，在下一次循环时，i与下下一位交换
                }
            }
        }
    }
    public void swap(char[] cs,int i,int j){
        char temp = cs[i];
        cs[i] = cs[j];
        cs[j] = temp;
    }
}

字典序排列算法：
获得所有的字典序排列，例如 得到346987521的下一个排序：
从尾部往前找第一个P(i-1) < P(i)的位置
      3 4 6 < 9 < 8 < 7 < 5 < 2 < 1 最终找到6是第一个变小的数字，记录下6的位置i-1
从i位置往后找到最后一个大于6的数
    3 4 6 > 9 > 8 > 7 5 2 1	最终找到7的位置，记录位置为m
交换位置i-1和m的值
    3 4 7 9 8 6 5 2 1
倒序i位置后的所有数据
     3 4 7 1 2 5 6 8 9
则347125689为346987521的下一个排列。
import java.util.ArrayList;
import java.util.Arrays;
public class Solution {
    public static ArrayList<String> Permutation(String str) {
        ArrayList<String> list = new ArrayList<String>();
        if(str==null || str.length()==0){
            return list;
        }
        char[] ch = str.toCharArray();
        Arrays.sort(ch);
        list.add(String.valueOf(ch));
        while(true){
            int L = ch.length-1; //从后往前
            int R ; //从前往后最后一个大于的位置
            while(L>=1 && ch[L-1]>=ch[L]){
                L--;
            }
            // 此时L-1的位置就是从后往前，第一个变小的位置
            if(L==0){
                break;
            }
            R = L;
            while(R<ch.length && ch[R]>ch[L-1]){
                R++;
            }
            //此时R-1的位置就是最后一个大于L-1位置的数
            swap(ch,L-1,R-1);
            reverse(ch,L);//交换之后，倒序L-1后面的数
            list.add(String.valueOf(ch));
        }
        return list;
    }
    private static void reverse(char[] ch, int L) {
        int R = ch.length-1;
        while(L<=R){
            swap(ch,L++,R--);
        }
    }
    public static void swap(char[] cs,int i,int j){
        char temp = cs[i];
        cs[i] = cs[j];
        cs[j] = temp;
    }
}
```
* 左旋转字符串
```java
对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。
例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”

将字符串先分割再拼接:
public class Solution {
    public String LeftRotateString(String str,int n) {
        if(str==null || str.length()<=1) return str;
        int len = str.length();
        if(n%len == 0){
            return str;
        } else{
            n%=len;
            String s1 = str.substring(0,n);
            String s2 = str.substring(n);
            str = s2.concat(s1);
            return str;
        }
    }
}

三次反转的方法
字符串abcdef，n=3，设X=abc，Y=def，所以字符串可以表示成XY，如题干，问如何求得YX。
假设X的翻转为XT，XT=cba，同理YT=fed，那么YX=(XTYT)T，三次翻转后可得结果。
public class Solution {
    public String LeftRotateString(String str,int n) {
        if(str==null || str.length()<=1) return str;
        int len = str.length();
        if(n%len == 0){
            return str;
        }
        char[] ch = str.toCharArray();
        reverse(ch,0,n-1);
        reverse(ch,n,len-1);
        reverse(ch,0,len-1);
        return new String(ch);
    }
    public void reverse(char[] ch,int L,int R){
        while(L<R){
            char temp = ch[L];
            ch[L] = ch[R];
            ch[R] = temp;
            L++;
            R--;
        }
    }
}
```
* 翻转单词顺序
```java
给定一个句子，它的顺序是翻转的。例如，“student a am I”，正确的句子应该是“I am a student”。
给定一个字符串，把它翻转回正确是顺序。

先分割，再按倒序拼接（可以转为数组，也可以使用栈实现）
public class Solution {
   public String ReverseSentence(String str) {
        if(str==null || str.length()==0 || str.trim().length()==0) return str;
		String[] arr = str.split(" ");
		String s = "";
		for(int i =arr.length-1;i>0;i--){
			s+=arr[i];
			s+=" ";
		}
		s+=arr[0];
		return s;
	}
}

先将整个字符串翻转，再对每个单词翻转
public class Solution {
   public String ReverseSentence(String str) {
         if(str==null || str.trim().length()==0) return str;
        int len = str.length();
        char[] ch = str.toCharArray();
        reverse(ch,0,len-1);
        int L=0;
        int R=0;
        while(R<len){
        	if(R==len-1){
        		reverse(ch,L,R);
        	}
        	if(ch[R]==' '){
        		reverse(ch,L,R-1);
        		R+=1;
        		L=R;
        	}else{
        		R++;
        	}
        }
        return new String(ch);
	}
     public static void reverse(char[] ch,int L,int R){
        while(L<R){
            char temp = ch[L];
            ch[L] = ch[R];
            ch[R] = temp;
            L++;
            R--;
        }
    }
}
```
* 把字符串转换成整数
```java
将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，
要求不能使用字符串转换整数的库函数。数值为0或者字符串不是一个合法的数值则返回0。
输入描述:
输入一个字符串,包括数字字母符号,可以为空
输出描述:
如果是合法的数值表达则返回该数字，否则返回0
示例:
输入
+2147483647	1a33
输出
2147483647	0

将字符串转换为字符数组，先判断第一位是否是符号“+”或“-”，记录下来。
然后在加值的时候，先把之前数值乘以10
public class Solution {
    public int StrToInt(String str) {
        if(str==null || str.length()==0) return 0;
        char[] ch = str.toCharArray();
        int j =0;
        boolean flag = true;
        if(ch[j]=='+'){
            j++;
        }else if(ch[j]=='-'){
            flag=false;
            j++;
        }
        int sum = 0;
        while(j<ch.length){
            if(ch[j]>'9'||ch[j]<'0'){
                return 0;
            }else{
                //sum *=10;
                sum =(sum<<1)+(sum<<3);// sum*2 + sum*8 = sum*10
                sum+=(ch[j]-'0');
                j++;
            }
        }
        return flag?sum:-sum;
    }
}
```
* 表示数值的字符串
```java
请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。
例如，字符串"+100",“5e2”,"-123",“3.1416"和”-1E-16"都表示数值。 
但是"12e",“1a3.14”,“1.2.3”,"±5"和"12e+4.3"都不是。

对字符串逐个判断
public class Solution {
    public boolean isNumeric(char[] str) {
        if(str==null || str.length==0) return false;
        boolean sign = false; //标记+-号
        boolean decimal = false; //标记小数点
        boolean hasE = false; // 标记e
        for(int i = 0 ;i < str.length; i++){
            if(str[i]=='e' || str[i]=='E'){
                if(i==str.length-1) return false;//e后面必须接数字
                if(hasE) return false;//不能有2个e
                hasE = true;
            }else if(str[i]=='-' || str[i]=='+'){
                //如果有第二个+-号，则必须紧跟在e后面
                if(sign && str[i-1]!='e' && str[i-1]!='E') return false ;
                //如果是第一个+-号，且不在开头，则必须紧跟在e后面
                if(!sign && i>0 &&  str[i-1]!='e' && str[i-1]!='E') return false;
                sign=true;
            }else if(str[i]=='.'){
                //e后面不能有小数点，小数点不能出现2次
                if(hasE || decimal) return false;
                decimal = true;
            }else if(str[i]<'0' || str[i]>'9'){
                return false;
            }
        }
        return true;
    }
}

使用正则表达式
public class Solution {
    public boolean isNumeric(char[] str) {
        String string = String.valueOf(str);
        return string.matches("[\\+-]?[0-9]*(\\.[0-9]*)?([eE][\\+-]?[0-9]+)?");
    }
}
[\\+-]?   表示正或负符号出现与否
[0-9]*    表示整数部分是否出现，如-.34 或 +3.34均符合
(\\.[0-9]*)?  	如果出现小数点，那么小数点后面必须有数字；否则一起不出现
([eE][\\+-]?[0-9]+)?	如果存在指数部分，那么e或E肯定出现，+或-可以不出现，紧接着必须跟着整数；
			或者整个部分都不出现

如果可以转换成double类型，说明字符串是一个数值
public class Solution {
    public boolean isNumeric(char[] str) {
        try {
            double re = Double.parseDouble(new String(str));
        } catch (NumberFormatException e) {
            return false;
        }
        return true;
    }
}
```
* 字符流中第一个不重复的字符
```java
请实现一个函数用来找出字符流中第一个只出现一次的字符。
例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。
当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

借助数组和另一辅助空间
public class Solution {
    //Insert one char from stringstream
    private byte[] bt = new byte[128];
    StringBuffer st = new StringBuffer();
    public void Insert(char ch){
        if(bt[ch]==0){ // 第一次出现
            bt[ch]++; 
            st.append(ch); // 添加到st中
        }else if(bt[ch]==1){ // 第二次出现
            bt[ch]++;
            String s = ch+"";
            int index = st.indexOf(s);
            st.deleteCharAt(index); // 从st中删除
        }else{
            bt[ch]++;
        }
    }
  //return the first appearence once char in current stringstream
    public char FirstAppearingOnce(){
        if(st.length()==0) return '#';
        return st.charAt(0);
    }
}

只使用数组
如果第一次出现，将bt[ch]设置为index(字符出现的顺序)，是大于0的，
如果重复出现了，就将bt[ch]设置为-1.
FirstAppearingOnce()的原理是，每次遍历数组，得到值大于0且index最小的那个位置i，
再把i强转为char类型，就是要返回的字符
public class Solution {
    //Insert one char from stringstream
    private byte[] bt = new byte[128];
    byte index = 1;
    public void Insert(char ch){
        if(bt[ch]==0){
            bt[ch] = index;
        }else if(bt[ch]>0){
            bt[ch] = -1;
        }
        index++;
    }
  //return the first appearence once char in current stringstream
    public char FirstAppearingOnce(){
        int min = Integer.MAX_VALUE;
        char res = '\0';
        for(int i =0;i<128;i++){
            if(bt[i]>0 && bt[i]<min){
                min = bt[i];
                res = (char)i;
            }
        }
        if(res =='\0') return '#';
        return res;
    }
}
```
* 正则表达式匹配
```java
请实现一个函数用来匹配包括’.‘和’ * ’ 的正则表达式。
模式中的字符’.‘表示任意一个字符，而’ * '表示它前面的字符可以出现任意次（包含0次）
在本题中，匹配是指字符串的所有字符匹配整个模式。
例如，字符串"aaa"与模式"a.a"和"ab * ac * a"匹配，但是与"aa.a"和"ab*a"均不匹配。

分两种情况：pattern下一个字符为*或不为*：
1.pattern下一个字符不为‘ * ’：这种情况比较简单，直接匹配当前字符。
如果匹配成功，继续匹配下一个；如果匹配失败，直接返回false。
注意这里的“匹配成功”，除了两个字符相同的情况外，还有一种情况，就是pattern的 当前字符为‘.’
2.pattern下一个字符为‘ * ’时，稍微复杂一些，因为‘ * ’可以代表0个或多个。这里把这些情况都考虑到：
a>当‘ * ’匹配0个字符时，str当前字符不变，pattern当前字符后移2位，跳过这个‘ x* ’；
b>当‘ * ’匹配1个时，str当前字符移向下一个，pattern当前字符后移2位。
c>当‘ * ’匹配多个时，str当前字符移向下一个，pattern当前字符不变。

递归法：
public class Solution {
    public boolean match(char[] str, char[] pattern) {
    if (str == null || pattern == null) {
        return false;
        }
    return ismatch(str, 0, pattern, 0);
    } 
    public boolean ismatch(char[] str,int i,char[] pattern,int j){
        if(j==pattern.length){ //j到了末尾
            return i==str.length; //i必须也到末尾才为true,否则为false
        }
        if(j+1 < pattern.length && pattern[j+1]=='*' ){ // j+1不越界并且为*
            if((i!=str.length && str[i]==pattern[j]) ||( pattern[j]=='.' && i!=str.length)){ // 当前位能匹配上
                return ismatch(str,i,pattern,j+2) || ismatch(str,i+1,pattern,j+2) || ismatch(str,i+1,pattern,j);
            }else{ // 当前位匹配不上
                return ismatch(str,i,pattern,j+2);
            }
        }
        // j+1越界了 或者下一位不是*，那么必须当前位是匹配的
        if((i!=str.length && str[i]==pattern[j] )|| (pattern[j]=='.' && i!=str.length)){
            return ismatch(str,i+1,pattern,j+1);
        }
        return false;
    }
}
```




















## LinkedList

* 从尾到头打印链表
```java
输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

先反转链表，再逐个添加：
public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        if(listNode==null) return list;
        ListNode next =null;
        ListNode pre=null;
        while(listNode.next!=null){
            next=listNode.next;
            listNode.next=pre;
            pre=listNode;
            listNode=next;
        }
        listNode.next=pre;
        while(listNode!=null){
            list.add(listNode.val);
            listNode=listNode.next;
        }
        return list;
    }

ArrayList的add(index,value)方法，在Index位置添加，会把index及其之后的元素往后移动
（如果在0位置，就相当于每次添加的在头部）：
public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<>();
        while(listNode != null){
            list.add(0,listNode.val);
            listNode = listNode.next;
        }
        return list;
    }
    
递归：
public class Solution {
    ArrayList<Integer> list = new ArrayList<>();
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        if(listNode!=null){
            printListFromTailToHead(listNode.next);
            list.add(listNode.val);
        }
        return list;
    }
}

用栈来实现：从头到尾遍历链表，依次将节点的val入栈，添加完后，再依次出栈加入list中
```

* 链表中倒数第k个结点
```java
输入一个链表，输出该链表中倒数第k个结点

使用栈压入，然后取出第k个：
public ListNode FindKthToTail(ListNode head,int k) {
        if(head==null) return null;
        if(k<=0) return null;
        Stack<ListNode> st = new Stack<ListNode>();
        while(head!=null){
            st.push(head);
            head=head.next;
        }
        for(int i=1;i<k;i++){
            if(!st.empty()){
                 st.pop();
            }else{
               return null;
            }
        }
        if(!st.empty()){
            return st.peek();
        }
        return null;
    }
    
定义快指针和慢指针。
快指针先走 k-1 步，到达第 k 个节点。
然后两指针同时齐步走，当快指针到达末尾时，此时慢指针就在倒数第k个节点上
public ListNode FindKthToTail(ListNode head,int k) {
        if(head==null) return null;
        if(k<=0) return null;
        ListNode fast = head;
        ListNode slow = head;
        for(int i=1;i<k;i++){
            if(fast.next!=null){
                fast=fast.next;
            }else{
                return null;
            }
        }
        while(fast.next!=null){
            fast=fast.next;
            slow=slow.next;
        }
        return slow;
    }
```

* 反转链表
```java
输入一个链表，反转链表后，输出新链表的表头

public ListNode ReverseList(ListNode head) {
        if(head==null)
            return null;
        ListNode temp=null;
        ListNode pre=null;
        while(head!=null){
            temp=head.next;
            head.next=pre;
            pre=head;
            head=temp;
        }
        return pre;
    }
    
递归：
public ListNode ReverseList(ListNode head) {
        if(head==null || head.next==null) return head;
        ListNode pre = ReverseList(head.next);
        head.next.next=head;
        head.next=null;
        return pre;
    }
```

* 合并两个排序的链表
```java
输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

递归：
public ListNode Merge(ListNode list1,ListNode list2) {
        if(list1==null) 
            return list2;
        if(list2==null)
            return list1;
        //ListNode res=null
        //递归
        if(list1.val<=list2.val){
            //res=list1;
            //res.next=Merge(list1.next,list2)
             list1.next=Merge(list1.next,list2); //这里可以直接用list1作为res
            return list1;
        }else{
            list2.next=Merge(list1,list2.next);
            return list2;
        }
}

非递归
public ListNode Merge(ListNode list1,ListNode list2) {
        if(list1==null)
            return list2;
        if(list2==null)
            return list1;
        ListNode head=new ListNode(-1);
        ListNode current=head;
        while(list1!=null && list2!=null){
            if(list1.val<=list2.val){
                current.next=list1;
                list1=list1.next;
            }else{
                current.next=list2;
                list2=list2.next;
            }
            current=current.next;
        }
        if(list1==null){
                current.next=list2;
            }
        if(list2==null){
                current.next=list1;
            }
        return head.next;
    }
```
* 复杂链表的复制
```java
输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），
返回结果为复制后复杂链表的head

使用一个哈希表，来存储原节点和拷贝节点:
public class Solution {
    public RandomListNode Clone(RandomListNode pHead){
        HashMap<RandomListNode, RandomListNode> map = new HashMap<>();
        RandomListNode cur = pHead;
        while(cur!=null){ //遍历链表，并拷贝，然后都存到哈希表中
            map.put(cur,new RandomListNode(cur.label));
            cur=cur.next;
        }
        cur=pHead;
        while(cur!=null){
            map.get(cur).next = map.get(cur.next);//连接拷贝的当前节点的next -> 拷贝的next
            map.get(cur).random = map.get(cur.random);//连接拷贝的当前节点的rand -> 拷贝的rand
            cur=cur.next;
        }
        return map.get(pHead);//返回拷贝的第一个节点
    }
}

1、遍历链表，复制每个结点，如复制结点A得到A1，将结点A1插到结点A后面；B1查到B后面...
2、重新遍历链表，复制老结点的随机指针给新结点，如A1.random = A.random.next;
3、拆分链表，将链表拆分为原链表和复制后的链表
public class Solution {
    public RandomListNode Clone(RandomListNode pHead){
        if(pHead==null) return null;
        RandomListNode cur=pHead;
         RandomListNode next=null;
        while(cur!=null){
            next=cur.next;  //存储下一个节点
            cur.next=new  RandomListNode(cur.label); //让1的next指向1'，1'指向2,2指向2' ...
            cur.next.next=next; //1'指向2
            cur=next;
        }
        cur = pHead;
        RandomListNode copy=null; 
        while(cur!=null){ //每次拿出1个节点和它的复制节点，如1和1'，2和2'
            next=cur.next.next;
            copy=cur.next;
	    //cur.rand表示原来的随机节点，cur.rand.next表示它的复制
            copy.random=cur.random!=null?cur.random.next:null;
            cur=next;
        }
         RandomListNode res=pHead.next;
        cur=pHead;
        while(cur!=null){ //把这个大链表分离
            next=cur.next.next;
            copy=cur.next; //cur.next表示是复制的节点
            cur.next=next; //cur是原来的节点
            copy.next=next!=null?next.next:null;
            cur=next;
        }
        return res;
    }
}
```
* 两个链表的第一个公共结点
```java
输入两个链表，找出它们的第一个公共结点。(这个题是无环的情况下)

使用HashMap存储链表1的每个结点，然后遍历链表2的结点，看map中是否包含这个结点，第一个找到的就是第一个公共结点：
(用HashMap而不用HashSet的原因是HashMap内部是红黑树，时间复杂度更低)
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        HashMap<ListNode,Integer> map = new HashMap<>();
        while(pHead1!=null){
            if(map.containsKey(pHead1)){
                break;
            }
            map.put(pHead1,pHead1.val);
            pHead1 = pHead1.next;
        }
        while(pHead2!=null){
            if(map.containsKey(pHead2)){
                return pHead2;
            }
            pHead2 = pHead2.next;
        }
        return null;
    }
}

分别得到链表1和链表2的长度，然后让较长的那个链表先移动m步（m为2个链表的长度差），
然后2个链表一起移动，直到第一个公共结点
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        int len1 = getLength(pHead1);
        int len2 = getLength(pHead2);
        while(len1>len2){
            pHead1 = pHead1.next;
            len1--;
        }
        while(len1<len2){
            pHead2 = pHead2.next;
            len2--;
        }
        while(pHead1!=null){
            if(pHead1==pHead2){
                return pHead1;
            }else{
                pHead1 = pHead1.next;
                pHead2 = pHead2.next;
            }
        }
        return null;
    }
    public int getLength(ListNode head){
        int len = 0;
        while(head!=null){
            len++;
            head = head.next;
        }
        return len;
    }
}
```
* 圆圈中最后剩下的数
```java
有n个人，围成一个圆圈，随机指定一个数m。然后从0开始报数，报到m-1的人出列，
然后从下一个人开始又从0开始报数，直到剩下最后一个人。
求最后剩下的人的编号。（编号从0到n-1）

使用一个链表，每次从Index开始数，数到index+m-1，
得到要出列的位置是（index+m-1）%list.size()，表示如果数到末尾，再从头开始数。
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        LinkedList<Integer> list = new LinkedList<Integer>();
        for(int i=0;i<n;i++){
            list.add(i);
        }
        int index = 0;
        while(list.size()>1){
            index = (index + m -1) % list.size();
            list.remove(index);
        }
        return list.size()==1?list.get(0):-1;
    }
}

利用公式，f(n)=( f(n-1)+m ) % n
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        if(n==0) return -1;
        if(n==1) return 0;
        return (LastRemaining_Solution(n-1,m) + m)% n;
    }
}

使用数组模拟环
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        if(n<1 || m<1) return -1;
        int[] arr = new int[n];
        int step=0;  // 计数，数到m就归0
        int i = -1; //表示当前位置
        int count = n; //剩余的人数
        while(count>0){ 
            i++;  //下移一位
            if(i>=n) i=0; //当i到达末尾，就从0开始
            if(arr[i]==-1) continue;  // -1表示当前位置已经被删除，就直接跳过，不计数
            step++;  /
            if(step == m){ // 当计数到了m，就删除当前位置
                arr[i] = -1; 
                step=0;
                count--;
            }
        }
        return i;
    }
}
```
* 链表中环的入口结点
```java
给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

使用HashSet存储结点，如果已经存在，这个结点就是入环结点
public class Solution {
    public ListNode EntryNodeOfLoop(ListNode pHead){
        HashSet<ListNode> set = new HashSet<>();
        while(pHead.next!=null){
            if(set.contains(pHead)){
                return pHead;
            }
            set.add(pHead);
            pHead=pHead.next;
        }
        return null;
    }
}

设置一个慢指针每次走1步，一个快指针每次走2步，如果有环，一定会在环内相遇。
然后让一个指针回到起点，起点开始走，让一个指针从相遇点开始继续往后走，
2个指针速度一样，那么，当从原点的指针走到环入口点的时候，从相遇点开始走的那个指针也一定刚好到达环入口点。
所以2者会相遇，且恰好相遇在环的入口点。
public class Solution {
    public ListNode EntryNodeOfLoop(ListNode pHead){
        if(pHead == null || pHead.next==null || pHead.next.next==null) return null;
        ListNode node1 = pHead.next;
        ListNode node2 = pHead.next.next;
        while(node1 != node2) {
            if(node1.next!=null && node2.next.next!=null){
                node1 = node1.next;
                node2 = node2.next.next;
            }else{
                return null;
            }
        }
        node1 = pHead;
        while(node1!=node2){
            node1=node1.next;
            node2=node2.next;
        }
        return node1;
    }
}
```
* 删除链表中重复的结点
```java
在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。
例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

递归：
public class Solution {
    public ListNode deleteDuplication(ListNode pHead) {
        if (pHead == null || pHead.next == null) { // 只有0个或1个结点，则返回
            return pHead;
        }
        if (pHead.val == pHead.next.val) { // 当前结点是重复结点
            ListNode pNode = pHead.next;
            while (pNode != null && pNode.val == pHead.val) {
                // 跳过值与当前结点相同的全部结点,找到第一个与当前结点不同的结点
                pNode = pNode.next;
            }
            return deleteDuplication(pNode); // 从第一个与当前结点不同的结点开始递归
        } else { // 当前结点不是重复结点
            pHead.next = deleteDuplication(pHead.next); // 保留当前结点，从下一个结点开始递归
            // 表示将当前节点与下一节点不同，所以要将下一节点放入递归程序去参加下一轮的比较，这样就将pHead保留了下来。
            // 返回值给pHead.next表示deleteDuplication返回了一个无重复的节点，所以要让当前节点指向它。
            return pHead;
        }
    }
}

新建一个头结点
public class Solution {
    public ListNode deleteDuplication(ListNode pHead) {
        if (pHead == null || pHead.next == null) { // 只有0个或1个结点，则返回
            return pHead;
        }
        ListNode head = new ListNode(-1);
        head.next = pHead;
        ListNode res = head;  // 指向确定不重复的结点
        ListNode cur = head.next; // 遍历的当前节点
        while(cur!=null){
            if(cur.next!=null && cur.val == cur.next.val){ //如果是重复结点
                while(cur.next!=null && cur.val == cur.next.val){
                    cur = cur.next;
                }
                res.next = cur.next; // 找到下一个不重复的结点，链上res
                cur = cur.next;
            }else{ 
                res = res.next;
                cur = cur.next;
            }
        }
        return head.next;
    }
}
```





























## StackQueue

* 用2个栈实现队列
```java
public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
     
    public void push(int node) {
        stack1.push(node);
    }
     
    public int pop() {
        if(stack1.empty()&&stack2.empty()){
            throw new RuntimeException("empty");
        }else if(stack2.empty()){
            while(!stack1.empty()){
                stack2.push(stack1.pop());
            }
        }   
        return stack2.pop();
    }
}


public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    public void push(int node) {
        stack1.push(node);
    }       
    public int pop() {
        while(!stack1.isEmpty()){
            stack2.push(stack1.pop());
        }
        int value = stack2.pop();
        while(!stack2.isEmpty()){
            stack1.push(stack2.pop());
        }
        return value;
    }
}
```
* 包含min函数的栈
```java
定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））

用2个栈，一个栈用来正常存储st1，另一个用来存当前的最小值st2。
逻辑是：如果st2的栈顶大于要进栈的数，就st1、st2都添加；如果st2的栈顶小于这个数，st2就再添加一次自己的栈顶元素
public class Solution {
    Stack<Integer> st1=new Stack<Integer>();
    Stack<Integer> st2=new Stack<Integer>();
    public void push(int node) {
        st1.push(node);
        if(!st2.empty()){
            if(node>st2.peek()){
                st2.push(st2.peek());
            }else{
                st2.push(node);
            }
        }else{
            st2.push(node);
        }
    }  
    public void pop() {
        if(st1.empty())
            throw new RuntimeException("Your stack is empty.");
        st2.pop();
        st1.pop();
    }   
    public int top() {
        if(st1.empty())
            throw new RuntimeException("Your stack is empty.");
        return st1.peek();
    }   
    public int min() {
         if(st1.empty())
            throw new RuntimeException("Your stack is empty.");
         return st2.peek();
    }
}

优化：辅助栈存储的是最小值的索引（这样就不用重复存最小值，只需要存一个索引）
public class MinStack {
    private List<Integer> data = new ArrayList<Integer>();
    private List<Integer> mins = new ArrayList<Integer>();
    public void push(int num) {
        data.add(num);
        if(mins.size() == 0) {
            // 初始化mins
            mins.add(0);
        } else {
            // 辅助栈mins push最小值的索引
            int min = getMin();
            if (num < min) {
                mins.add(data.size() - 1);
            }
        }
    }
    public int pop() {
        // 栈空，抛出异常
        if(data.size() == 0) {
            throw new EmptyStackException();
        }
        // pop时先获取索引
        int popIndex = data.size() - 1;
        // 获取mins栈顶元素，它是最小值索引
        int minIndex = mins.get(mins.size() - 1);
        // 如果pop出去的索引就是最小值索引，mins才出栈
        if(popIndex == minIndex) {
            mins.remove(mins.size() - 1);
        }
        return data.remove(data.size() - 1);
    }
    public int getMin() {
        // 栈空，抛出异常
        if(data.size() == 0) {
            throw new EmptyStackException();
        }
        // 获取mins栈顶元素，它是最小值索引
        int minIndex = mins.get(mins.size() - 1);
        return data.get(minIndex);
    }
}
```

* 栈的压入、弹出序列
```java
输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。
例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。
（注意：这两个序列的长度是相等的）

思路:借用一个辅助的栈，遍历压栈顺序，先讲第一个放入栈中，这里是1，然后判断栈顶元素是不是出栈顺序的第一个元素，这里是4，
很显然1≠4，所以我们继续压栈，直到相等以后开始出栈，出栈一个元素，则将出栈顺序向后移动一位，直到不相等，
这样循环等压栈顺序遍历完成，如果辅助栈还不为空，说明弹出序列不是该栈的弹出顺序。
public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        if(pushA.length==0 || popA.length==0)
            return false;
        Stack<Integer> st = new Stack<Integer>();
        for(int i=0,j=0;i<pushA.length;i++){
            st.push(pushA[i]);
            while(!st.empty() && st.peek()==popA[j]){
                    st.pop();
                    j++;
                }
            }
        return st.empty();
    }
}
```
* 滑动窗口的最大值
```java
给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。
例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}；
针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}，
{2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。
注意：当size>数组长度时，认为没有最大值

每滑动一次，都遍历当前窗口内的数字，找到最大值
import java.util.*;
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size){
        ArrayList<Integer> res = new ArrayList<>();
        if(num==null || num.length==0 || size==0 || size>num.length) 
            return res;
        for(int i=0; i+size-1<num.length; i++){
            res.add(maxNum(num, i, i+size-1 ));
        }
        return res;
    }
    private int maxNum(int[] num, int L, int R){
        int max = num[L];
        for(int i = L;i<=R;i++){
            if(num[i] > max){
                max = num[i];
            }
        }
        return max;
    }
}

使用双端队列：
队头用来记录当前窗口的最大值，队尾用来加入数字，新增加的值从队尾开始比较，把所有比他小的值丢掉
（因为当前数添加的较晚，并且还比队尾的数字大，所以队尾的数不可能是窗口内的最大值）。
返回最大值时从队头开始，要先判断是否过期了（不在窗口内），过期就删除。
import java.util.*;
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size){
        ArrayList<Integer> res = new ArrayList<Integer>();
        if(num==null || num.length==0 || size==0 || size>num.length)
            return res;
        LinkedList<Integer> list = new LinkedList<Integer>();
        for(int i=0; i<size-1; i++){ // 先添加0-size-1个数，此时不输出最大值
            while(!list.isEmpty() && num[list.getLast()]<num[i]){
                list.removeLast();
            }
            list.add(i);
        }
        for(int i=size-1; i<num.length; i++){ // 此时开始输出最大值
            while(!list.isEmpty() && num[list.getLast()] < num[i]){
                list.removeLast();
            }
            list.add(i);
            while(list.getFirst()+size-1 < i){ // 判断队头元素是否过期
                list.removeFirst();
            }
            res.add(num[list.getFirst()]);
        }
        return res;
    }
}
```





























## DP

* 斐波那契数列
```java
大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）

动态规划：
public int Fibonacci(int n) {
        if(n<=1) return n;
        int m1=0;
        int m2=1;
        int sum=0;
        for(int i=2;i<=n;i++){
            sum=m1+m2;
            m1=m2;
            m2=sum;
        }
        return sum;
    }
```

* 跳台阶
```java
一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）

递归版本
public class Solution {
    public int JumpFloor(int target) {
    	if(target<=0)
		return 0;
        if(target==1)
            return 1;
        if(target==2)
            return 2;
        return JumpFloor(target-1)+JumpFloor(target-2);
    }
}

动态规划：
public class Solution {
    public int JumpFloor(int target) {
        if(target ==1 ||target==2)
            return target;
        int n1=1;
        int n2=2;
        int count=0;
        for(int i=3;i<=target;i++){ //每个i台阶依赖i-1和i-2的方案
            count=n1+n2;
            n1=n2;
            n2=count;
        }
        return n2;   
    }
}
```

* 变态跳台阶
```java
一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

递归版：
public class Solution {
    public int JumpFloorII(int target) {
        int sum=0;
        if(target<=0)
            return 0;
        if(target<=2)
            return target;
        for(int i=1;i<target;i++){
            sum +=JumpFloorII(target-i);
        }
        return sum+1;
    }
}
递归版2：
public class Solution {
    public int JumpFloorII(int target) {
        if(target<=0)
            return 0;
        if(target<=2)
            return target;
        return 2*JumpFloorII(target-1);
    }
}

动态规划：
public class Solution {
    public int JumpFloorII(int target) {
        if(target<=2)
            return target;
        int[] dp=new int[target+1];
        dp[0]=0;
        dp[1]=1;
        dp[2]=2;
        for(int i=3;i<=target;i++){
            int j=i;
            while(j>0){
                dp[i]+=dp[--j];
            }
            dp[i]+=1;
        }
        return dp[target];
    }
}

数学法：
public class Solution {
    public int JumpFloorII(int target) {
      return 1<<--target;
      }
}
```

* 小矩形覆盖大矩形
```java
我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

斐波那契数列：
public class Solution {
    public int RectCover(int target) {
        if(target<=2) return target;
        int m1=1;
        int m2=2;
        for(int i=3;i<=target;i++){
            m2=m1+m2;
            m1=m2-m1;
        }
        return m2;
    }
}
```




















## Tree
* 重建二叉树
```java
输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。
假设输入的前序遍历和中序遍历的结果中都不含重复的数字。
例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

分析：
首先先序遍历的第一个数是整棵树的根节点，然后拿这个根节点去中序遍历中找到对应的位置。
中序遍历中根节点左边是左子树上的点，右边是右子树上的点。
根据上面的规则去递归即可。
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        TreeNode root = reConstructBinaryTree(pre,0,pre.length-1,in,0,in.length-1);
        return root;
    }
     private TreeNode reConstructBinaryTree(int [] pre,int startPre,int endPre,int [] in,int startIn,int endIn) {
         if(startPre>endPre || startIn>endIn)
             return null;
         TreeNode root = new TreeNode(pre[startPre]);  // 创建根节点
         for(int i = startIn;i<=endIn;i++){
             if(in[i] == pre[startPre]){ //在中序遍历中找到根节点的位置 i
             // 左子树范围，在pre数组中是[startPre+1,startPre+i-startIn],在in数组中是[startIn,i-1]
                 root.left = reConstructBinaryTree(pre, startPre+1, startPre + i - startIn, in , startIn , i-1);
             // 右子树范围，在pre数组中是[i-startIn+startPre+1,endPre],在in数组中是[i+1,endIn]
                 root.right= reConstructBinaryTree(pre, i-startIn + startPre+1, endPre, in , i+1 , endIn);
                 break;
             }
         }
         return root;
    }
}
```
* 树的子结构
```java
输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

当root1和root2都不为null时，才开始判断，否则直接为false
如果在root1中找到了对应的root2根节点，则进入匹配判断，如果完全匹配则返回true。
如果匹配不上，那么就到root1的左子树去找，如果还匹配不上，就到root1的右子树去找。
匹配的逻辑是：
如果root2遍历完了，说明匹配完了，返回true;
如果root1遍历完了，说明匹配不够，返回false;
如果root1.val!=root2.val，返回false;
如果当前节点的值相等，那么还需要再去判断左子树和右子树，每个节点都相等才返回true。

方法一：递归
import java.util.*;
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        boolean flag = false;
        // 当root1和root2都不为null时，才开始判断
        if(root1!=null && root2!=null){
            if(root1.val==root2.val){ // 找到了对应的根节点，进入判断
                flag=match(root1,root2);
            }
            if(!flag){ // 匹配失败，到root1的左子树去找
                flag=HasSubtree(root1.left,root2);
            }
            if(!flag){ // 匹配失败，再到root2的右子树去找
                flag=HasSubtree(root1.right,root2);
            }
        }
        return flag;
    }
    private boolean match(TreeNode root1,TreeNode root2){
        if(root2==null){
            return true;
        }
        if(root1==null){
            return false;
        }
        if(root1.val!=root2.val)
            return false;
        return match(root1.left,root2.left) && match(root1.right , root2.right);
    }
}

方法二：遍历root1
import java.util.*;
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if(root2==null)
            return false;
        Stack<TreeNode> stack1 = new Stack<>();
        if(root1!=null){
            stack1.push(root1);
            while(!stack1.isEmpty()){
                root1 = stack1.pop();
                if(root1.val==root2.val){ // 找到对应的根节点
                    if(match(root1,root2)){ // 进入判断，匹配上了就直接返回true
                        return true;
                    }
                }
                if(root1.right!=null){ // 否则继续遍历root1，再找下一个与root2相等的节点
                    stack1.push(root1.right);
                }
                if(root1.left!=null){
                    stack1.push(root1.left);
                }
            }
        }
        return false;
    }
    private boolean match(TreeNode root1,TreeNode root2){
        if(root2==null){
            return true;
        }
        if(root1==null){
            return false;
        }
        if(root1.val!=root2.val)
            return false;
        return match(root1.left,root2.left) && match(root1.right , root2.right);
    }
}
```
* 二叉树的镜像
```java
操作给定的二叉树，将其变换为源二叉树的镜像。
二叉树的镜像定义：

源二叉树 
    	    8
    	   /  \     
    	  6   10
    	 / \  / \
    	5  7 9  11
镜像二叉树
    	    8
    	   /  \
    	  10   6
    	 / \  / \
    	11 9 7   5

递归版：
import java.util.*;
public class Solution {
    public void Mirror(TreeNode root) {
        if(root==null)
            return ;
        if(root.left==null && root.right==null)
            return;
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        if(root.left!=null)
            Mirror(root.left);
        if(root.right!=null)
            Mirror(root.right);
    }
}

非递归版：
public class Solution {
    public void Mirror(TreeNode root) {
        if(root==null)
            return ;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode node = stack.pop();
            if(node.left!=null || node.right!=null){
                TreeNode temp = node.left;
                node.left = node.right;
                node.right = temp;
            }
            if(node.left!=null)
                stack.push(node.left);
            if(node.right!=null)
                stack.push(node.right);
        }
    }
}
```
* 从上往下打印二叉树
```java
从上往下打印出二叉树的每个节点，同层节点从左至右打印。

利用队列FIFO的特性，创建一个队列，然后依次添加左、右子节点。
从队列中取的时候，就相当于在每一层从左到右的取节点
public class Solution {
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        ArrayList<Integer> list = new ArrayList<>();
        if(root==null)
            return list;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            root = queue.poll();
            if(root.left!=null){
                queue.offer(root.left);
            }
            if(root.right!=null){
                queue.offer(root.right);
            }
            list.add(root.val);
        }
        return list;
    }
}
```
* 二叉搜索树的后序遍历序列
```java
输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。
假设输入的数组的任意两个数字都互不相同。

后序遍历的最后一个节点是根节点，前面的可以分为2部分，大于根节点（右子树）和小于根节点（左子树）。
左、右子树的后序遍历也符合这个规律，即最后一个节点是根节点，前面的分为大、小两部分。
根据分析，这可以是一个递归。
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        if(sequence==null || sequence.length==0)
            return false;
        return post(sequence,0,sequence.length-1);       
    }   
    private boolean post(int[] arr, int L, int R){
        if(L>=R)
            return true;
        int index = R - 1 ;
        while( (index>=L) && (arr[index]>arr[R]) ){ //找到左子树的根节点（全部小于根节点）
                index--;
        }
        for(int j=L; j<index; j++){ //从L-index是左子树的范围，应满足全部小于根节点
            if(arr[j] > arr[R])
                return false;
        }
        return post(arr,L,index) && post(arr,index+1,R-1); //递归判断左、右子树
    }
}
```
* 二叉树中和为某一值的路径
```java
输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。
路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

递归遍历：
public class Solution {
    ArrayList<ArrayList<Integer>> res = new ArrayList<>();
    ArrayList<Integer> path = new ArrayList<>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        if (root == null) {
            return res;
        }
        findPath(root, target);
        return res;
    }    
    public void findPath(TreeNode root, int target) {
        //因为FindPath中和 下面程序中都进行了判null操作，root绝对不可能为 null
        path.add(root.val);
        //已经到达叶子节点，并且正好加出了target
        if (root.val == target && root.left == null && root.right == null) {
            //将该路径加入res结果集中
            res.add(new ArrayList(path));
        }
        //如果左子树非空，递归左子树
        if (root.left != null) {
            findPath(root.left, target - root.val);
        }
        //如果右子树非空，递归右子树
        if (root.right != null) {
            findPath(root.right, target - root.val);
        }
        //无论当前路径是否加出了target，必须去掉最后一个，然后返回父节点，去查找另一条路径，最终的path肯定为null
        path.remove(path.size() - 1);
        return;
    }
}

优化：
public class Solution {
    ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
    ArrayList<Integer> list = new ArrayList<>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        if(root==null )
            return res;
        target -= root.val;
        if(target>=0){ // 如果 target<0 直接返回
            list.add(root.val);
            if(target ==0 && root.left==null && root.right==null){
                res.add(new ArrayList<Integer>(list));
            }
           	FindPath(root.left,target); // 判null放在了最开始
           	FindPath(root.right,target);
           // 如果遍历到叶子节点发现此叶子节点不符合，需要将此叶节点从list里删除，回溯到叶节点的父节点
           // 这个时候符合的已经加到了res队列中，所以不会影响结果
            list.remove(list.size()-1);
        }
        return res;
    }
}
```
* 二叉搜索树与双向链表
```java
输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。
要求不能创建任何新的结点，只能调整树中结点指针的指向。

非递归 中序遍历
public class Solution {
    public TreeNode Convert(TreeNode pRootOfTree) {
        if(pRootOfTree ==null)
            return pRootOfTree;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode head = null; // 用于返回链表的头结点
        TreeNode pre = null;  // 用于保存遍历过程中的前一个节点
        while (!stack.isEmpty() || pRootOfTree != null) {
            if (pRootOfTree != null) {
                stack.push(pRootOfTree); 
                pRootOfTree = pRootOfTree.left; 
            } else {
                pRootOfTree = stack.pop();
                if(head==null){  // 保存链表的头结点
                    head = pRootOfTree;
                    pre = pRootOfTree;
                }else{
                    pre.right = pRootOfTree;
                    pRootOfTree.left = pre;
                    pre = pRootOfTree;
                }
                pRootOfTree = pRootOfTree.right;
            }
        }
        return head;
    }
}

递归版：
public class Solution {
    TreeNode pre = null;
    TreeNode head = null;
    public TreeNode Convert(TreeNode pRootOfTree) {
        if(pRootOfTree ==null)
            return pRootOfTree;
        inOrder(pRootOfTree);
        return head;
    }
    private void inOrder(TreeNode p){
        if(p == null)
            return;
        inOrder(p.left);
        if(head == null){
            head = p;
            pre = p;
        }else{
            pre.right = p;
            p.left = pre;
            pre = p;
        }
        inOrder(p.right);
    }
}

递归优化：
先遍历左子树的话，其链表生成的方向是小—>大，在链表生成完毕之后，记录指向的便是链表中的最后一个节点，
也就是最大的那个节点，需要使用一个变量来记录链表的头结点。
而先遍历右子树的话，其链表生成的方向是大---->小，在链表生成完毕之后，记录指向的便是链表中的最开始一个节点，
也就是最小的那个节点。这样就可以少了一个记录。
public class Solution {
    TreeNode head= null;
    public TreeNode Convert(TreeNode pRootOfTree) {
        if(pRootOfTree == null) return pRootOfTree;       
        Convert(pRootOfTree.right);
        if(head== null){
            head= pRootOfTree;
        } else {
            head.left = pRootOfTree;
            pRootOfTree.right = head;
            head= pRootOfTree;
        }
        Convert(pRootOfTree.left);   
        return head;
    }
}
```
* 二叉树的深度
```java
输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，
最长路径的长度为树的深度。

递归：
public class Solution {
    public int TreeDepth(TreeNode root) {
        if(root == null)
            return 0;
        if(root.left==null && root.right==null)
            return 1;
        int left = TreeDepth(root.left);
        int right = TreeDepth(root.right);
        return left>right? left+1 : right+1;
    }
}

层序遍历
import java.util.*;
public class Solution {
    public int TreeDepth(TreeNode root) {
        if(root == null)
            return 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0; // 深度
        int count = 0; // 当前层已遍历的节点数
        int nextCount = 1; // 当前层节点总数
        boolean flag = true;
        while(!queue.isEmpty()){
            root = queue.poll();
            count++;
            if(root.left!=null)
                queue.offer(root.left);
            if(root.right!=null)
                queue.offer(root.right);
            if(count == nextCount){ // 本层节点已经遍历完毕
                nextCount = queue.size(); //下一层的节点数
                count = 0;
                depth++;
            }
        }
        return depth;
    }
}
```
