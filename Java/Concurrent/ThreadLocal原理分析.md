# ThreadLocalԭ��

��ֹ�����ڹ�����Դ�ϲ�����ͻ��һ�ַ�ʽ�Ǹ����Ա����Ĺ���ʹ���̵߳ı��ش洢Ϊʹ����ͬ�����Ĳ�ͬ�̴߳�����ͬ�Ĵ洢��

������һ��ThreadLocal��ʵ������������ʹ���˾�̬��ȫ�ֱ���threadLocal������Integer���͵�ֵ��������main�̡߳������ڲ�ͬ���߳��н�ָ�������ִ��뵽threadLocal�н��б��档Ȼ���ٽ����ȡ������

    private static ThreadLocal<Integer> threadLocal = new ThreadLocal<Integer>();

    public static void main(String...args) {
        threadLocal.set(-1);
        ExecutorService executor = Executors.newCachedThreadPool();
        for (int i=0;i<5;i++) {
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

ÿ���̶߳���ȷ�ض�ȡ�����˱��浽ThreadLocal�е����ݡ�

## 1��ThreadLocal������ԭ��

������ĳ����У����ǽ�ThreadLocal����Ϊȫ�ֵľ�̬����������ÿ��ֻҪ��ָ�����߳�ִ�еķ����У�����ThreadLocal��set()��get()�����ͽ�ֵ���浽��ָ�����̵߳����¡���ʵ������ǰ����ִ�е��߳���ThreadLocal�ж�д���ݵ�ʱ���Ƕ����ڵ�ǰ�߳�����ɵģ�ÿ�λ����Thread.currentThread()������ȡ��ǰ���̣߳�Ȼ���ٻ�ȡ���߳��е�һ����ϣ�����õ�ǰ��ThreadLocal��Ϊ���ӹ�ϣ���л�ȡֵ�����ԣ��������Լ�ֵ�Ե���ʽ�洢���߳��еġ�

### 1.1 д����

��������һ���̵߳��ڲ�����һ��ThreadLocal��д�����ݵ�ʱ�򣨵���set()����������Ҫִ�е��߼����£�

    public void set(T value) {
	    // ��ȡ��ǰ�߳�
        Thread t = Thread.currentThread();
		// ��getMap()�����л᷵��Thread��threadLocals�ֶΣ����ֶ�Ĭ����null��
        ThreadLocalMap map = getMap(t);
        if (map != null)
          // �Լ�ֵ�Ե���ʽ������д�뵽��ϣ����
            map.set(this, value);
        else
		    // ���ָ��Thread��Ӧ��threadLocals�ֶ�ΪNull����ʵ����һ��
            createMap(t, value);
    }

��������ThreadLocal��`set()`������ʱ�����ǻ�����`Thread.currentThread()`������ȡ��ǰ�̡߳�Ȼ����`getMap(Thread)`������ȡ��ǰ�̵߳�`threadLocals`�ֶΡ�����ֶ���`ThreadLocalMap`��ʵ��������һ����ϣ��ÿ��Entry�Ķ���һ���̳���WeakReference��ʵ����Ҳ���������á����ļ���`ThreadLocal<?>`��ֵ��Object���͡�Ȼ�����ǵ�����`ThreadLocalMap`��`set(ThreadLocal<?>, Object)`������ʱ�򣬻Ὣָ���ļ�ֵ�Բ��뵽�ù�ϣ���С���ʵ�������ThreadLocalMap��ʵ������Thread�У�����Ҫʵ��һ��һ�Զ�Ĺ�ϵ����һ��Thread��Ӧ����ThreadLocal<?>��

�ܽ᣺���ڵ�ǰ���߳��е���ThreadLocal.set()������ʱ��ʵ�����ǽ���ThreadLocal��Ϊһ�����������ڹ�ϣ��(ThreadLocalMap)�и��ݸü���ȡ��Ӧ��ֵ����ʵ�ʵ��������Լ�ֵ��(ThreadLocalMap.Entry)����ʽ�洢��Thread���еġ�

### 1.2 ������

�����Ƕ�ȡ������ص��߼���

    public T get() {
	    // ��ȡ��ǰ�߳�
        Thread t = Thread.currentThread();
		// ��ȡ��ǰ�̵߳�Map
        ThreadLocalMap map = getMap(t);
        if (map != null) {
		    // ��ȡ��ǰThreadLocal��Ӧ��Entry����Entry��һ�����������ͣ����ܱ�����
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
			    // ��Entry�л�ȡ���
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }

�����￴��������ȷҲ�ǣ��Ȼ�ȡ����ThreadLocal��get()�������̣߳�Ȼ���ȡ���߳��ڲ���Map��Ȼ�󽫵�ǰ��ThreadLocal��Ϊ������map�ж�ȡһ����ֵ�ԣ������ж�ȡ����Ľ����

### 1.3 �ײ����ݽṹ

��������˵ThreadLocal�ı������Ǳ�����Thread�ڲ���һ������ThreadLocalMap�У�����������ô�洢���أ�

���ȣ����ڲ������������ڹ�ϣ��Ľ����࣬�����������̳���WeakReference��Ҳ����˵���ڴ治���õ�ʱ��ü�ֵ�Կ��ܻᱻ���ա�

    static class Entry extends WeakReference<ThreadLocal<?>> {
        Object value;

        Entry(ThreadLocal<?> k, Object v) {
            super(k);
            value = v;
        }
    }

Ȼ����ThreadLocalMap�Ķ��塣��������Կ����������ڲ�������һ��Entry���͵����飺

    static class ThreadLocalMap {

        private Entry[] table;
    
        // ....
    }

��Ҳ����ʵ�ֹ�ϣ��һ�ַ�ʽ����HashMap��TreeMap��ͬ�ĵط����ڣ�����ʵ�ֵĹ�ϣ�洢�ǻ�������̽�ⷨ��ʵ�ֵġ���Ҳ�����涨������Ľ���ԭ��������������ʽ�Ľ�㣬���Խ�������Ϊһ����ͨ�������һ��Ԫ�ء���������������д洢Ԫ�ص�ʱ��Ҳ���ǰ�һ������ֵ�ԡ������������ʱ����ʵ����ִ��������Ĳ�����

    private void set(ThreadLocal<?> key, Object value) {

        ThreadLocal.ThreadLocalMap.Entry[] tab = table;
        int len = tab.length;
        // ���ﵹ�Ǹ�HashMapһ������ʹ��2������������ʵ�֣���ָ����ThreadLocal
        // ӳ�䵽�����������������������ȡThreadLocal�Ĺ�ϣ��ĺ�λ
        int i = key.threadLocalHashCode & (len-1);

        // ������һ������������Ŀ������Ѱ���뵱ǰ��ThreadLocal�Ĺ�ϣ����ȵ�����Ԫ��
        // ��Ѱ�ҵĿ�ʼλ����ǰ�����ó���������Ȼ����һ�������ء��н��в���
        for (ThreadLocal.ThreadLocalMap.Entry e = tab[i];
             e != null;
             e = tab[i = nextIndex(i, len)]) {
            ThreadLocal<?> k = e.get();

            if (k == key) {
                e.value = value;
                return; // ע�⣬�ҵ��˸���֮��ͷ�����
            }

            if (k == null) {
                replaceStaleEntry(key, value, i);
                return;
            }
        }

        // ��ʾû���ҵ����½�һ����㲢��ֵ�������ָ��Ԫ��
        tab[i] = new ThreadLocal.ThreadLocalMap.Entry(key, value);
        // ɢ�б��д洢�ļ�¼����+1
        int sz = ++size;
        // ����Ѿ��洢��������������ָ������������ô������������ݣ������¼����ϣֵ
        if (!cleanSomeSlots(i, sz) && sz >= threshold)
            rehash();
    }

���Ͼ���ThreadLocal�Ļ����÷��͵ײ��ʵ�ֵ�ԭ��