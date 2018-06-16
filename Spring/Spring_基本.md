# Spring����

## 1��Spring���

### 1.1 Spring����Ҫ���ܣ�

1. �������ǹ������֮���������ϵ�����������ڣ�
2. �ṩ��ͨ����־��¼������ͳ�ơ���ȫ���ơ��쳣��������������������
3. �������ǹ������ݿ����񣬱����ṩ��һ�׼򵥵�JDBC����ʵ�֣�������������������ݷ��ʿ�ܼ��ɣ�
4. �ṩһ���Լ���web����Spring MVC��
5. �ṩճ�����������Ϳ�ܵ������������������������Ŀ����ϡ�

### 1.2 Spring�ܹ�

![Spring�ܹ�ͼ](res/spring_framework.JPG)

### 1.3 Spring����Ӧ��

![Spring����Ӧ��ͼ](res/spring_usage.JPG)

## 2�������

ʹ��IDEA������Maven-WebApp��Ȼ�����ǽ������µ����ã�

### 2.1 Java 8 ����������

��Ҫ��pom.xml��build��ǩ�м������µ����ã�

    <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
    </configuration>
    </plugin>

�������֮�����ǾͿ����ڴ�����ֱ��ʹ��Java8�����б���ˡ�

### 2.2 Spring ��������

�������һ���Ե���Spring������Ҫ�Ļ��������ã�����������IOC������ݣ�ͬʱҲ����Test��Aop��ģ�飺

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
    </dependency>

ע��һ����������û��ֱ����������������Ҫ�İ汾�ţ�����ͳһ����properties��ǩ�У�

    <properties>
        <spring.version>4.3.10.RELEASE</spring.version>
    </properties>

���ˣ��������ǵ�Spring���������Ѿ����ϣ���������������ʹ��һ������������������һ�����ǵĻ����Ƿ��ɹ���

### 2.3 �������ԣ�һ���򵥵�ע�������

�򵥽���һ�£��������Ƕ�����һ���ӿ�`IHelloApi`��Ȼ����һ������ʵ����`HelloApiImpl`��

    public interface IHelloApi {
        void sayHelloWorld();
    }

	public class HelloApiImpl implements IHelloApi {
        @Override
        public void sayHelloWorld() {
            System.out.println("Hello world!");
        }
    }
	
Ȼ��������`main`Ŀ¼���洴��һ����Ϊ`resources`��Ŀ¼����ʹ������Ҽ�������ѡ��`Mark as`->`Resources Root`��
Ȼ���ڸ�Ŀ¼�д���һ����Ϊ`HelloWorld.xml`���ļ��������ļ����������ǵ�Bean��

    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
        <!--�����calss���Ե�ֵ��������Ҫ���õ�ʵ���ľ���λ��-->
        <bean id="hello" class="me.shouheng.spring.hello.beanimp.HelloApiImpl"/>
    </beans>

Ȼ�����Ǵ���һ��������`HelloWorldTest`������������`ClassPathXmlApplicationContext`����ȡһ�������ģ�������������ȡBeanʵ����ʹ�ã�

    public class HelloWorldTest {
        public static void main(String...args) {
            ApplicationContext context = new ClassPathXmlApplicationContext("HelloWorld.xml");
            IHelloApi helloApi = (IHelloApi) context.getBean("hello");
            helloApi.sayHelloWorld();
        }
    }

���ݣ����Ƕ����Beanʵ�����������ȷ�����`Hello world!`����ô���ǵĻ��������Ǵ�ɹ��ˡ�
