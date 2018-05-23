# �����ѧϰJava8�������

## 1������

1. Java8����Collection��������һ��stream()�������÷�������һ��Stream���͡����Ǿ����ø�Stream����������̵ģ�
2. ���뼯�ϲ�ͬ������ֻ���ڰ������ģ����������Ѿ�������ϲ����ڻ����еģ�
3. ���������һ����ֻ�ܱ�����һ�Σ������Ҫ�ٱ���һ�飬��������´�����Դ��ȡ���ݣ�
4. �ⲿ��������ָ��Ҫ�û�ȥ���������ڲ������ڿ�����ɵģ������û�ʵ�֣�
5. ����������������������Ϊ�м�������ر����Ĳ�����Ϊ�ն˲���������ʽ�Ͽ���������`.`�������Ĳ����У��м����Щ���м���������յ��Ǹ��������ն˲�������

## 2��ɸѡ

### 2.1 ����������

    Stream<T> filter(Predicate<? super T> predicate);

filterͨ��ָ��һ��Predicate���͵���Ϊ���������е�Ԫ�ؽ��й��ˣ����ջ��ǻ᷵��һ��������Ϊ�����м�������м�������صĽ������һ���������ԣ����������Ҫ�õ�һ�����ϻ��������ķ������ͣ�����Ҫʹ���ն˲�������ȡ��

    List<Integer> list = Arrays.asList(1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9);
    List<Integer> filter = list.stream().filter(integer -> integer > 3).collect(Collectors.toList());
    // [4, 5, 5, 6, 7, 8, 9]
	
### 2.2 ȥ��

    Stream<T> distinct();
	
�������ȥ�صķ����Ķ��壬���ᰴ�����е�Ԫ�ص�equal()��hashCode()��������ȥ�ء�ȥ��֮�󽫼�������һ������������Ҳ���м������

    List<Integer> list = Arrays.asList(1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9);
    List<Integer> filter = list.stream().filter(integer -> integer > 3).distinct().collect(Collectors.toList());
	// [4, 5, 6, 7, 8, 9]
	
### 2.3 ���Ʒ��ؽ��

    Stream<T> limit(long maxSize);

������SQL�����limit��䣬������Ҳ�����Ƶ�limit()���������������Ʒ��صĽ�������������������ͷ��ʼȡ�̶�������Ԫ�أ�Ҳ���м������ʹ����֮����Ȼ�᷵��һ������

    List<Integer> list = Arrays.asList(1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9);
    List<Integer> filter = list.stream().filter(integer -> integer > 3).limit(3).collect(Collectors.toList());
    // [4, 5, 5]
	
### 2.4 ����Ԫ��

    Stream<T> skip(long n);

��������Ķ����limit()�������ơ���Ҳ���м��������������������ͷ��ʼָ��������Ԫ�أ�ʹ����֮����Ȼ�᷵��һ������

    List<Integer> list = Arrays.asList(1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9);
    List<Integer> filter = list.stream().filter(integer -> integer > 3).skip(3).collect(Collectors.toList());
	// [6, 7, 8, 9]
	
## 3��ӳ��

    <R> Stream<R> map(Function<? super T, ? extends R> mapper);

���ǵ�Function�����ӿڵķ�����������������������ת������һ�����͡������������map()�����е�Ӧ�á�����������ʹ���˸÷���֮�����ͻ᳢�Խ���ǰ�������е�Ԫ��ת������һ�����͡���������ն˲���collect()��ʱ����ȻҲ�͵õ�����һ�����͵ļ��ϡ�

    List<Integer> list = Arrays.asList(1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9);
    List<String> filter = list.stream().map((integer -> String.valueOf(integer) + "-")).collect(Collectors.toList());
    // �����[1-, 1-, 2-, 3-, 4-, 5-, 5-, 6-, 7-, 8-, 9-]




