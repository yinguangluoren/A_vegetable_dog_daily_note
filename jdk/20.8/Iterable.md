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
        for (T t : this) {// iterable使用的是fori的遍历形式支持foreach
            action.accept(t);
        }
    }
```                      

```java
 default Spliterator<T> spliterator() {
        return Spliterators.spliteratorUnknownSize(iterator(), 0);
    }
```

### iterator
Iterator
遍历器，迭代器的简称，是iterable的声明方法，即每一个实现该接口的类都需要提供返回一个遍历器的实现方法 用于遍历，iterator通常指代集合中当前正在遍历的那个元素的前一个位置，可以想象为元素和元素之间的缝隙就是一个个iterator

Iterator 的定义也是一个接口，其声明了作为一个迭代器应具有哪些特性，
1.	返回当前iterator是否存在下一个元素 Boolean hasNext();
2.	返回当前iterator的下一个元素 E next();  (这里E是一个泛型声明)
3.	移除当前iterator之前的元素，如果该位置之前没元素(即未调用过next方法，则会出现IllegalStateException) 所以remove方法必须搭配next方法使用，如果再next和remove的时候，集合出现了结构的改变，则也会出现IllegalStateException。
```java
default void remove(){throw new UnsupportedOperationException(“remove”);} 
```
> 接口的默认实现会抛出异常，需要实现类自己实现覆盖。 

4.遍历方法，使用iterator遍历该集合的方法
```java
default void forEachRemaining(Consumer<? Super E> action){ // Consumer是一个实现了FunctionalInterface的接口,其声明的E的父类的action对象，可以对集合的每一个元素都执行action对象的accept方法
    Objects.requireNonNull(action);
    while(hasNext()){
        action.accept(next())
    }
}
```

