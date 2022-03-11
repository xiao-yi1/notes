# 第六章：接口  
## 零碎的知识点  
1. 类只能继承一个父类，但是可以实现多个接口，所以接口不能由抽象类替代
   多重继承会让语言变得很复杂，效率也会降低
2. 在Java SE 8中，允许在接口中增加静态方法。理论上讲，没有任何理由认为这是不合法的。只是这有违于将接口作为抽象规范的初衷，解决方法就是提供一个伴随类，例如Paths和Path，Paths里面有静态的get方法。java8中可以在接口中直接实现方法，伴随类已经过时。

# 第十四章：并发
1. ### 阻塞和等待的区别？
<https://blog.csdn.net/woshiyigeliangliang/article/details/81116872>
2. ### 为何数字不一样？
我们都知道Cpu运算速度极快，但是每次读取数据都要直接访问内存，会严重拖慢Cpu的速率，所以内存就有了一层高速缓存，在JAVA中，每次线程读取到一个数据，会将这份数据copy到高速缓存中，然后写操作在高速缓存中进行，然后在"空余"时间，将缓存结果刷新到内存中。 回到问题，博主得到的结果大概率少于10000，就可以这样解释，比如A线程读取到Num的值为100 然后进行+1操作，写入缓存中，但是还没来得及同步到主存中，而B线程同理读取到100，进行+1操作，更新缓存，刷新到内存中。这样内存中的num为101，但是实际上做了2次 +1操作，所以结果会少于10000。 而你执行了打印操作，那么cpu获得一定的空余时间，线程每次都有空余时间，将缓存的值刷新到内存，所以你可以每次得到10000. JAVA的 volatile 关键字就是用于线程可见性的，强制线程读取内存中的值。这样就可以避免线程不安全了。
```
public class Count {
    private int num;
    public void increment() {
        num++;
    }
    public int get() {
        return num;
    }
}
public class ThreadTest {
    public static void main(String[] args) {
        Count count = new Count();
        Runnable runnable = new Runnable() {
            public void run() {
                for (int i = 0; i < 10000; i++) {
                    count.increment();
                }
            }
        };
        List<Thread> threads = new ArrayList<>(10);
        for (int i = 0; i < 10; i++) {
            Thread thread = new Thread(runnable);
            threads.add(thread);
            thread.start();
        }
        while (true) {
            if (allThreadTerminated(threads)) {// 所有线程运行结束
                System.out.println(count.get());
                break;
            }
        }
    }
    private static boolean allThreadTerminated(List<Thread> threads) {
        for (Thread thread : threads) {
            if (thread.isAlive()) {
                return false;
            }
        }
        return true;
    }
}
```
这里启动了10个线程，每个线程累加1万次，我们期望的最终结果是10万，看一下输出结果：95388

3. 什么是CAS？https://www.jianshu.com/p/ae25eb3cfb5d
4. 线程池
<https://tech.meituan.com/2020/04/02/java-pooling-pratice-in-meituan.html>
5. java线程阻塞唤醒的四种方式
<https://blog.csdn.net/u014044812/article/details/79474575>
6. java并发系列-monitor机制实现
<https://www.cnblogs.com/qingshan-tang/p/12698705.html>
7. 创建线程方法
8. java 线程中断和 InterruptedException 异常
<https://blog.csdn.net/qq_37012496/article/details/106217013>
9. Java 并发：线程间通信与协作
<https://blog.csdn.net/justloveyou_/article/details/54929949>
重点:{
10. LockSupport与AQS
<https://www.jianshu.com/p/7fecabeeea6b>
11. 面试 LockSupport.park()会释放锁资源吗？
<https://www.cnblogs.com/tong-yuan/p/11768904.html>
12. 简单看看LockSupport和AQS
<https://www.cnblogs.com/wyq1995/p/12244538.html>
13. 面试官问我什么是JMM
<https://zhuanlan.zhihu.com/p/258393139>
14. Java内存模型（JMM）总结
<https://zhuanlan.zhihu.com/p/29881777>
<https://www.cnblogs.com/hello-shf/p/12100799.html>






}
1.  多线程编程：{
Java多线程——交替打印ABC
<https://blog.csdn.net/weixin_41070695/article/details/112596316>



}
15. 线程池是怎么创建的，参数的类型你能够说说吗？
16. 彻底搞懂Java的等待-通知(wait-notify)机制
<https://cloud.tencent.com/developer/article/1488110>
17. 快速了解基于AQS实现的Java并发工具类
<https://mp.weixin.qq.com/s?__biz=MzUyNzgyNzAwNg==&mid=2247483885&idx=1&sn=8fe2bf133cbc7932def11e407e76a783&scene=21#wechat_redirect>
18. 不可不说的Java“锁”事
<https://tech.meituan.com/2018/11/15/java-lock.html>
19. Java线程池七个参数详解：核心线程数、最大线程数、空闲线程存活时间、时间单位、工作队列、线程工厂、拒绝策略












