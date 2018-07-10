# ThreadLocal��ʹ�ü���Դ��ʵ��

## 1��ThreadLocal��ʹ��

��ֹ�����ڹ�����Դ�ϲ�����ͻ��һ�ַ�ʽ�Ǹ����Ա����Ĺ���ʹ���̵߳ı��ش洢Ϊʹ����ͬ�����Ĳ�ͬ�̴߳�����ͬ�Ĵ洢��

������һ��ThreadLocal��ʵ������������ʹ���˾�̬��ȫ�ֱ���`ThreadLocal`����������`Integer`���͵�ֵ��
�����ڲ�ͬ���߳��н�ָ�������ִ��뵽`threadLocal`�н��б��档Ȼ���ٽ����ȡ������

    private static ThreadLocal<Integer> threadLocal = new ThreadLocal<Integer>();

    public static void main(String...args) {
        threadLocal.set(-1);
        ExecutorService executor = Executors.newCachedThreadPool();
        for (int i=0; i<5; i++) {
            final int ii = i; // i������final�ģ�������ʱ����
            executor.submit(new Runnable() {
                public void run() {
                    threadLocal.set(ii);
                    System.out.println(threadLocal.get());
                }
            });
        }
        executor.shutdown();
        System.out.println(threadLocal.get());
    }

�ӳ����ִ�н�����Կ�����ÿ���̶߳���ȷ�ض�ȡ�����˱��浽ThreadLocal�е����ݡ�

���ԣ������ܽ�һ��`ThreadLocal`�����þ��ǣ��洢��`ThreadLocal`�еı������̰߳�ȫ�ģ�ÿ���߳�ֻ�ܶ�ȡ���Լ��洢��ֵ��

ͨ������ʹ�÷�ʽ���Ƕ���һ����̬ȫ�ֵ�`ThreadLocal`ʵ����Ȼ��ÿ���߳�ʹ��������дֻ���Լ����õ������ݡ�
���磬����ҪΪÿ���̴߳�����һ�����ݿ����ӣ����Ҹ�����ֻ������߳��Լ�ʹ�ã���ô���Խ����洢��`ThreadLocal`�У�
Ȼ�����õ��ĵط���ȡ��

������������ӣ�Ҳ�����������һЩ���⣺

1. ThreadLocal�д洢��ֵ����α�֤���Ե��̰߳�ȫ�ģ�
2. ��ô��Щֵ�Ǵ洢��ʲô�ط���
3. �Ǿ�̬���͵Ļ���ʵ�����͵ģ�
4. ���ĳ���߳�ִ������ˣ��������ˣ���ô��Щ�洢��ֵ�ᱻ��ô�����أ�
5. ����

�����������Щ���⣬������������JDKԴ����`ThreadLocal`�����ʵ�ֵġ�

## 2��ThreadLocal������ԭ��

���ǻ����ȴӶ�ȡ�Ĳ���������

������`ThreadLocal`��`set()`�����Ĵ��룺

    public T get() {
        Thread t = Thread.currentThread();  // 1
        ThreadLocalMap map = getMap(t);  // 2
        if (map != null) {  // 3
            ThreadLocalMap.Entry e = map.getEntry(this);  // 4
            if (e != null) {
                T result = (T) e.value; // 5
                return result;
            }
        }
        return setInitialValue();
    }

�������Ȼ��ٲ���1�л�ȡ����ǰ�̵߳�ʵ����Ȼ���ڲ���2��ͨ��`getMap()`������ʹ�õ�ǰ���̵߳�`ThreadLocalMap`��
�����`ThreadLocalMap`�Ķ������£�

    static class ThreadLocalMap {

        static class Entry extends WeakReference<ThreadLocal<?>> {
            Object value;
            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }
		
        private Entry[] table;
		
        private Entry getEntry(ThreadLocal<?> key) {
            int i = key.threadLocalHashCode & (table.length - 1);
            Entry e = table[i];
            if (e != null && e.get() == key)
                return e;
            else
                return getEntryAfterMiss(key, i, e);
        }
		
        // ...
    }

Ȼ�����ǿ���`getMap()`�����Ķ��壺

    ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }

Ҳ����˵ʵ���ϵ����ǵ���`get()`������ʱ�򣬻��Ȼ�ȡ��ǰ�߳��е�`threadLocals`�ֶΣ����ֶ���`ThreadLocalMap`���͵ġ�
Ȼ������ʹ�õ�ǰ��`ThreadLocal`ʵ����Ϊ�����ӹ�ϣ���л�ȡ��һ��`Entry`����ʵ�ʵ�ֵ�ͱ�����`Entry`��`value`�ֶ��С�

���������`getEntry()`����������������ƺ�����Ĺ�ϣ��ֻ��һ�����飬�ǹ�ϣ��ͻ����ô������أ�
ʵ���ϣ�����֪��ͨ�������ϣ��ͻ�����ֽ����ʽ��һ������������һ��������̽�ⷨ��
ǰ����`HashMap`��`ConcurrentHashMap`��ʹ�ý϶࣬�������õ�����ʵ��������̽�ⷨ��
˵���˾��ǽ����е�ֵ����һ����������Ȼ�����ɢ�еĽ����������ȡֵ�������ʵ�ַ�ʽ���Կ���ص����ݽṹ֪ʶ�㡣

����Ĺ�ϵ�ǲ����е��ң���������һ�£�

����ʹ��`ThreadLocal`�洢��ֵʵ���Ǵ洢��`Thread`ʹ��`ThreadLocalMap`���еģ�
�������`ThreadLocal`ʵ��ֵ����һ����ϣ��ļ������ã�

![ThreadLocal](res/ThreadLocal.png)

������ͼ��ʾ�������������������߳�`thread1`�е�����`threadLocal1`��`get()`������
���Ȼ���`Thread.currentThread()`������ȡ��`thread1`��Ȼ���ȡ��`thread1`��`threadLocals`ʵ����
`threadLocals`��һ��`ThreadLocalMap`���͵Ĺ�ϣ��Ȼ����������`threadLocal1`��Ϊ��
����`threadLocals`�л�ȡ��ֵ`Entry`������`Entry`��ȡ���洢��ֵ�����ء�

���ˣ������Ѿ��˽���ThreadLocal��ʵ�ֵ�ԭ�������뿴��`set()`�����ģ����ǵ����Ѿ������������ˣ�����Ҳ��û�м�����ȥ�ı�Ҫ�ˡ�

## 3���ܽ�

���ǻع�ͷ������֮ǰ����ļ������⣺

1. ThreadLocal�д洢��ֵ����α�֤���Ե��̰߳�ȫ�ģ�
ʵ����ÿ��ֵ���Ǵ����߳��ڲ��ģ�ThreadLocalֻ�����������ǴӸ��߳��ڲ��Ĺ�ϣ�����ҵ���ŵ��Ǹ�ֵ��
2. ��ô��Щֵ�Ǵ洢��ʲô�ط����߳��ڲ���ʵ���ֶΡ�
3. �Ǿ�̬���͵Ļ���ʵ�����͵ģ��߳��ڲ���ʵ���ֶΡ�
4. ���ĳ���߳�ִ������ˣ��������ˣ���ô��Щ�洢��ֵ�ᱻ��ô�����أ���Ϊ���̵߳ľֲ��ֶΣ������̲߳����ˣ�ֵ��û���ˡ�

���Ͼ���ThreadLocal���÷���ʵ��ԭ��
