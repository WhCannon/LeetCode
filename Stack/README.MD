# 栈和队列

## [练习](#练习)









## 练习
* [练习1](#练习1)
```java
用数组结构实现大小固定的队列和栈
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
