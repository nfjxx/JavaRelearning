# Array

## 声明

```text
基本格式：
T[] a ={a,b,c};
T a = new T[len];
T a = new T[]{a,b,c}

多维：
T[][] a = {{1,2},{3,4}}

例子：
int[] a = {1,2,3};
String[] a = new String[10];
int[] b = new int[]{1, 2, 3};
int[][] = {{1,2},{3,4}}
String[][] a = new String[][]{
        new String[]{"1", "2", "3", "4", "5"},
        new String[]{"1", "2", "3", "4", "5"},
        new String[]{"1", "2", "3", "4", "5"},
        new String[]{"1", "2", "3", "4", "5"},
        new String[]{"1", "2", "3", "4", "5"},
};
```

## Array常用方法

### 遍历

```text
基本for循环：for (int i = 0; i <array.length ; i++) {}
增强for循环：for (T E : array) {}

区别：
增强for循环不能对数据进行操作，主要用于遍历
必须从头到尾都遍历，且不能获取下标
```

### 显示

```text
@return String
Arrays.toString(T[]):显示T[]中的各项数据 
直接print的是数组类型和地址

@return String
Arrays.deepToString(arrays):显示多维数组的各项数据
```

### 填充

```text
@return void @null(start,end)
Arrays.fill(T[],start,end,valve):用valve填充一维数组

@return void
Arrays.setAll(T[],F(i))
lambda自定义填充 i为索引 return 要填充的值 
```

### 转换

```text
@return ArrayList
Arrays.asList(T[]):将数组转换为ArrayList
Arrays.asList() 返回的是 java.util.Arrays.ArrayList，并不是java.util.ArrayList，它的长度是固定的，无法进行元素的删除或者添加。

常见处理：
List<T> list = Arrays.asList(T[]);                             (read only)
ArrayList list = new ArrayList(Arrays.asList(T[]));            (simple)
List<T> arrayList = new ArrayList<>(T[].length);
Collections.addAll(arrayList, T[]);                            (best)
```

### 复制

```text
@return T[] 
Arrays.copyOf(T[],len):从头复制指定len长度的数组
len超过T[].length时，用null填充

@return T[] 
Arrays.copyOfRange(T[],from,to):复制从from到to的T[]部分片段
to超过T[].length时，用null填充

都是进行对System.arraycopy的封装
```

### 比较

```text
@return boolean
Arrays.equals(T[],T[]):比较两个数组是否相同
Arrays.equals(T[],Aform,Ato,T[],Bform,Bto):比较两个数组片段是否相同

@return boolean
Arrays.deepEquals(T[],T[]):比较两个多维数组是否相同
```

### 排序

```text
Arrays.sort(T[]):数组排序(默认升序)  基本类型 DP快排 | Object[] a ComparableTimSort |T[] TimSort
Arrays.sort(T[],from,to):[from to) 范围内排序
Arrays.legacyMergeSort(T[]):传统合并排序

Arrays.parallelSort():并行排序 
数组len> 1<<13才进行
```

### 查找

```text
@return int 
Arrays.binarySearch(T[],e):查询已排序数组中e的索引
Arrays.binarySearch(T[],from,to,e):查询已排序数组[from,to)中e的索引
```

### 流

```text
@return Stream<T>
Arrays.stream(T[]):将数组转换为流
Arrays.stream(T[],from,to):将数组[from,to]转换为流
```

### 并行前缀

```text
Arrays.parallelPrefix(T[],F(i,j))
i为前一元素值，j为当前元素值，返回值在j上，0位置上i为0
```

## 补充

### 泛型数组

```text
Java 不直接支持泛型数组（可用其他方式实现），因此Arrays中方法大多选择重载，而容器类中采用泛型复用很多代码。
为什么不支持泛型数组？
泛型的本身特性（类型擦除）？Java本身向后兼容的特点？容器类的出现？开发成本？
```