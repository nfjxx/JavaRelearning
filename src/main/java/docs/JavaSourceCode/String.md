# Java String

## 属性

```java
public final class String implements Serializable, Comparable<String>, CharSequence {
    private final byte[] value;//Java 9 由char[]变为byte[]
    private final byte coder;//0:Latin-1 单字节编码 1:UTF-8
    private int hash;
    private static final long serialVersionUID = -6849794470754667710L;
    static final boolean COMPACT_STRINGS = true;//是否紧密压缩
    private static final ObjectStreamField[] serialPersistentFields = new ObjectStreamField[0];//指定默认序列化
    public static final Comparator<String> CASE_INSENSITIVE_ORDER = new String.CaseInsensitiveComparator();//默认比较器
    static final byte LATIN1 = 0;
    static final byte UTF16 = 1;
    ...
}
```

## 构造方法

```java
    public String() {
        this.value = "".value;
        this.coder = "".coder;
    }
    @HotSpotIntrinsicCandidate
    public String(String original) {
        this.value = original.value;
        this.coder = original.coder;
        this.hash = original.hash;
    }
```

javap -v -c Test.class:

String a = "abc":

```text
Constant pool:
   #1 = Methodref          #4.#13         // java/lang/Object."<init>":()V
   #2 = String             #14            // abc
   #3 = Class              #15            // Test
   #4 = Class              #16            // java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Utf8               Code
   #8 = Utf8               LineNumberTable
   #9 = Utf8               main
  #10 = Utf8               ([Ljava/lang/String;)V
  #11 = Utf8               SourceFile
  #12 = Utf8               Test.java
  #13 = NameAndType        #5:#6          // "<init>":()V
  #14 = Utf8               abc
  #15 = Utf8               Test
  #16 = Utf8               java/lang/Object

...
0: ldc           #2           // String abc
2: astore_1
3: return
...
ldc:将对应的常量的引用压入操作数栈
astore_1:将操作数栈底元素弹出，存储到局部变量表中的 1 号元素
return:方法返回值为 void，标志方法执行完成，将方法对应栈帧从栈中弹出
```



String a=new String("abc"):

```text
Constant pool:
   #1 = Methodref          #6.#15         // java/lang/Object."<init>":()V
   #2 = Class              #16            // java/lang/String
   #3 = String             #17            // abc
   #4 = Methodref          #2.#18         // java/lang/String."<init>":(Ljava/lang/String;)V
   #5 = Class              #19            // Test
   #6 = Class              #20            // java/lang/Object
   #7 = Utf8               <init>
   #8 = Utf8               ()V
   #9 = Utf8               Code
  #10 = Utf8               LineNumberTable
  #11 = Utf8               main
  #12 = Utf8               ([Ljava/lang/String;)V
  #13 = Utf8               SourceFile
  #14 = Utf8               Test.java
  #15 = NameAndType        #7:#8          // "<init>":()V
  #16 = Utf8               java/lang/String
  #17 = Utf8               abc
  #18 = NameAndType        #7:#21         // "<init>":(Ljava/lang/String;)V
  #19 = Utf8               Test
  #20 = Utf8               java/lang/Object
  #21 = Utf8               (Ljava/lang/String;)V

...
0: new           #2                  // class java/lang/String
3: dup
4: ldc           #3                  // String abc
6: invokespecial #4                  // Method java/lang/String."<init>":(Ljava/lang/String;)V
9: astore_1
10: return
...
new:在java堆上为#2对象分配内存空间，并将地址压入操作数栈顶
dup:复制操作数栈顶值，并将其压入栈顶
ldc:将对应的常量的引用压入操作数栈
invokespecial：调用构造方法
astore_1:将操作数栈底元素弹出，存储到局部变量表中的 1 号元素
return:方法返回值为 void，标志方法执行完成，将方法对应栈帧从栈中弹出
```