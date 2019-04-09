# 剑指Offer

 ## [Array](#array)
 ## [String](#string)
 ## [LinkedList](#linkedlist)
 ## [Stack_Queue](#Stack_Queue)
 ## [DP](#dp)









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



## Stack_Queue

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