# Java集合
1. Java 8系列之重新认识HashMap
<https://zhuanlan.zhihu.com/p/21673805>
2. JAVA HASHMAP的死循环
<https://coolshell.cn/articles/9606.html>
3. HashMap中hash(Object key)原理，为什么(hashcode ＞＞＞ 16)
<https://blog.csdn.net/qq_42034205/article/details/90384772?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ELandingCtr%7EHighlightScore-3.queryctrv2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ELandingCtr%7EHighlightScore-3.queryctrv2&utm_relevant_index=6>
4. Java遍历集合的几种方法分析（实现原理、算法性能、适用场合）
<https://cloud.tencent.com/developer/article/1179580>
<https://cloud.tencent.com/developer/news/700876>
5. ArrayList的扩容机制
<https://www.cnblogs.com/zeroingToOne/p/9522814.html#:~:text=ArrayList%E7%9A%84%E6%89%A9%E5%AE%B9%E6%9C%BA%E5%88%B6%20ArrayList%E7%9A%84%E6%89%A9%E5%AE%B9%E6%9C%BA%E5%88%B6%EF%BC%9A,%E5%BD%93%E5%90%91ArrayList%E4%B8%AD%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0%E7%9A%84%E6%97%B6%E5%80%99%EF%BC%8CArrayList%E7%9A%84%E5%AD%98%E5%82%A8%E5%AE%B9%E9%87%8F%E5%A6%82%E6%9E%9C%E6%BB%A1%E8%B6%B3%E6%96%B0%E5%85%83%E7%B4%A0%E7%9A%84%E5%AE%B9%E9%87%8F%E8%A6%81%E6%B1%82%EF%BC%8C%E5%88%99%E7%9B%B4%E6%8E%A5%E5%AD%98%E5%82%A8%EF%BC%9BArrayList%E7%9A%84%E5%AD%98%E5%82%A8%E5%AE%B9%E9%87%8F%E5%A6%82%E6%9E%9C%E4%B8%8D%E6%BB%A1%E8%B6%B3%E6%96%B0%E5%85%83%E7%B4%A0%E7%9A%84%E5%AE%B9%E9%87%8F%E8%A6%81%E6%B1%82%EF%BC%8CArrayList%E4%BC%9A%E5%A2%9E%E5%BC%BA%E8%87%AA%E8%BA%AB%E7%9A%84%E5%AD%98%E5%82%A8%E8%83%BD%E5%8A%9B%EF%BC%8C%E4%BB%A5%E8%BE%BE%E5%88%B0%E5%AD%98%E5%82%A8%E6%96%B0%E5%85%83%E7%B4%A0%E7%9A%84%E8%A6%81%E6%B1%82%E3%80%82%20%E5%9B%A0%E4%B8%BA%E4%B8%8D%E5%90%8C%E7%9A%84JDK%E7%89%88%E6%9C%AC%E7%9A%84%E6%89%A9%E5%AE%B9%E6%9C%BA%E5%88%B6%E5%8F%AF%E8%83%BD%E6%9C%89%E5%B7%AE%E5%BC%82%EF%BC%8C%E4%B8%8B%E9%9D%A2%E4%BB%A5JDK1.8%E4%B8%BA%E4%BE%8B%E8%AF%B4%E6%98%8E>
6. 一文彻底搞懂ConcurrentHashMap原理
<https://www.itqiankun.com/article/concurrenthashmap-principle>

