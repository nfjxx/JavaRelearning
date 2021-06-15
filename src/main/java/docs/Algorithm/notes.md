## 算法

### 位运算

0|取结果 1&做判断 

0|=(1<<x) 令x位为1

### Map

CountNums:

```java
for(Object c: Collections){
    map.put(c,map.getOrDefault(c,0)+1);
}
```