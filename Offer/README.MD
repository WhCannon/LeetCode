# 剑指Offer
## [字符串](#字符串)
## [动态规划](#动态规划)







## 字符串
* 替换空格  
```java
请实现一个函数，将一个字符串中的每个空格替换成“%20”。
例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy

public String replaceSpace(StringBuffer str) {
       return  str.toString().replaceAll(" ", "%20");
}

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