7. Comparable接口和Comparator接口的使用和区别
<https://blog.csdn.net/IT_10/article/details/104747173>
<https://www.cnblogs.com/ssskkk/p/8784542.html>
8. ArrayList中elementData为什么被transient修饰？
<https://blog.csdn.net/zero__007/article/details/52166306>
9. 了解Java集合中的快速失败机制（fail-fast）
<https://blog.csdn.net/weixin_44170221/article/details/105028266>
10. Java中HashMap和TreeMap的区别深入理解
<https://www.cnblogs.com/williamjie/p/9099130.html#:~:text=HashMap%E9%80%9A%E8%BF%87hashcode%E5%AF%B9%E5%85%B6%E5%86%85%E5%AE%B9%E8%BF%9B%E8%A1%8C%E5%BF%AB%E9%80%9F%E6%9F%A5%E6%89%BE%EF%BC%8C%E8%80%8C%20TreeMap%E4%B8%AD%E6%89%80%E6%9C%89%E7%9A%84%E5%85%83%E7%B4%A0%E9%83%BD%E4%BF%9D%E6%8C%81%E7%9D%80%E6%9F%90%E7%A7%8D%E5%9B%BA%E5%AE%9A%E7%9A%84%E9%A1%BA%E5%BA%8F%EF%BC%8C%E5%A6%82%E6%9E%9C%E4%BD%A0%E9%9C%80%E8%A6%81%E5%BE%97%E5%88%B0%E4%B8%80%E4%B8%AA%E6%9C%89%E5%BA%8F%E7%9A%84%E7%BB%93%E6%9E%9C%E4%BD%A0%E5%B0%B1%E5%BA%94%E8%AF%A5%E4%BD%BF%E7%94%A8TreeMap%EF%BC%88HashMap%E4%B8%AD%E5%85%83%E7%B4%A0%E7%9A%84%E6%8E%92%E5%88%97%E9%A1%BA%E5%BA%8F%E6%98%AF%E4%B8%8D%E5%9B%BA%E5%AE%9A%E7%9A%84%EF%BC%89%E3%80%82.%20HashMap%20%E9%9D%9E%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%20TreeMap%20%E9%9D%9E%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8.,%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8.%20%E5%9C%A8Java%E9%87%8C%EF%BC%8C%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E4%B8%80%E8%88%AC%E4%BD%93%E7%8E%B0%E5%9C%A8%E4%B8%A4%E4%B8%AA%E6%96%B9%E9%9D%A2%EF%BC%9A.%201%E3%80%81%E5%A4%9A%E4%B8%AAthread%E5%AF%B9%E5%90%8C%E4%B8%80%E4%B8%AAjava%E5%AE%9E%E4%BE%8B%E7%9A%84%E8%AE%BF%E9%97%AE%EF%BC%88read%E5%92%8Cmodify%EF%BC%89%E4%B8%8D%E4%BC%9A%E7%9B%B8%E4%BA%92%E5%B9%B2%E6%89%B0%EF%BC%8C%E5%AE%83%E4%B8%BB%E8%A6%81%E4%BD%93%E7%8E%B0%E5%9C%A8%E5%85%B3%E9%94%AE%E5%AD%97synchronized%E3%80%82.%20%E5%A6%82ArrayList%E5%92%8CVector%EF%BC%8CHashMap%E5%92%8CHashtable.%20%EF%BC%88%E5%90%8E%E8%80%85%E6%AF%8F%E4%B8%AA%E6%96%B9%E6%B3%95%E5%89%8D%E9%83%BD%E6%9C%89synchronized%E5%85%B3%E9%94%AE%E5%AD%97%EF%BC%89%E3%80%82.%20%E5%A6%82%E6%9E%9C%E4%BD%A0%E5%9C%A8interator%E4%B8%80%E4%B8%AAList%E5%AF%B9%E8%B1%A1%E6%97%B6%EF%BC%8C%E5%85%B6%E5%AE%83%E7%BA%BF%E7%A8%8Bremove%E4%B8%80%E4%B8%AAelement%EF%BC%8C%E9%97%AE%E9%A2%98%E5%B0%B1%E5%87%BA%E7%8E%B0%E4%BA%86%E3%80%82.%20>

11. java中的fail-fast(快速失败)机制
<https://blog.csdn.net/zymx14/article/details/78394464>
12. 真正搞懂hashCode和hash算法
<https://developer.aliyun.com/article/817686>
13. HashMap 链表插入方式 → 头插为何改成尾插
<https://www.cnblogs.com/youzhibing/p/13915116.html>
<https://blog.csdn.net/weixin_42373997/article/details/112085344>
14. hashmap头插法和尾插法区别_一个跟面试官扯皮半个小时的HashMap
<https://blog.csdn.net/weixin_42373997/article/details/112085344>
15. 并发容器之ConcurrentHashMap
<https://blog.csdn.net/a979331856/article/details/105922069>
<https://cloud.tencent.com/developer/article/1448721>



# JVM
1. 从实际案例聊聊Java应用的GC优化
<https://tech.meituan.com/2017/12/29/jvm-optimize.html>
2. 图解 | 原来这就是 class
<https://www.toutiao.com/a6943184223163531814/?log_from=a4bf0757559d4_1646220258627>
3. jvm之safepoint、safeRegion和OopMap
<https://blog.csdn.net/Saintyyu/article/details/107896097>
4. Java双亲委派模型是什么、优势在哪、双亲委派模型的破坏
<https://blog.nowcoder.net/n/1c94ae6f6a1b4f18b7446ef38a540acf?from=nowcoder_improve#:~:text=%E5%8F%8C%E4%BA%B2%E5%A7%94%E6%B4%BE%E6%A8%A1%E5%9E%8B%E6%98%AFJ,%E5%85%8D%E7%B1%BB%E7%9A%84%E9%87%8D%E5%A4%8D%E5%8A%A0%E8%BD%BD>.
<https://blog.csdn.net/weixin_36586120/article/details/117457014>
1. 面试题：能否自定义一个java.lang.Object类
<https://blog.csdn.net/u011212394/article/details/104113847>


