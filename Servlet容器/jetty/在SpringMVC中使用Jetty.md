# ��Spring MVC��ʹ��Jetty

## 1������Jetty

֮ǰ��������Tomcat��ʱ��û���漰Servlet���������ݡ�ʵ���ϣ������ǿ�����ʱ��ʹ�õ���Tomcat������һ���Ƚ�������Servlet������
����Tomcat��JettyҲ��һ��Servlet��������Ȼ��λ���ڲ���Tomcat��

Jetty��Tomcat�ļ�������

1. ���Tomcat��Jetty��������������Tomcat������ѭJavaServlet�淶֮�⣬������չ�˴���JEE������������ҵ��Ӧ�õ�����
����Tomcat�ǽ��������ģ��������ý�Jetty�ิ����ࡣ
�����ڴ�����ͨ������Ӧ�ö��ԣ�������Ҫ�õ�Tomcat�����߼����ԣ���������������£�ʹ��Tomcat�Ǻ��˷���Դ�ġ�
�������Ʒ��ڷֲ�ʽ�����£��������ԡ�
����Jetty��ÿ��Ӧ�÷�����ʡ���Ǽ����ڴ棬���ڴ�ķֲ�ʽ�������ǽ�ʡ������Դ��
���ң�Jetty��������Ҳʹ���ڴ���߲���ϸ��������ĳ������Եø����ٸ�Ч��
2. Jetty������������ɲ���ԺͿ���չ�ԣ������ڿ����߶�Jetty������ж��ο���������һ���ʺ����������Web Server��
���֮�£���������Tomcatԭ����֧�ֹ������ԣ�Ҫ��������ĳɱ�Զ���ڷḻJetty�ĳɱ������Լ�����⣬���������׼����ѡ�
3. Ȼ������֧�ִ��ģ��ҵ��Ӧ��ʱ��JettyҲ�����Ҫ��չ�����ⳡ����Tomcat���Ǹ��ŵġ�

## 2����װJetty

���ǿ��Ե���ַ��[���ص�ַ](https://www.eclipse.org/jetty/download.html)��ȡJetty�İ�װ�汾����������ѡ��zip��ʽ����������������ѹ��ָ����Ŀ¼���ɡ�

Ȼ�����ǽ��뵽Jetty��Ŀ¼���棬����������ִ����������

```
cd $JETTY_HOME
java -jar start.jar
```

���������Jetty����������û�в����κ�webӦ�ã����Ե�����ͨ��http://localhost:8080���ʵ�ʱ���õ�404�Ĵ���
��ʱ�����ǿ��Խ���jettyĿ¼�����`demo-base`Ŀ¼��ִ����������

```
cd demo-base
java -jar ../start.jar
```

�����������ٴδ�http://localhost:8080��ʱ��ͽ�����Jetty�Ļ�ӭҳ�档������Ѿ������˻�ӭҳ�棬��ô˵��JettyҲ�Ѿ���װ�ɹ��ˡ�

## 3��ʹ��Jetty

### 3.1 ����Jetty��Ŀ¼

�������ļ������洴��һ���µ��ļ���JettyDemo��Ȼ�����JettyDemoĿ¼��������������ִ�У�

```
java -jar D:\jetty\jetty-distribution-9.4.11.v20180605\start.jar --add-to-startd=http,deploy
```

�����`D:\jetty\jetty-distribution-9.4.11.v20180605\start.jar`���ҵĵ����е�Jetty�İ�װĿ¼��

�����ʹ�������Jetty�Ļ�Ŀ¼��������ʱ��ִ��`java -jar D:\jetty\jetty-distribution-9.4.11.v20180605\start.jar`��Ȼ�ῴ��404����Ϊ��Ŀ¼����ʱ��û����Ŀ��

### 3.2 �ı�Jetty�Ķ˿�

��Jetty��Ŀ¼��ִ��

```
java -jar D:\jetty\jetty-distribution-9.4.11.v20180605\start.jar jetty.http.port=8081
```

���������޸�ʹ�õĶ˿ڡ�����ʹ��������ָ�Ҳ���Ե�`start.ini`����`start.d/http.ini`�ļ��н����޸ġ�

### 3.3 ΪHTTPS & HTTP2����SSL

��Jetty��Ŀ¼��ִ��

```
java -jar D:\jetty\jetty-distribution-9.4.11.v20180605\start.jar --add-to-startd=https,http2
```

### 3.4 �޸�Jetty��HTTPS�˿�

��Jetty��Ŀ¼��ִ��

```
java -jar D:\jetty\jetty-distribution-9.4.11.v20180605\start.jar jetty.ssl.port=8444
```

## 4����Intellij��ʹ��Jetty

���ǿ�����Intellij��ֱ��ʹ��Jetty�������ǵ�SpringMVC��Ŀ����Maven�е����÷�ʽҲ�Ƚϼ򵥣�ֻҪ��`pom.xml`��`plugins`�м������µĲ�����ɣ�

```
<!--jetty���-->
<plugin>
     <groupId>org.eclipse.jetty</groupId>
     <artifactId>jetty-maven-plugin</artifactId>
     <version>9.4.11.v20180605</version>
     <configuration>
         <stopPort>9988</stopPort>
         <stopKey>foo</stopKey>
         <scanIntervalSeconds>5</scanIntervalSeconds>
         <webAppConfig>
              <contextPath>/</contextPath>
         </webAppConfig>
    </configuration>
</plugin>
```

�����������֮����Maven Project��ǩҳ�����Plugins����ͻ����һ��Jetty��Ŀ¼������ֱ��˫��`jetty:run`�����������ǵ���Ŀ
