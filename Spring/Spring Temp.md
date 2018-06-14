
����ʹ�ù��췽���򴴽���ʵ��������ֵ��������ͨ��`setter`������SpringҪ�󷽷�������ȷʵ����setter������Ҫ�󣩶�ʵ�������Խ������á�
�����������÷�ʽ��ʹ�ù��췽����ͬС�죬�������ڣ�
1).������Ҫ��ָ��������ȷʵ�ṩ��Setter������2).Ȼ�����ǿ�����bean��ǩ��ʹ��`property`�ӱ�ǩ���������Ե�ֵ���������÷�ʽ��`constructor-arg`�ǳ����ơ�

�������Ƕ�����������DiBean��RefBean�����е�DiBean��һ��`message`�ֶΣ���������ʹ��`property`Ϊ��ע��ֵ`So what?`��
��RefBean����һ������ΪDiBean���ֶ�`diBean`���������Ƕ���һ��Bean��ͨ��`property`��`ref`����ָ�������õ�Bean��

    <bean id="di" class="me.shouheng.spring.di.DiBean">
    <property name="message" value="So what?"/>
    </bean>

    <bean id="refBean" class="me.shouheng.spring.di.RefBean">
    <property name="diBean" ref="di"/>
    </bean>

����ע��һЩ�ַ���������������Bean��`property`��ǩ��Ϊ�����ṩ�����������ӻ��ڵĹ��ܣ����ǿ���ʹ����������ɸ��Ӷ�������ע�롣

Ϊ�˲�����ע����������ʵ�������������Ƕ�����һ����ΪDiMulti���ࡣ����һ����Ϊ`names`��`List<String>`���͵Ĳ�����Ȼ�����ǿ�����������������ע��ֵ��

    <bean id="diMulti" class="me.shouheng.spring.di.DiMulti">
    <property name="names">
        <list>
        <value>Chen</value>
        <value>Li</value>
        <value>Xu</value>
        </list>
    </property>
    </bean>

���������IDEA��ʹ�õĻ�����ô��������Զ������������͸���������ˣ���`property`��ǩ��������������ӱ�ǩ��
���ǿ���ͨ��`property`��ǩ��`name`����ָ��Ҫע����ֶε����ƣ�Ȼ����ʹ��`property`���ӱ�ǩ����װ������Ҫע��ĸ������͵�ֵ��

�����ʹ��p�����ռ�����setterע�룬���������`refBean`����ʹ�����µķ�ʽ���м򻯣�

    <bean id="refBean" class="me.shouheng.spring.di.RefBean" p:diBean-ref="di"/>

ע��������Ҫ����p�����ռ䣺

    xmlns:p="http://www.springframework.org/schema/p"

���ǿ����Ĺ�����ʵ����`p:�ֶ���="ֵ"`�������ֶ����������ͣ���ô����`p:�ֶ���-ref=��Bean��id��`


ѭ��������

����A����Ҫʹ��B��B��Ҫ�õ�C����C��Ҫ�õ�A������ע��ķ�ʽ��ͬ�ֳַɣ�������ѭ��������setter����ѭ��������

������ѭ��������ʾBean�ڴ�����ʱ��������������Bean����ɵ�ѭ������������ѭ�������޽⣬ֻ��ͨ���׳�BeanCurrentlyInCreationException�쳣��ʾѭ��������

����setter����ѭ�������ֳַ��������Σ������������ѭ��������prototype�������ѭ��������ǰ���ǿ��Խ���ģ��������޷������
��Ϊ�����������Bean�ڴ���֮��ᱻ����Bean�������д��ã���prototype�������Bean���ᱻSpring�������棬�޷���ǰ��¶������ϵ�Bean��

Bean��������

1. singleton�����������򣬱�ʾÿ��Spring Ioc������ֻ�����һ��ʵ������������������������ȫ��Spring��������
Spring���������û��ָ��������Ĭ�Ͼ��ǵ����ģ����õķ�ʽҲ�ܼ򵥾���ͨ����bean��ǩ��ʹ��`scope`����ָ��ֵΪ`singleton`��
���Bean���ӳٳ�ʼ���ģ���ô��Bean�����״�ʹ�õ�ʱ�򴴽������ڵ���������С�
2. prototype��ԭ�ͣ�ָÿ����Spring���������ȡBean������һ��ȫ�µ�Bean������ڡ�singleton����˵���ǲ�����Bean��ÿ�ζ���һ������Bean���崴����ȫ��Bean��









`property`��ǩ�п�ѡ���ӱ�ǩ��

1. value
2. list
3. bean
4. ref
5. array
6. desciption
7. idref
7. map
8. null
9. meta
10. props
11. set

��������DiMulti������һ���µ��ֶ�`Map<String, Integer> grades`��Ȼ�����ǿ�����XML�а�������ķ�ʽ��Ϊ�丳ֵ��

    <property name="grades">
        <map key-type="java.lang.String" value-type="java.lang.Integer">
        <entry key="Wang" value="100"/>
        <entry key="Li" value="99"/>
        <entry key="Sun" value="98"/>
        </map>
    </property>

���Ͼ���Map���͵�ע�뷽ʽ����ΪMap�ļ�ֵ�Կ������κε��Զ������ͣ����Զ�Ӧ������`key`��`value`�Ļ���`key-ref`��`value-ref`��

�������͵�ע�뷽ʽ�����ǾͲ��ټ�����ˡ������漸�����������÷�ʽ�����Ǵ��¿��Եó�Springע��ʱ���õĻ������߼���

	
	
beans��ǩ�п�ѡ���ӱ�ǩ��

1. `bean`��
2. `alias`��
3. `desciption`��
4. `import`��

bean��ǩ�п���ѡ������ԣ�

1. `id`��
2. `abstract`��
3. `autowire`��
4. `autowire-candidate`��
5. `class`��
6. `depends-on`��
7. `destroy-method`��
8. `factory-bean`��
9. `factory-method`��
10. `init-method`��
11. `lazy-init`��
12. `name`��
13. `parent`��
14. `primary`��
15. `scope`��

bean��ǩ�ڿ�ѡ���ӱ�ǩ��

1. `constructor-arg`
2. `description`
3. `lookup-method`
4. `meta`
5. `property`
6. `qualifier`
7. `replaced-method`



