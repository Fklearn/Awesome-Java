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

## 2��Shiro��֤ ��Ȩ���Զ���Realm

���������Ѿ���ʶ����Shiro��Realm��һ�ּ򵥵�ʹ�÷�ʽ`SimpleAccountRealm`��ʵ������������಻ͬ��Realm���Թ�����ʹ�á�
ͨ������Ĵ��룬����Ӧ�ö�Realm��һ��������ӡ�󣬼����Ͻ��ؽ���Realm����������ָ��������Դ�л�ȡ�û���Ȩ�ޡ���ɫ����ص���Ϣ�ġ�
����������Դ���Ƿ�װ��һЩ����ָ�����͵�����Դ�ķ������������ǿ�����Shiro�и��ֲ�ͬ���͵�Realm��ʹ�÷�ʽ��

### 2.1 IniRealm

IniRealm������`ini`���͵��ļ��м����û�����Ϣ����������resources�ļ������洴��һ����Ϊ`users.ini`���ļ������м������漸�д��룺

```
[users]
WngShhng=123456,admin
[roles]
admin=user:delete
```

�������ǰ���IniRealm��Ҫ��ĸ�ʽ������һЩ�û�����Ϣ��Ȼ�����ǾͿ���ʹ�ø��ļ��д洢����Ϣ���е�Ԫ���ԣ�

```
public class IniRealmTest {

    @Test
    public void testWithIniFiles() {
        IniRealm iniRealm = new IniRealm("classpath:users.ini");

        DefaultSecurityManager securityManager = new DefaultSecurityManager();
        securityManager.setRealm(iniRealm);

        SecurityUtils.setSecurityManager(securityManager);
        Subject subject = SecurityUtils.getSubject();

        AuthenticationToken authenticationToken = new UsernamePasswordToken("WngShhng", "123456");
        // У���û��Ƿ��Ѿ���¼���Ƿ���ڸ��˺ŵļ�¼��
        subject.login(authenticationToken);
        System.out.println("Is authenticated" + subject.isAuthenticated());
        // У���û���ɫ
        subject.checkRole("admin");
        subject.checkPermission("user:delete");
        subject.checkPermission("user:update");
    }
}
```

��������ǿ�������Ĵ����1.2�еĴ���󲿷ֶ�һ����ֻ������Realm���֣������Ǵ�ini�ļ��ж�ȡ���û���Ϣ��
�����û��������Ϣ���������ǻ��������û��Ľ�ɫ��Ȩ�޵���Ϣ��У�顣������ļ�����������û���Ϣ���Բ鿴��ص��ĵ���

### 2.2 JdbcRealm

JdbcRealm���ǽ�����Դ���ó������ݿ⣬��������ʹ��MySQL��Ϊ����Դ�������ڳ����������µ�����:

1.���ȣ�����Ҫʹ��MySQL������������ʹ�ð����Druid���������ӳأ�

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.11</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.10</version>
</dependency>
```

Ȼ�����ǽ������µĲ��ԣ�

```
public class JdbcRealmTest {

    private DruidDataSource druidDataSource = new DruidDataSource();

    {
        druidDataSource.setUrl("jdbc:mysql://localhost:3306/shiro_test?serverTimezone=GMT%2B8");
        druidDataSource.setUsername("root");
        druidDataSource.setPassword("xxxxxx");
    }

    @Test
    public void testJdbcRealm() {
        // �鿴JdbcRealm�Ĵ������涨����һЩSQL����ʵ�����Ͼ���ͨ����ЩSQL����ѯ�û���Ϣ��
        JdbcRealm jdbcRealm = new JdbcRealm();
        jdbcRealm.setDataSource(druidDataSource);

        DefaultSecurityManager securityManager = new DefaultSecurityManager();
        securityManager.setRealm(jdbcRealm);

        SecurityUtils.setSecurityManager(securityManager);
        Subject subject = SecurityUtils.getSubject();

        UsernamePasswordToken token = new UsernamePasswordToken("WngShhng", "123456");
        subject.login(token);
    }
}

```

������Ĵ�����Կ�������������÷�ʽ������Ļ���һ����ֻ���������ǽ�Realm������Դ�л�����MySQL���ݿ⡣

��ִ������ĵ�Ԫ����֮ǰ�����ǻ���Ҫ�����ݿ������һ�ű�

```
create table if not exists users (
    username varchar(255),
	password varchar(255)
);
```

Ȼ������������ݿ��в���һ�����ݣ�

```
insert into users("WngShhng", "123456");
```

��������ִ�е�Ԫ���ԾͿ���ͨ���ˡ�

���Ǳ���û��дʲôSQL���ڽ���У���ʱ����Щ�߼�������װ����JdbcRealm���棬����Խ���JdbcRealm�鿴����Ĵ��룺

```
    protected static final String DEFAULT_AUTHENTICATION_QUERY = "select password from users where username = ?";
    protected static final String DEFAULT_SALTED_AUTHENTICATION_QUERY = "select password, password_salt from users where username = ?";
    protected static final String DEFAULT_USER_ROLES_QUERY = "select role_name from user_roles where username = ?";
    protected static final String DEFAULT_PERMISSIONS_QUERY = "select permission from roles_permissions where role_name = ?";
	......
