## Problem 1

Cannot create PoolableConnectionFactory (Could not create connection to database server.)
 
MySQL version 8.0.11 
 
Update MySQL Connector to : 
 
         <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.11</version>
        </dependency>
 
## Problem 2

Caused by: java.sql.SQLException: Access denied for user 'shouh'@'localhost' (using password: YES) 

    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="${jdbc.driverClassName}" />
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="root" />
        <property name="password" value="${password}" />
    </bean>
	
֮ǰʹ��ռλ�����û���д�������ļ����棬���Ե�ʱ����ע���ֵ���Ǵ���ģ�������ռλ��������ֱ��д��bean�����������û�������ˡ�

MYSQL������ʱ����Ҫ�ֶ�����`MySQL -uroot -password`���У�����ᱨ��:`Access denied for user 'ODBC'@'localhost' (using password: NO)`

## Problem 3

�������Գ����쳣��The server time zone value '?D1��������?����??' is unrecognized or represents more than one time zone.

�������沽��ִ�У�

![](res/time_zone_err.png)

�ο���

[mysql���б�The server time zone value '?D1��������?����??' is unrecognized or represents more than one time zone�Ľ������](https://www.cnblogs.com/ljy-20180122/p/9157912.html)

