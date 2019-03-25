# 栈和队列

## [练习](#练习)









## 练习
* [练习1](#练习1)
```java
用数组结构实现大小固定的队列和栈
```
* [练习2](#练习2)
```java
实现一个特殊的栈， 在实现栈的基本功能的基础上， 再实现返回栈中最小元素的操作。
【要求】
1． pop、 push、 getMin操作的时间复杂度都是O(1)。
2． 设计的栈类型可以使用现成的栈结构。
```
* [练习3](#练习3)
```java
如何仅用队列结构实现栈结构？
如何仅用栈结构实现队列结构？
```



## 练习1
```java
public static class ArrayStack {
			private Integer[] arr;
			private Integer size;

			// 数组实现栈结构
			public ArrayStack(int initSize) {
				if (initSize < 0) {
					throw new IllegalArgumentException(
							"The init size is less than 0");
				}
				arr = new Integer[initSize]; // 创建一个指定长度的数组
				size = 0;
			}

			public Integer peek() {
				if (size == 0) {
					return null;
				}
				return arr[size - 1]; // 返回栈顶
			}

			public void push(int obj) {
				if (size == arr.length) {
					throw new ArrayIndexOutOfBoundsException("The queue is full");
				}
				arr[size++] = obj; // 添加
			}

			public Integer pop() {
				if (size == 0) {
					throw new ArrayIndexOutOfBoundsException("The queue is empty");
				}
				return arr[--size]; // 返回上一个数
			}
		}

		// 数组实现队列
		public static class ArrayQueue {
			private Integer[] arr;
			private Integer size; // 代表队列中的数量
			private Integer start; // 代表当前返回的位置（队列的头）
			private Integer end; // 代表添加的位置（队列的尾）

			public ArrayQueue(int initSize) {
				if (initSize < 0) {
					throw new IllegalArgumentException(
							"The init size is less than 0");
				}
				arr = new Integer[initSize];
				size = 0;
				start = 0;
				end = 0;
			}

			public Integer peek() {
				if (size == 0) {
					return null;
				}
				return arr[start]; // 返回队列头
			}

			public void push(int obj) {// 在队列中添加一个数
				if (size == arr.length) {
					throw new ArrayIndexOutOfBoundsException("The queue is full");
				}
				size++;
				arr[end] = obj; // 添加到队列的尾
				end = end == arr.length - 1 ? 0 : end + 1; // 当end到达数组末尾时，再回到数组的0位置
			}

			public Integer poll() { // 队列中弹出一个数
				if (size == 0) {
					throw new ArrayIndexOutOfBoundsException("The queue is empty");
				}
				size--;
				int tmp = start;
				start = start == arr.length - 1 ? 0 : start + 1; // 当start到达数组末尾时，再回到数组的0位置
				return arr[tmp]; // 返回队列头的数
			}
		}
```
## 练习2
```java
public static class MyStack2 {
		private Stack<Integer> stackData; // 一个栈正常的压数据
		private Stack<Integer> stackMin; // 一个栈用来压最小值

		public MyStack2() {
			this.stackData = new Stack<Integer>();
			this.stackMin = new Stack<Integer>();
		}

		public void push(int newNum) { // 压栈
			if (this.stackMin.isEmpty()) {
				this.stackMin.push(newNum);
			} else if (newNum < this.getmin()) {
				this.stackMin.push(newNum); // 如果添加的数小于stackMin的最小值，就压入
			} else {
				int newMin = this.stackMin.peek();
				this.stackMin.push(newMin); // 否则就还压入原来的最小值
			}
			this.stackData.push(newNum); // 压入正常的数据栈
		}

		public int pop() { // 弹栈
			if (this.stackData.isEmpty()) {
				throw new RuntimeException("Your stack is empty.");
			}
			this.stackMin.pop();
			return this.stackData.pop();
		}

		public int getmin() {
			if (this.stackMin.isEmpty()) {
				throw new RuntimeException("Your stack is empty.");
			}
			return this.stackMin.peek();
		}
	}
```
## 练习3
```java
// 用2个队列实现栈结构
		public static class TwoQueuesStack {
			private Queue<Integer> data;
			private Queue<Integer> help;

			public TwoQueuesStack() {
				data = new LinkedList<Integer>();
				help = new LinkedList<Integer>();
			}

			public void push(int pushInt) {
				data.add(pushInt); // 压栈
			}

			public int peek() {
				if (data.isEmpty()) {
					throw new RuntimeException("Stack is empty!");
				}
				while (data.size() != 1) {
					help.add(data.poll());
				}
				int res = data.poll();
				help.add(res);
				swap();
				return res;
			}

			public int pop() { // 弹栈
				if (data.isEmpty()) {
					throw new RuntimeException("Stack is empty!");
				}
				while (data.size() > 1) { // 将data队列中的数据加入help队列中，最后data中会只剩最后一个添加的数
					help.add(data.poll());
				}
				int res = data.poll(); // 返回最后的这个数，就相当于后进先出
				swap(); // 把2个队列交换
				return res;
			}

			private void swap() {
				Queue<Integer> tmp = help;
				help = data;
				data = tmp;
			}

		}


```
