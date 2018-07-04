# Shiro

## 1�����

Apache��Դ����֤����Ȩ����ҵ�Ự������ȫ���ܵȵĿ�ܡ������Spring Security��Shiro�������漸�����ƣ�
1).ʹ���������Ӽ򵥡���2).��������Springʹ�ã���Spring Security��������Spring��3).Ȩ�޹�������ȱȽϴ֣����Ը��Ի����ơ�

### 1.1 ����ܹ�

![Shiro������ܹ�](res/shiro_framework.png)

������Shiro������ܹ����������һ��Subject��ʾ��ǰ���û���Security Manager��Shiro�û�����ĺ��ġ�Cryptogaphy��Ҫ���������ݽ��м��ܡ�
��Security Manager���֣�Authenticator�������û���֤��Authorizer��������Ȩ����Session Manager�������Ự������ͨ��Session DAO��ȡ�Ự��Ϣ��
Cache Manager����������������������û�����ݺ���֤��Ϣ�ȡ�
�����Realmsһ��������һ���м����������ڴ�����Դ�л�ȡ��Ȩ��Ϣ�ȡ�

### 1.2 �򵥵�ʾ��

��������ʹ��һ���򵥵Ĳ��Գ�����Shiro�еĸ���ģ���һЩ���á�

���ȣ�������Ҫ�һ��������ܣ�����������Ҫ��������������һ���������е�Ԫ���ԣ�һ������Shiro�ĺ��İ���

```
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-core</artifactId>
        <version>1.4.0</version>
    </dependency>
```

Ȼ�����Ǵ������µĵ�Ԫ���ԣ�

```
public class AuthenticationTest {

    private SimpleAccountRealm realm = new SimpleAccountRealm();

    @Before
    public void prepare() {
        realm.addAccount("WngShhng", "123456");
    }

    @Test
    public void testAuthentication() {
        DefaultSecurityManager securityManager = new DefaultSecurityManager();
        securityManager.setRealm(realm);
        SecurityUtils.setSecurityManager(securityManager);
        Subject subject = SecurityUtils.getSubject();
        AuthenticationToken authenticationToken = new UsernamePasswordToken("WngShhng", "123456");
        subject.login(authenticationToken);
        System.out.println("Is authenticated" + subject.isAuthenticated());
    }
}
```

���������ȴ�����һ��Realm���󣬲��ڵ�Ԫ���Է���������֮ǰ���������һЩ�˺���Ϣ��
Ȼ�����ǻ�ȡ��һ��`SecurityManager`ʵ���������涨���Realmע�뵽���С�
Ȼ�����ǽ����涨���`SecurityManager`ͨ��`SecurityUtils`��`setSecurityManager`����ע�롣
֮������ͨ��������ϵ�`SecurityUtils`��ȡһ��`Subject`����
֮������ʹ���û������봴��һ��Token�������������`Subject`��`login`�������Խ��е�¼��
���յĵ�¼��Ϣ�洢��subject�У����ǿ���ͨ�����е�һЩ��������ȡ�����������`isAuthenticated()`���������ж��û��Ƿ���֤�ɹ��ġ�


