# 二叉树
## [平衡二叉搜索树AVL](#平衡二叉搜索树)
## [二叉树的遍历](#二叉树的遍历)
## [练习](#练习)


## 平衡二叉搜索树
有AVL树、SBT树、红黑树等
* 红黑树
```java
TreeMap、TreeSet以及JDK1.8之后的HashMap底层都用到了红黑树。
```









## 二叉树的遍历
* 先序遍历
```
递归版
public class TraverseTree {
	public static class Node {
		public int value;
		public Node left;
		public Node right;
		public Node(int data) {
			this.value = data;
		}
	}
	public static void preOrderRecur(Node head) {//先序遍历
		if (head == null) {
			return;
		}
		System.out.print(head.value + " ");//先打印当前节点
		preOrderRecur(head.left);//再打印左子树
		preOrderRecur(head.right);//然后打印右子树
	}
}

非递归
public static void preOrderUnRecur(Node head) { //先序遍历非递归版
		if (head != null) {
			Stack<Node> stack = new Stack<Node>();
			stack.add(head); //添加当前节点
			while (!stack.isEmpty()) {
				//当前节点出栈后，它的右孩子先进栈，左孩子后进栈，这样出栈时左孩子就会先出栈，就会先打印左孩子
				head = stack.pop(); 
				System.out.print(head.value + " "); //中-左-右
				if (head.right != null) {
					stack.push(head.right);//右孩子先进栈
				}
				if (head.left != null) {
					stack.push(head.left);//左孩子后进栈
				}
			}
		}
	}
```
* 中序遍历
```java
递归版
public static void inOrderRecur(Node head) {//中序遍历
		if (head == null) {
			return;
		}
		inOrderRecur(head.left); //先打印左子树
		System.out.print(head.value + " ");//再打印当前节点
		inOrderRecur(head.right);//最后打印右子树
	}

非递归
public static void inOrderUnRecur(Node head) { //中序遍历非递归版
		System.out.print("in-order: ");
		if (head != null) {
			Stack<Node> stack = new Stack<Node>();
			while (!stack.isEmpty() || head != null) {
				//当前节点不为null，进栈，移到左孩子。当前节点为null，从栈中取一个，移到右孩子
				if (head != null) {
					stack.push(head); //当前节点进栈
					head = head.left; //移到左孩子
				} else {
					head = stack.pop();//1、先打印左孩子  3、如果右孩子为null，就回到了父节点
					System.out.print(head.value + " "); //左-中-右
					head = head.right;//2、移到右孩子
				}
			}
		}
		System.out.println();
	}
```
* 后序遍历
```java
递归版
public static void posOrderRecur(Node head) {//后序遍历
		if (head == null) {
			return;
		}
		posOrderRecur(head.left);//先打印左子树
		posOrderRecur(head.right);//再打印右子树
		System.out.print(head.value + " ");//最后打当前节点
	}
  
非递归
先设计成左、右、中进栈，然后出栈的过程中，压入辅助栈中，再出栈就是左-右-中的顺序
public static void posOrderUnRecur1(Node head) { //后序遍历非递归
		System.out.print("pos-order: ");
		if (head != null) {
			Stack<Node> s1 = new Stack<Node>();
			Stack<Node> s2 = new Stack<Node>();
			s1.push(head);
			while (!s1.isEmpty()) {
				head = s1.pop();
				s2.push(head);
				if (head.left != null) {
					s1.push(head.left);
				}
				if (head.right != null) {
					s1.push(head.right);
				}
			}
			while (!s2.isEmpty()) {//打印辅助栈
				System.out.print(s2.pop().value + " ");//左-右-中
			}
		}
		System.out.println();
	}
```
* 层序遍历
```java

```
* Morris遍历
```java

```







## 练习
* [练习1](#练习1)   





