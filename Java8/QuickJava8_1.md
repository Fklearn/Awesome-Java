# Quick Java 8 ��һ��

## 1������

Java8�ĸĽ�����ʷ���κ�һ�θı䶼�Ƚ���Զ��Java�ڸĽ���ʵҲ�Ǳ��������̬�仯��ԭ�������������Ҫ�ڶ���������У���Java��ǰ�ǲ�֧�����ֲ����ġ�

��Java8֮ǰ�������Ҫ���ö����������ںˣ���Ҫʹ���̣߳�����Ҫ�����ӵ�ͬ���߼���������Java8�У�����Ժ����׵�ʹ�������Լ��Ĵ����ڶ���ں�����ִ�С�

���⣬����������������ԺͿ�Դ������ݣ�����Scala��Guava�ȡ������ܽ�һ��Java8����Ҫ�����Ľ���

1. ����ʽ��̺�Lambda���ʽ��
2. ��(Stream)��̣�
3. ʱ��API�ĸĽ���
4. Ĭ�Ϸ���

## 2����Ϊ������

����Ϊ������ָ����������Ϊ������������ָ��������Ϊ�������룬˵���˾���ָ**����ģʽ**����Java8ֻ��ʹ����Lambda���ʽ����������Ĵ��룬ʹ�����࿴�������Ӽ�ࡣ����������ϣ�������Ҫ���յĶ��������ࡣ

### 2.1 Lambda���ʽ�����﷨

    (parameters) -> expression     // ���ʽ
    (parameters) -> {statements;}  // ��䣬���β���ֺŵ���Ҫ�û�����������

������Lambda�Ļ����﷨����һ������Lambda��ʹ�ñ��ʽ��������ڶ�����ʽLambda��ʹ�����������ֻҪע��һ�������Ҫ�û��������������ɡ�

������һЩ�ӿڵĶ��壬�Լ�ʹ��Lambda���ʽ

    public static void main(String...args) {
	    // ��������
        ICreateObject createObject = Employee::new;
        IExpression expression = employees -> employees.get(0);
        // ���Խ�һ����Ϊ IExpression expression2 = List::isEmpty;
        IExpression expression2 = employees -> employees.isEmpty(); 
        IConsumeObject consumeObject = employee -> System.out.println(employee.name);
        IAdd add = (a, b) -> a + b;
        IAdd add1 = Java8LambdaExample::cal;
        // �ᱨ������Function�ӿ��쳣
        //        Object object = Employee::new;
    }

    private static int cal(int a, int b) {
        return a + b;
    }
	
	@FunctionalInterface
    public interface ICreateObject {
        Employee create();
		default void method() {}
    }

    public interface IExpression {
        void filter(List<Employee> employees);
    }

    public interface IConsumeObject {
        void consume(Employee employee);
    }

    public interface IAdd {
        int add(int a, int b);
    }

    private static class Employee {
        String name;
    }

�������ʾ�����룬���ǿ����ܽ��һЩ���ۣ�

1. ��ν�ĺ����ӿھ���ָֻ����һ����Ĭ�Ϸ����Ľӿڣ�������`@FunctionalInterface`ע�����ָ���Ľӿ��Ǻ����ӿڣ�
2. ���Lambda�е�`->`���������䣬���ҵ������ֻ��һ�е�ʱ�����ǿ��Խ�������ȥ����
3. ��Ҫ��Lambda���ʽ��ֵ��һ�������ʱ��������������`�����ӿ�`����ôIDEAs�����ʾ��
4. ��Ҫע�⺯��ʽ�ӿ��ǲ������׳��ܼ��쳣�ġ�

���������ܽ�һЩ�����ķ������õ�ʾ����

��������е�`Employee::new`������ν�ķ������ã������ǳ����ķ������õ����ӣ�

|���|Lambda|��Ч�ķ�������|
|:-:|:-:|:-:|
|1|`(Employee e)->e.getName()`|`Employee::getName`|
|2|`(String s) -> System.out.println(s)`|`System.out::println`|
|3|`(str, i) -> str.substring(i)`|`String::substring`|

���ԣ������ܽ����������ַ������õ�����

|���|Lambda|��Ч�ķ�������|
|:-:|:-:|:-:|
|1|`(����) -> ����.��̬����(����)`|`����::��̬����`|
|2|`(����1, ��������) -> ����1.ʵ������(��������)`|`����::ʵ������`|
|3|`(����) -> ���ʽ.ʵ������(����)`|`���ʽ::ʵ������`|

