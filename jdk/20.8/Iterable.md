### Iterable 接口
直接翻译， 即为可遍历的，实现该接口的类通常为集合类或者是抽象的集合类或者集合接口，
比如java.util.AbstractList

### 方法
Iterable 接口有3个方法

- 一个抽象方法
- 两个default方法 
   > jdk8之后接口支持带default方法，实现类会自动继承该方法的方法体， 如果实现类实现了俩个接口具有相同的default方法，则实现类必须重写该方法。
   > 如果实现类的父类和实现接口都有相同default方法，则父类优先

### 具体方法说明
```java
   Iterator<T> iterator();
```

   
```java
default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }
```                      

```java
 default Spliterator<T> spliterator() {
        return Spliterators.spliteratorUnknownSize(iterator(), 0);
    }
```
