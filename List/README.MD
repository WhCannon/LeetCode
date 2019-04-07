# 表
## 顺序表
## 链表
  * [双向链表](#双向链表)
    * [窗口结构](#窗口结构)
  * [约瑟夫环](#约瑟夫环)
## [练习](#练习)



## 练习
* [练习1](#练习1)
```java
	 反转单向和双向链表
	【题目】 分别实现反转单向链表和反转双向链表的函数。
	【要求】 如果链表长度为N， 时间复杂度要求为O(N)， 额外空间复杂度要求为O(1)
```
* [练习2](#练习2)
```java
	输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。
```


## 双向链表
### 窗口结构
```java
窗口:此结构一端只用来放数据，另一端用来获取目前为止出现过的最大值（最小值）。可以用LinkedList来实现。
窗口的最大值更新结构：当向此结构放数据时会检查一下结构中的已有数据，从时间戳最大的开始检查，
如果检查过程中发现该数据小于即将放入的数据则将其弹出并检查下一个，直到即将放入的数据小于正在检查的数据或者结构中的数据都被弹出了为止，
再将要放入的数据放入结构中并盖上时间戳。如此每次从该结构取数据时，都会返回结构中时间戳最小的数据，也是目前为止进入过此结构的所有数据中最大的那一个。
此结构可以使用一个双端队列来实现，一端只用来放数据（放数据之前的检查过程可能会弹出其他数据），另一端用来获取目前为止出现过的最大值。
public class Window {	
	  private LinkedList<Integer> queue;
	  public Window() {
	    this.queue = new LinkedList<Integer>();
	  } 
	  //更新窗口最大值
	  public void add(int i){
	    while (!queue.isEmpty() &&  queue.getLast()<= i) {
	      queue.pollLast();
	    }
	    queue.add(i);
	  }
	 
	  //获取窗口最大值
	  public int getMax() {
	    if (!queue.isEmpty()) {
	      return queue.peek();
	    }
	    return Integer.MIN_VALUE;
	  }
	 
	  //使窗口最大值过期
	  public void expireMaxValue() {
	    if (!queue.isEmpty()) {
	      queue.poll();
	    }
	  }
	 
	  public static void main(String[] args) {
	    Window window = new Window();
	    window.add(6);
	    window.add(4);
	    window.add(9);
	    window.add(8);
	    System.out.println(window.getMax());//9
	    window.expireMaxValue();
	    System.out.println(window.getMax());//8
	  }

}
```


## 练习1
```java
public static class Node {
		public int value;
		public Node next;

		public Node(int data) {
			this.value = data;
		}
	}

	public static Node reverseList(Node head) {
		Node pre = null;//记录head的前一个
		Node temp = null; //记录head的后一个
		while (head != null) {
			temp=head.next;//先把head的下一个记录起来
			head.next=pre; //让head指向前一个，也就是反转
			pre=head;//前2个反转完成，让pre向后移动一位，来到head的位置
			head=temp;//head也向后移动一位，来到temp的位置
		}
		return pre;
	}

	public static class DoubleNode {
		public int value;
		public DoubleNode last;
		public DoubleNode next;

		public DoubleNode(int data) {
			this.value = data;
		}
	}

	public static DoubleNode reverseList(DoubleNode head) {
		DoubleNode pre = null;
		DoubleNode next = null;
		while (head != null) {
			next = head.next;
			head.next = pre;
			head.last = next;
			pre = head;
			head = next;
		}
		return pre;
	}
```
## 练习2
```java
//两个链表一一对比，小的数就添加到合成链表，然后向后移一位，再进行对比。
	
	public static class ListNode {
	    int val;
	    ListNode next = null;
	    ListNode(int val) {
	        this.val = val;
	    }
	}
	
	    public static ListNode Merge(ListNode list1,ListNode list2) {
	        if(list1==null) 
	            return list2;
	        if(list2==null)
	            return list1;
	        //ListNode res=null
	        //递归
	        if(list1.val<=list2.val){
	            //res=list1;
	            //res.next=Merge(list1.next,list2)
	             list1.next=Merge(list1.next,list2);
	            return list1;
	        }else{
	        	//res=list2;
	        	//res.next=Merge(list1,list2.next)
	            list2.next=Merge(list1,list2.next);
	            return list2;
	        }
	        //return res;
	        
	        /* 非递归
	        ListNode head=new ListNode(-1);
	        ListNode current=
