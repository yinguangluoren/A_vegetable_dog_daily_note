## 啥是个Spliterator
先老年英文翻译下文档
不翻译了，反正就是个支持拆分遍历的遍历器
一般实现的类都是工具类里面的内部类

```java
import lombok.Getter;

import java.util.Spliterator;
import java.util.function.Consumer;

@Getter
public class SpliteratorTest implements Spliterator {

    private final Object[] data;

    // 当前的下标
    private int index;

    private final int size;

    private int fence;

    public SpliteratorTest(Object[] data, int index, int size, int fence) {
        this.data = data;
        this.index = index;
        this.size = size;
        this.fence = fence;
    }

    /**
     * 如果当前spliterator 还有剩余的元素 ， 则对剩余元素执行action， 返回true， 否则返回false
     * @param action
     * @return
     */
    @Override
    public boolean tryAdvance(Consumer action) {
        if (index <= fence) {
            action.accept(data[index++]);
            return true;
        }
        return false;
    }

    /**
     * 如果当前遍历的集合是可以分割的，该方法会返回个分割后的遍历器
     * 除非集合的元素是无穷的，不断调用该方法会最终返回null
     * @return
     */
    @Override
    public SpliteratorTest trySplit() {
        if (size == 0 || index == fence) {
            return null;
        }
        int mid = index + fence >>> 1;
        return new SpliteratorTest(data, index, mid-index+1, index = mid);
    }

    /**
     * 预估foreachRemaining 仍需遍历多少元素
     * @return
     */
    @Override
    public long estimateSize() {
        return fence - index;
    }

    @Override
    public int characteristics() {
        return 0;
    }

    public void reset() {
        index = 0;
        fence = data.length;
    }

    public static void main(String[] args) {
        Integer[] s = new Integer[]{1,2,3,4,5};
        SpliteratorTest t = new SpliteratorTest(s, 0, s.length, s.length);
        t.tryAdvance(a -> System.out.println(a));
        System.out.println(t.getIndex());
        t.reset();
        
        // 二分一个迭代器出来
        SpliteratorTest t2 = t.trySplit();
        System.out.println(t2.getIndex());
        System.out.println(t.getIndex());
        t2.tryAdvance(a -> System.out.println(a));
    }
}

```