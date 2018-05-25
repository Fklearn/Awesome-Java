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

## 7����ֵ��

ͬ������Ϊװ�������ԭ��Java8��Ϊ��ֵ����ר���ṩ����ֵ����IntStream DoubleStream��LongStream��Stream�ӿ��ṩ�������м䷽������ɴ�������ӳ�䵽��ֵ���Ĳ�����

    IntStream mapToInt(ToIntFunction<? super T> mapper);
    LongStream mapToLong(ToLongFunction<? super T> mapper);
    DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper);
	
��������������������������������л�ȡ��ֵ����Ȼ����������ֵ���ķ�������������Ĳ���������������ֵ����Stream�ӿڶ��̳���BaseStream���������ǰ����ķ�������������ģ�����������˵��ͬС�졣Stream�ȽϾ���һ���ԣ�����������ֵ����������ԣ�����Ҳ�ṩ���������ķ����������Ҫ����ֵ���л�ȡ������������Ե������ǵ�`boxed()`����������ȡװ��֮�������

�������ἰһ�£�����Optional��Java8ҲΪ�����ṩ�˶�Ӧ����ֵ���ͣ�OptionalInt OptionalDouble OptionalLong��

�������������ֵ���л��м�����̬�������ڻ�ȡָ����ֵ��Χ������

    public static LongStream range(long startInclusive, final long endExclusive)
    public static LongStream rangeClosed(long startInclusive, final long endInclusive)

���������ڻ�ȡָ����Χ��LongStream�ķ�����һ����Ӧ����ѧ�еĿ����䣬һ����Ӧ����ѧ�еı�����ĸ��
	
## 8��������

���������ڻ�ȡ����ʱ��ʵ���϶��Ǵ�Collection��Ĭ�Ϸ���`stream()`�л�ȡ����������Щ��׾��ʵ���ϣ�Java8Ϊ�����ṩ��һЩ�������ķ�������������о�һ����Щ������

    public static<T> Builder<T> builder() // 1
    public static<T> Stream<T> empty() // 2
    public static<T> Stream<T> of(T t) // 3
    public static<T> Stream<T> of(T... values) // 4
    public static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f) // 5
    public static<T> Stream<T> generate(Supplier<T> s) // 6 
    public static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b) // 7

����ķ�������Stream�ӿ��еľ�̬���������ǿ�������Щ��������ȡ�������������Ƕ�ÿ��������һЩ��Ҫ��˵����

1. �������ϾͿ��Կ�������ʹ���˹�����ģʽ�������ÿ�ε���Builder��`add()`��������һ��Ԫ������������
2. ��������һ���յ���
3. ����һ��ֻ����һ��Ԫ�ص���
4. ʹ�ò�����������һ������ָ��Ԫ�ص���
5. Ū�������ԭ��ؼ���Ҫ�����׺����UnaryOperator�ĺ��壬����һ������ʽ�ӿڣ����Ҽ̳���Function����֮ͬ������������κͻز�������ͬ�����������ԭ���Ǵ�ĳ������ֵ��ʼ�����պ���ĺ����Ĺ�����м��㣬ÿ������֮ǰ��ֵ�Ļ�����ִ��ĳ�������ġ�����`Stream.iterate(2, n -> n * n).limit(3)`��������`2 4 16`���ɵ�����
6. �����SupplierҲ��һ�������ӿڣ���ֻ��һ��get()�������޲Σ�ֻ����ָ�����͵ķ���ֵ�����ԣ����������Ҫ���ṩһ������������ֵ�ĺ���������˵���򣩣�����Math.random()�ȵȡ�
7. ����Ƚ�������⣬����ͨ�����������ϲ����õ�һ���µ�����
	
## 9���ռ���

���������Ѿ���ʶ�������Ĺ�Լ������������Щ�������Ƚ����ɡ�Java8���ռ���Ϊ�����ṩ�˸���ǿ��Ĺ�Լ���ܡ�

˵���ռ������϶��Ʋ���������Collector��Collectors��������ɶ��ϵ�أ���ʵCollectorֻ��һ���ӿڣ�Collectors��һ����, ���еľ�̬�ڲ���CollectorImplʵ���˸ýӿڣ����ұ�Collectors�����ṩһЩ���ܡ�Collectors�������ľ�̬�������ڻ�ȡCollector��ʵ����ʹ����Щʵ�����ǿ�����ɸ��ӵĹ��ܡ���Ȼ������Ҳ����ͨ��ʵ��Collector�ӿ��������Լ����ռ�����

Stream��collect()������3�����صİ汾�����Ǿ���ͨ�����е�һ����ʹ���ռ����ģ��������Ķ��壺

    <R, A> R collect(Collector<? super T, A, R> collector);
	
����ע��һ����������Ĳ����ͷ�������. ���������ǿ��Կ��������Collector��3������,���е����һ��������R�뷵�ص�������һ�µ�. �����Ҫ��������Ԥ���������ĳ������ȴ��֪�����շ��ص���ʲô���͡�

����������һЩ�򵥵����ӣ������stream����Student���󹹳ɵ�����

    Optional<Student> student = stream.collect(Collectors.maxBy(comparator))  // ��Ҫ����һ���Ƚ�����maxBy()������
    long count = stream.collect(Collectors.counting())

