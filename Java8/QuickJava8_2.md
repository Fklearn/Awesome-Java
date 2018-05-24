# �����ѧϰJava8�������

## 1������

1. Java8����Collection��������һ��stream()�������÷�������һ��Stream���͡����Ǿ����ø�Stream����������̵ģ�
2. ���뼯�ϲ�ͬ������ֻ���ڰ������ģ����������Ѿ�������ϲ����ڻ����еģ�
3. ���������һ����ֻ�ܱ�����һ�Σ������Ҫ�ٱ���һ�飬��������´�����Դ��ȡ���ݣ�
4. �ⲿ��������ָ��Ҫ�û�ȥ���������ڲ������ڿ�����ɵģ������û�ʵ�֣�
5. ����������������������Ϊ�м�������ر����Ĳ�����Ϊ�ն˲���������ʽ�Ͽ���������`.`�������Ĳ����У��м����Щ���м���������յ��Ǹ��������ն˲�������

## 2��ɸѡ

### 2.1 ����

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
	
### 2.3 ����

    Stream<T> limit(long maxSize);

������SQL�����limit��䣬������Ҳ�����Ƶ�limit()���������������Ʒ��صĽ�������������������ͷ��ʼȡ�̶�������Ԫ�أ�Ҳ���м������ʹ����֮����Ȼ�᷵��һ������

    List<Integer> list = Arrays.asList(1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9);
    List<Integer> filter = list.stream().filter(integer -> integer > 3).limit(3).collect(Collectors.toList());
    // [4, 5, 5]
	
### 2.4 ����

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

## 4������

    Optional<T> findFirst();
    Optional<T> findAny();

��ָ�������в���Ԫ�ص�ʱ�������������������������Stream�ӿ��еķ��������ص��Ѿ�������Stream�����ˣ������˵���������ն˲��������ԣ�ͨ��Ҳ�����������նˣ����������Ļ���Ҫʹ��Optional�ӿڵķ����ˡ�

    List<Integer> list = Arrays.asList(1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9);
    Optional<Integer> optionalInteger = list.stream().filter(integer -> integer > 10).findAny();
    Optional<Integer> optionalInteger = list.stream().filter(integer -> integer > 10).findFirst();

������ʹ�õ�����ʾ�������ﷵ�صĽ����Optional���͵ġ�Optional����ƽ����Guava�е�Optional��ʹ�����ĺô����㲻��Ҫ����ǰһ�������صĽ����null�����жϣ����ڽ��Ϊnull��ʱ��ͨ��`=`��ֵһ��Ĭ��ֵ�ˡ�ʹ��Optional�еķ���������Ը����ŵ������ͬ�Ĳ��������������г�Optional�е�һЩ���õķ�����

|���|����|˵��|
|:-:|:-:|:-:|
|1|isPresent()|�ж�ֵ�Ƿ���ڣ����ڵĻ��ͷ���true�����򷵻�false|
|2|isPresent(Consumer<T> block)|��ֵ���ڵ�ʱ��ִ�и����Ĵ���|
|3|T get()|���ֵ���ڣ���ô���ظ�ֵ�������׳�NoSuchElement�쳣|
|4|T orElse(T other)|���ֵ���ڣ���ô���ظ�ֵ�������򷵻�other|
	
## 5��ƥ��

    boolean allMatch(Predicate<? super T> predicate);
    boolean noneMatch(Predicate<? super T> predicate);
    boolean anyMatch(Predicate<? super T> predicate);
	
�Ӷ��������������������������Ҳ���ն˲��������Ƿֱ������жϣ����е������Ƿ�ȫ��ƥ��ָ�������������е������Ƿ�ȫ����ƥ��ָ�������������е������Ƿ����һЩƥ��ָ����������������һЩʾ����
	
    List<Integer> list = Arrays.asList(1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9);
    boolean allMatch = list.stream().allMatch(integer -> integer < 10);
    boolean anyMatch = list.stream().anyMatch(integer -> integer > 3);
    boolean noneMatch = list.stream().noneMatch(integer -> integer > 100);

## 6����Լ

    Optional<T> reduce(BinaryOperator<T> accumulator);
    T reduce(T identity, BinaryOperator<T> accumulator);

Stream�ӿ��е�reduce���������������ذ汾���������Ǹ������õ������Ķ��塣���ǻ��������Ƶģ�ֻ�ǵڶ������������б��ж��˸���ʼֵ����û�г�ʼֵ���Ǹ���������Optinoal���ͣ����ԣ����𲻴�����ֻҪ������������Ϊ�Ϳ����ˡ������ǹ�Լ�����ӣ�

    List<String> list = Arrays.asList("a", "b", "c", "d", "e", "f");
    String ret = list.stream().reduce("-", (a, b) -> a + b);

������������`-abcdef`����Ȼ����Ч�����ǣ����磬`$`��ĳ�ֲ�����List��ĳ��"����"����ô��Լ���������`��ʼֵ$n[0]$n[1]$n[2]$...$n[n-1]`��




