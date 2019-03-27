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
