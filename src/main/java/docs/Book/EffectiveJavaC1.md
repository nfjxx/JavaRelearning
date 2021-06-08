# Effective Java

## Chapter 1.Create and Destroy Object

### 1.Static factory method instead of constructor

Classic case:
```java
public static Boolean valueOf(boolean b){
    return b?Boolean.TRUE:Boolean.FALSE;
}
```
Advantage:
1. Have name; 
2. Don't create new object;
3. Return an object of any subtype of the original return type; 
4. Retrun class change by parameters;
5. Retrun class can not exist before create

Disadvantage:
1. Difficult to find;
2. Can't be subclassed if constructor isn't public or protected 

Common examples:
1. form
2. of
3. valueof
4. instance or getInstance
5. create or newInstance
6. getType
7. newType
8. type
