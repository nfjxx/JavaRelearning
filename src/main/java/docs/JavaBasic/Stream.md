# 流

## 特点

1. 流不储存元素，元素存储在底层的集合
2. 流不改变元数据，在原来的流基础上，新生成一个流
3. 延迟执行，直到需要时才执行

## 典型工作流程

1. 创建一个流
2. 执行将初始流转化为其他流的中间操作
3. 应用终止操作产生结果

## 创建流

1. 有限流
    1. Arrays.stream(T[],from,to)
    2. Stream.of(T... valves)
    3. Collections.stream()
2. 无限流
    1. Stream.generate(()->?)
    2. Stream.iterate(T,x->?)
    
## 中间操作

filter(P var):生成匹配一定条件的新流 

map(F var1):将流中的元素转为另一种形式

flatMap(F var1):将流中的元素转为另一种形式且合并

limit(n):生成指定n个元素的流，用于裁剪

skip(n):略过前n个元素

takeWhile(P var1):收集元素直到predicate不满足

dropWhile(P var1):删除元素直到predicate不满足

distinct():去重

sorted():排序，可重写排序方法

peek():创建一个与原先流相同的流，一般用于调试



## 规约

count():计数

max()/min():返回最大小值(Optional包装)

findFirst():查询第一个值

findAny():查询任意一个匹配值 常用于并行

anyMatch(P var1):查看流是否有匹配元素

reduce(var1,var2):从前两个元素开始进行某种规约操作

## 基本类型流

8种基本类型强化流

.boxed() 装箱

## 收集结果

1. foreach(C var1):遍历每个元素

2. toArray(IF var):转换为数组

   例:

   xx.toArray(T::new)生成对应类型数组

3. collect(C var):将流收集到目标容器 

   例:

   xx.collect(Collectors.toXX())收集到list or set
   
   xx.collect(Collectors.toCollection(xx::new))收集到容器实现类

   xx.collect(Collectors.joining("分隔符"))收集到String中

   xx.collect(Collectors.summarizingInt(String::length))计算流各项数字指标

4. 分组与分片

   xx.collect(Collectors.groupingBy(T/P))根据分类依据或P分组

   xx.collect(Collectors.groupingBy(T/P,C))分组下游处理
   
   C:counting() summing() max/minBy() filter()

## 补充

.parallelStream()使用采用并行方式，基本等同.stream()
predicate:x->boolean
