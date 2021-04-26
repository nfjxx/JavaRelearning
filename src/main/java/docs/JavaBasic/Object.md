## Java Object

### equal ==

```java
public boolean equals(Object obj){
        return this==obj;
        }
```

equals:

作用不能用于判断基本数据类型的变量，只能用来判断两个对象是否相等 默认情况等价于通过“==”比较这两个对象 

一般会覆盖 equals()方法来比较两个对象中的属性是否相等 

所有整型包装类对象之间值的比较，全部使用 equals 方法比较(避免常量池干扰)

==:

对于基本数据类型来说，==比较的是值 对于引用数据类型来说，==比较的是对象的内存地址