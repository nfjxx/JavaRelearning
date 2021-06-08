# 泛型

## 声明

泛型类:

```java
public class Cname<K, V> {
    private T type;
    private V value;
}
```

泛型方法:

```java
public static<T> returnType Fname(T parm){}
```

## 类型限定与通配符

<T extends X>:确保元素类型为?的子类
<? extends X>:表示X的一个未知子类型 只能读不能写
<? super X>:表示X的一个未知父类型
PECS助记码