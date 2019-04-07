# 剑指Offer
## [动态规划](#动态规划)




## 动态规划
* 跳台阶
```java
一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）

递归版本
public class Solution {
    public int JumpFloor(int target) {
        if(target==1)
            return 1;
        if(target==2)
            return 3;
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