```

���ԣ������ǲ�ȥ����SQL�ȵ���Ϣ��ʱ��Ĭ�ϻ�ȥִ�������SQL�������Ǵ������ʱ���SQLҲ�Ǵ�����ļ���SQL���ܽ�����ġ�

���Ҫʹ��Ȩ�޵���Ϣ��Ҫ����JdbcRealm�Ŀ���Ϊ��״̬��

```
jdbcRealm.setPermissionsLookupEnabled(true)
```

��������ʹ�õ�ShiroΪ�����ṩ��Ĭ�ϵ�SQL���������ݲ�ѯ�ģ�����Ҳ�����Զ���һЩSQL�����в�ѯ������

```
jdbcRealm.setUserRolesQuery("select ....");
```

JdbcRealm���л���������Ƶķ����ɹ�����ʹ�ã����ǿ��Խ���ЩSQLע�뵽JdbcRealm���У�Ȼ��Shiro�ͻ�ʹ������ָ����SQL�������ݲ�ѯ��

### 2.3 �Զ���Realm

����ʹ��ShiroΪ�����ṩ��Realm�����ǻ������Զ���Realm�����ǿ���ʹ���Զ���Realm�����������Ŀ�ܣ�����ʹ��һЩ��������Ϊ����Դ�ȣ�
�⼫���������������������Դ������ԡ���������ͨ��һ���򵥵���������ʾһ��Shiro���Զ���Realm�����ݡ�

ͨ���鿴JdbcRealm�Ĵ��룬����֪�����Ǽ̳���AuthorizingRealm���������Զ���Realm��ʱ������Ҳ�Ӹ��������չ��

```
public class CustomRealm extends AuthorizingRealm {

    // ����ģ���û���Ϣ����Դ
    private Map<String, String> maps = new HashMap<>();

    {
        maps.put("WngShhng", "123456");
        super.setName("customRealm");
    }

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        String userName = (String) authenticationToken.getPrincipal();
        String password = getPasswordByUyUserName(userName);

        // ͨ������װ֮���AuthenticationInfo���أ����ڼ���û�����֤��Ϣ
        return new SimpleAuthenticationInfo(userName, password, "customRealm");
    }

    private String getPasswordByUyUserName(String userName) {
        return maps.get(userName);
    }

    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        String userName = (String) principalCollection.getPrimaryPrincipal();

        // ��������ģ���ָ������Դ�л�ȡָ���û��Ľ�ɫ��Ȩ����Ϣ
        Set<String> roles = getRolesByUserName(userName);
        Set<String> permissions = getPermissionByUserName(userName);

        // ��Ȩ��Ϣ������װ֮���AuthorizationInfo���أ��ڲ������˸��û���Ȩ�޺ͽ�ɫ��Ϣ
        SimpleAuthorizationInfo authorizationInfo = new SimpleAuthorizationInfo();
        authorizationInfo.setRoles(roles);
        authorizationInfo.setStringPermissions(permissions);

        return authorizationInfo;
    }

    // ģ���ȡָ���û��Ľ�ɫ
    private Set<String> getRolesByUserName(String userName) {
        Set<String> roles = new HashSet<>();
        roles.add("Admin");
        roles.add("User");
        return roles;
    }

    // ģ���ȡָ���û���Ȩ��
    private Set<String> getPermissionByUserName(String userName) {
        Set<String> permissions = new HashSet<>();
        permissions.add("do-1");
        permissions.add("do-2");
        permissions.add("do-3");
        return permissions;
    }
}
```

AuthorizingRealm���������������ò�����⣬������������ָ�����û�������ȡ���û������롢��ɫ��Ȩ�޵ȣ�Ȼ�󽫻�ȡ���Ľ����װ��һ�������з��ء�
���Shiro��������Ƿ��صĶ������жϸ��û��Ƿ����ָ���Ľ�ɫ��Ȩ�޵���Ϣ��

��������Ķ��壬���ǽ������µĵ�Ԫ���ԣ�

```
public class CustomRealmTest {

    @Test
    public void testCustomRealm() {
        CustomRealm customRealm = new CustomRealm();

        DefaultSecurityManager securityManager = new DefaultSecurityManager();
        securityManager.setRealm(customRealm);

        SecurityUtils.setSecurityManager(securityManager);
        Subject subject = SecurityUtils.getSubject();

        AuthenticationToken authenticationToken = new UsernamePasswordToken("WngShhng", "123456");
        // У���û��Ƿ��Ѿ���¼���Ƿ���ڸ��˺ŵļ�¼��
        subject.login(authenticationToken);
        System.out.println("Is authenticated" + subject.isAuthenticated());
        // У���û���ɫ
        subject.checkRoles("Admin", "User");

        subject.checkPermissions("do-1", "do-2");
    }
}
```

������Ĵ������ǿ��Կ������Զ���Realm��ʱ����Ҫ�Ǹ��ݴ�����û���Ϣ�ڸ�д�����������н��в�ѯ��������Ȩ����֤��Ϣ��
�ؼ���һ�����ڴ�ĳ������Դ��ȡ�û�����Ϣ�����ԣ�ͨ���Զ���Realm�����ǿ��Ժܷ����ʹ�ø�������Դ��Shiro���ɡ�



