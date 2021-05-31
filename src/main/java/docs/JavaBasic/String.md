# String

## 声明

```text
String x = "x";
String x = new String("x");

String 对象是不可变的。String 类中每一个看起来会修改 String 值的方法，实际上都是创建了一个全新的 String 对象。最初的 String 对象不会改变。
详见源码解析-String
```

## 常用方法

### 长度

```text
@return int 
s.length():获取字符串长度
不同于数组的 .length 属性
```

## 索引

```text
@return char
s.charAt(int):获取指定位置的字符

@return int
s.indexOf(e,@not start):获取元素出现的索引 e(str|int)  未找到为-1
s.lastIndexOf(e,@not start):从后往前获取元素出现的索引 

@return boolean
s.contains(s1):判断s中是否存在s1
s.isempty():判断是否s为空
```

## 大小写

```text
@return String
s.toUpperCase():转换为大写
s.toLowerCase():转换为小写
```

## 比较

```text
@return boolean
s.equal(s1):比较字符串是否相同
s.equalIgnoreCase(s1):比较字符串是否相同忽略大小写
s.regionMatches(from,s1,s1from,len):比较s[from,from+len]与s1[s1from.s1from+len]是否相同

@return int
s.compareTo(s1):按字典序比较字符串大小
s.compareToIgnoreCase(s1):按字典序比较字符串是否相同忽略大小写
相同返回0 仅长度不相同返回长度差值 不相同返回第一个不同处字符差值
```

## 正则表达式
```text

```

# 转换
```text
@return String
String.valveOf(E):将(基本类型)E转换为String,等于各自包装类的toString

@return String[]
s.split(s1,n):将s按s1规则分成n份

@return String
String.join(symbol,seq...):将序列按symbol分隔符拼接

@return String
s.subString(start,@not end):截取s[start,end)部分字符串

@return CharSeq
s.subSequence(start,@not end):截取s[start,end)部分字符串

@return String
s.strip():去掉首尾空格

@return String
String.format("%x",E):按指定规则格式化字符串
```

## 补充
