threadlocal导致内存泄漏
```
static class ThreadLocalMap {
    // 定义一个Entry类，key是一个弱引用的ThreadLocal对象
    // value是任意对象
    static class Entry extends WeakReference<ThreadLocal<?>> {
        /** The value associated with this ThreadLocal. */
        Object value;
        Entry(ThreadLocal<?> k, Object v) {
            super(k);
            value = v;
        }
    }
    // 省略其他
}
```
垃圾收集器工作时，会回收掉只被弱引用关联的对象实例--wrong
ThreadLocalMap还持有ThreadLocal的强引用,所以ThreadLocalMap的生命周期跟Thread一样长--true

每次使用完ThreadLocal，建议调用它的remove()方法，清除数据
