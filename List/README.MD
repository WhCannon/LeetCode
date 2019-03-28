# 表
## 顺序表
## 链表
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
	        */
	    }
```
