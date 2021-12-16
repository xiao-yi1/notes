# java
## 第一天
### 下面哪个Set类是按元素排好序的？

LinkedHashSet  
继承于HashSet、又基于 LinkedHashMap 来实现 

TreeSet  
使用二叉树的原理对新 add()的对象按照指定的顺序排序（升序、降序），每增加一个对象都会进行排序，将对象插入的二叉树指定的位置。

HashSet  
存储元素的顺序并不是按照存入时的顺序（和 List 显然不同） 而是按照哈希值来存的所以取数据也是按照哈希值取得

TreeSet自然排序，LinkedHashSet按添加顺序排序

### 包

Collection  
    -----List  
               -----LinkedList    非同步  
                ----ArrayList      非同步，实现了可变大小的元素数组  
                ----Vector          同步  
                         ------Stack
    -----Set   不允许有相同的元素


Map  
    -----HashTable        同步，实现一个key--value映射的哈希表  
    -----HashMap          非同步，  
    -----WeakHashMap   改进的HashMap，实现了“弱引用”，如果一个key不被引用，则被GC回收  

### 代码题
``` java
public class Test {
    public int aMethod(){
        static int i = 0;
        i++;
        return i;
    }
public static void main(String args[]){
    Test test = new Test();
    test.aMethod();
    int j = test.aMethod();
    System.out.println(j);
    }
}
```
报错:静态变量只能在类主体中定义，不能在方法中定义

### 关于文件

