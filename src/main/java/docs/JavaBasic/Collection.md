# Collection

## Collection 与 Arrays

Collection->Arrays:
Arrays.<?>asList() 方法接受一个数组或是逗号分隔的元素列表（使用可变参数），并将其转换为 List 对象。

Arrays->Collection:
Collections.addAll() 方法接受一个 Collection 对象，以及一个数组或是一个逗号分隔的列表，将其中元素添加到 Collection 中。

## ArrayList

ArrayList 保存的是 Object ，所以不仅可以通过 ArrayList 的 add() 方法将A对象放入这个集合，而且可以放入B对象，这无论在编译期还是运行时都不会有问题。 当使用 ArrayList 的 get()
方法来取出你认为是 A 的对象时，得到的只是 Object 引用，必须将其转型为 A 调用方法：((A)object).method

可显式声明继承自哪个类，如List<String> list = new ArrayList<>();

当指定了某个类型为泛型参数时，并不仅限于只能将确切类型的对象放入集合中,向上转型也可以像作用于其他类型一样作用于泛型


## Stack(不建议使用)

Java 中的 Stack 类，最大的问题是继承了 Vector 这个类。Vector 作为动态数组，是有能力在数组中的任何位置添加或者删除元素的。Stack 继承了 Vector，Stack 也有这样的能力！
```java
    Stack<Integer> stack =new Stack<>();
    stack.push(1);
    stack.push(2);
    stack.add(1,777);
    System.out.println(stack);//output:[1, 777, 2]
    //新版本建议
    Deque<Integer> stack =new ArrayDeque<>();
```