### 2.2 Java API �еĺ���ʽ�ӿ�

Java8��API��Ϊ�����ṩ�˼�������ʽ�ӿڣ���Щ�ӿ��б�Ҫ�˽�һ�¡���Ϊ�Դ�Java8��ʼ�ӿڿ��Զ���Ĭ�Ϸ����ˣ�������Щ�ӿ��������ṩ��һЩ����˼��Ĭ�Ϸ���������ܶ����Ǳ�̱Ƚ��а�����

    public interface Predicate<T> {
        boolean test(T t);
	}
	
    public interface Consumer<T> {
        void accept(T t);
    }

    public interface Function<T, R> {
        R apply(T t);
    }

��������������ӿڵĶ��塣��ֻҪ��������ֻ�з��ص�ֵ�ǲ�ͬ�ľͿ����ˡ���һ�������жϵģ���������ʵ�ֹ��˵�Ч�����ڶ���û�з������ͣ�ֻ�������Դ���Ĳ������д���������������ӳ��ģ�Ҳ����˵��������Ҫʵ�ֵ���Ϊ�Ĳ����ͷ����ǲ�ͬ�����͵�ʱ�����������

��Ϊ������ֵ���ͣ�Java��Ҫ�������װ��Ͳ���Ĳ�����������Ҫ�ɱ��ġ����ԣ���������������ӿڣ������Ľӿ�Ҳ�ǣ���Java8���ṩ�˲���Ҫװ��İ汾��Ҳ���Ǵӷ��ͱ������ֵ���Ͷ��ѡ���IntPredicateΪ���ɣ�

    public interface IntPredicate {
        boolean test(int value);
    }

### 2.3 ����Lambda���ʽ
	
Java8���ṩ��һЩ�ӿڻ��ǿ��Ը��ϲ����ġ�ʹ�ø��ϲ�������ʵ�ָ����ӵ��߼�����Щ���ϲ���ʱ��Ĭ�Ϸ�������ʽ����ģ�ÿ������ʽ�ӿ����в�ͬ�����ԣ���������ֻ�оٳ��������ڸ��ϵķ�������ʵ�ʵĿ��������У������ֱ�ӽ��뵽ָ���ĺ���ʽ�ӿ��в鿴��Щ�����Ķ��塣

#### 2.3.1 �Ƚ���Comparator<T>

������һ�������б�employees�����еĶ�����Employee������getName()��getAge()����������

    employees.sort(Comparator.comparing(Employee::getName));
    employees.sort(Comparator.comparing(Employee::getName).reversed().thenComparing(Employee::getAge));

��������д����У���һ��ʵ�ֶ�employees����getName()�Ľ���������򡣵ڶ��д����employees���Ȱ���getName()�Ľ����������Ȼ�󽫷��صĽ�������ٰ���getAge()�Ľ����������

#### 2.3.2 ν�ʸ��� negate()��or()��and()

    Predicate<Employee> employeePredicate = (employee -> employee.getAge() > 13)
	employeePredicate.negate()
    employeePredicate.and(employee -> employee.getAge() <= 15).or(employee -> "LiHua".equals(employee.getName()))

�������ȶ�����employeePredicate���������������ˡ��������13�Ĺ�Ա�������������negate()����������һ��Predicate�������������ˡ����С�ڵ���13�Ĺ�Ա����Ȼ�����ĸ��ϲ�����ʾ���������13����С��15�Ĺ�Ա��������ΪLiHua�Ĺ�Ա����ע�⣬�����and��or������˳���Ǵ������ҵģ�Ҳ����`a.or(b).and(c)`������`(a || b) and c`��
	
#### 2.3.3 ����Function<T>����
	
Function��andThen��compose����Ĭ�Ϸ��������Ƕ��᷵��һ��Functionʵ����

    Function<Integer, Integer> f = x -> x + 1;
    Function<Integer, Integer> g = x -> x * 2;
    Function<Integer, Integer> h1 = f.andThen(g); // h1(x) = g(f(x)) = (x + 1) * 2
    Function<Integer, Integer> h2 = f.compose(g); // h2(x) = f(g(x)) = (x * 2) + 1
    System.out.println(h1.apply(1));
    System.out.println(h2.apply(1));

������Function�ĸ��ϲ�����ʾ������ʵ����Ч�����൱����ѧ�еĸ��Ϻ�����������Ӧ��ע��һ������������ʵ�ʵĸ���Ч����
	
	