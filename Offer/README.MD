# 剑指Offer
  * [位运算](#位运算)
  * [Array](#array)
  * [String](#string)
  * [LinkedList](#linkedlist)
  * [StackQueue](#stackqueue)
  * [DP](#dp)





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