��������ַ�ʽ�Ƚϼ��ߣ���Ϊ�����ʹ��count()��max()������������ǡ����������ٿ�һЩ�ռ������������ӣ�ע������Щ�����У��Ҳ�û��ʹ��lambda�򻯺���ʽ�ӿڣ�����Ϊ��Ҫ�������ؿ������ķ����ͺͷ������塣������������������Щ���������û���
	
### 9.1 ����ƽ��ֵ������

�����������ڼ���ƽ��ֵ�����ƵĻ���summingInt()���ڼ������������ǵ��÷������Ƶġ�
	
    Double d = stream.collect(Collectors.averagingInt(new ToIntFunction<Student>() {
        @Override
        public int applyAsInt(Student value) {
            return value.getGrade();
        }
    }));
 
���������ǿ���������averagingInt()������ʱ����Ҫ����һ��ToIntFunction����ʽ�ӿڣ����ڸ���ָ�������ͷ���һ������ֵ��

### 9.2 �����ַ���

joining()����������ר�����������ַ����ģ���Ҫ�������ַ������������ڶ�Student������ƴ��֮ǰ����Ҫ�Ƚ���ӳ����ַ�������

    String members = stream.map(new Function<Student, String>() {
        @Override
        public String apply(Student student) {
           return student.getName();
        }
    }).collect(Collectors.joining(", ")); // ʹ��','���ַ���ƴ������
	
### 9.3 ����Ĺ�Լ����

    Optional<Student> optional = stream.collect(Collectors.reducing(new BinaryOperator<Student>() {
        @Override
        public Student apply(Student student, Student student2) {
            return student.getGrade() > student2.getGrade() ? student : student2;
        }
    }));

����ľ���������Լ�ĺ�������������reducing�����������������д���һ��BinaryOperator���͡���������ָ�����յķ���������Student�����ԣ�����Ĵ����Ч���ǻ�ȡ�ɼ�����ѧ����

### 9.4 ����

Collectors�еķ��黹�ǱȽ�����˼�ġ������ȿ�groupingBy�����Ķ��壺

    Collector<T, ?, Map<K, D>> groupingBy(Function<? super T, ? extends K> classifier)
    Collector<T, ?, Map<K, D>> groupingBy(Function<? super T, ? extends K> classifier, Collector<? super T, A, D> downstream)

groupingBy������3�����صİ汾���������Ǹ������г��õ���������һ��������ͨ��ָ������������з���ģ����ڶ���������ͨ��classifierָ���Ĺ���������з��飬Ȼ����downstream�Ĺ���Է����������к����Ĳ�����ע��ڶ���������Ȼ��Collector���ͣ���˵��������Ȼ���ԶԷ��������ٴ��ռ��������ٷ��顢�����ֵ�ȵȡ�

    Map<Integer, List<Student>> map = stream.collect(Collectors.groupingBy(new Function<Student, Integer>() {
        @Override
        public Integer apply(Student student) {
           return student.getClazz();
        }
    }));
	
������groupingBy()�����ĵ�һ�����ӡ�ע������������ͨ����Studentͨ��'�༶�ֶ�'ӳ���һ�����������з���ġ�������һ�����η�������ӡ��������������ĵڶ���groupingBy()����������downstream��ָ������һ�����������

    Map<Integer, Map<Integer, List<Student>>> map = stream.collect(Collectors.groupingBy(new Function<Student, Integer>() {
        @Override
        public Integer apply(Student student) {
           return student.getClazz();
        }
    }, Collectors.groupingBy(new Function<Student, Integer>() {
        @Override
        public Integer apply(Student student) {
            return student.getGrade() == 100 ? 1 : student.getGrade() > 90 ? 2 : student.getGrade() > 80 ? 3 : 4;
        }
    })));
	
### 9.5 ����

��������ƵĻ���һ�������Ĳ���������ֻ�Ƿ����һ�����������ǵ�ʹ�÷�ʽҲ����һ�£����ķ���ǩ���������groupingBy�������ơ�����ֱ�ӿ�����һ��ʹ�õķ�ʽ���ˣ�

    Map<Boolean, List<Student>> map = stream.collect(Collectors.partitioningBy(new Predicate<Student>() {
        @Override
        public boolean test(Student student) {
            return student.getGrade() > 90;
        }
    }));

����Ƿ�����ʹ�÷�ʽ����ͨ��һ��ָ���ĺ���ʽ�ӿڣ���ָ��������ӳ�䵽һ���������͡����ԣ�����������飬ֻ����������Ľ��ֻ�����֣�Ҫôtrue��Ҫôfalse����Ȼ�������ڷ��飬��Ҳ������partitioningBy()�����ĵڶ�����������ָ��һ���ռ����������Ϳ��ԶԷ�����������к����Ĳ����ˡ�

## �ܽ᣺

���Ͼ���Java8�е����ĳ������÷�������ֻ���о���һЩ�����ġ�Java8 API���ṩ��һЩ��ͷ������ص���Ȼ�Ǹ�������е���Ƶ�ԭ����ҪäĿ���䡣ѧϰ��ʱ����JDKԴ����У����������Ķ���ʹ����˽����������ԭ����󣬲��ò�˵���ǣ�ʹ�������ȷʵ�ܼ������š�
 
��ش��룺