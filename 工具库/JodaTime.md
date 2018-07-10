# ���õ�Java��������JodaTime

Java8֮ǰ��ʱ����д���һЩ��Ʋ��õĵط��������������ǳ��ز����㣬�����׳������磬Ҫʵ����ָ�������ڵĻ�����������ָ����ʱ��Ĳ���������ҪЩ������������룻�������·ݴ�0��ʼ�����в����ͻ������С����ԣ�ͨ������ʹ�õ�������Joda Time������ʱ����صĲ�����

## 1��ʹ��JodaTime

JodaTime��Github�������ҳ��[JodaTime](https://github.com/JodaOrg/joda-time)

ʹ��JodaTime��ʱ����������÷�ʽ��

��Maven�У�

    <dependency>
      <groupId>joda-time</groupId>
      <artifactId>joda-time</artifactId>
      <version>2.9.9</version>
    </dependency>

��Gradle�У�

    compile 'joda-time:joda-time:2.9.9'

## 2����ȡDateTimeʵ��

��ʹ��JodaTime��ʱ��������Ҫ��ȡһ��DateTimeʵ����Ȼ������������������������ʵ��ǿ��Ĺ��ܡ���Ҫ��ȡһ��DateTimeʵ�������кܶ��ַ�ʽ�������г������ļ��ַ�ʽ��

��ʽ1��ʹ��ϵͳʱ�乹��DateTimeʵ��

    DateTime dateTime = new DateTime();

��ʽ2��ʹ�þ����ʱ�乹��DateTimeʵ�����÷�����������ذ汾

    DateTime dateTime1 = new DateTime(
    2000, // year
    1,    // month
    1,    // day
    0,    // hour (midnight is zero)
    0,    // minute
    0,    // second
    0     // milliseconds
    );

��ʽ3��ʹ��Calendar����DateTimeʵ��

    DateTime dateTime2 = new DateTime(Calendar.getInstance());

��ʽ4��ʹ������DateTimeʵ������DateTimeʵ��

    DateTime dateTime3 = new DateTime(dateTime);

��ʽ5��ʹ���ַ�������DateTimeʵ��

    DateTime dateTime4 = new DateTime("2006-01-26T13:30:00-06:00");
    DateTime dateTime5 = new DateTime("2006-01-26");

## 3��ʹ��DateTime�ķ���

DateTime�������ķ������������ǽ����õķ����ֳ����ࡣһ�����ڷ����з���DateTime�����֣�һ�����ڷ����з���Property���͵����֡���Ȼ����������ּ������������Ļ�������Ҫ����Property��ʵ�������ˡ�

��������ȸ���DateTime�еĵ�һ�෽����

    // ָ����ʱ�䵥λ��������ָ����ֵ
    DateTime dateTime0 = dateTime.plusDays(1);
    System.out.println(dateTime0);

    // ָ����ʱ�䵥λ�������ָ����ֵ
    DateTime dateTime6 = dateTime.minusDays(1);
    System.out.println(dateTime6);

    // �����������ڻ�����ֱ��ָ������ָ��ʱ�䵥λ�����ֵ
    DateTime dateTime7 = dateTime.withYear(2020);
    System.out.println(dateTime7);

    // ����ָ���ĸ�ʽ�������
    System.out.println(dateTime.toString("E MM/dd/yyyy HH:mm:ss.SSS"));

������Ĵ����У�����ֻ���������е�һ���ַ�����ʵ����ʵ���ϣ���DateTime�ڲ������ķ�����ֻ�����ǵ�ԭ��������ơ�

�����һЩ����������漰��ʱ�䷢���˱仯��������ָʱ���Ӧ�ĺ����������˱仯�����ͻ����DateTimeʵ����withMillis()�������ڸ÷����У�������ִ���ĺ������뵱ǰ�ĺ�������һ���ͻ��½�һ��DateTimeʵ���������䷵�ء����ԣ������plusDays(1)��minusDays(1)���ص�DateTimeʵ�����Ѿ�����һ��ʵ���ˡ�

## 4��ʹ��Property��

����ͨ��DateTimeʵ����millisOfDay() dayOfYear() minuteOfDay()��һЩ�з������Ի�ȡ����DateTime��һ��Propertyʵ����Ȼ�����ͨ������Property�ķ����ٻ�ȡһ��DateTimeʵ����Ҳ����˵��ʵ���ϵ���DateTime�ķ�����ȡPropertyʵ����Ϊ�˶�ָ����ʱ��λ�õ���Ϣ�����޸ġ����磬�ԡ��ա������޸ģ��ԡ��ꡱ�����޸ĵȵȡ��޸���֮����Ҫ��ȡһ��DateTimeʵ����Ȼ���ټ������к����Ĳ�����

ʵ����ÿ�ε���DateTime�ķ�����ȡPropertyʵ����ʱ�򣬶��Ὣ��ǰ��DateTime��Ϊ�������롣Ȼ�󵱵�����ָ���ķ���֮���ֻ����DateTimeʵ����withMillis()�����ж�ʱ���Ƿ����仯����������˱仯�ʹ���һ����ʵ�������ء�

����������һЩʾ����

    // ��������dayOfMonth��ȡһ��Propertyʵ����Ȼ���������withMaximumValue����
    // ���ĺ�����ָ�����ڵ��������ڲ��䣬�·ݱ������֮�󷵻�һ��DateTime��������������2018��5��1�գ�������2018��5��31�գ�
    // �꣬�£����λ�ò��䣬�ձ�ɸ������ġ�
    DateTime dateTime0 = dateTime.dayOfMonth().withMaximumValue();
    DateTime dateTime1 = dateTime.dayOfMonth().withMinimumValue();
		
## 5�������ľ�̬����
		
���������һЩ��֮�⣬JodaTime�������ľ�̬����������ʹ�á����磺

    System.out.println(Days.daysBetween(dateTime1, dateTime).getDays());
    System.out.println(Months.monthsBetween(dateTime1, dateTime).getMonths());
    System.out.println(Years.yearsBetween(dateTime1, dateTime).getYears());

��Ȼ����������ֻ�г��˶�����DateTimeʵ���ġ��ա� ���¡��͡��ꡱ��λ�Ĳ���������������Ƶ�����������ԡ����롱���롱�Ȳ�����

## ����

����ֻ��ͨ��JodaTime��һЩ���õķ�����ʵ����˵������ƵĻ���ԭ���ص������������е��߼�������ÿ���������Ĳ�����������ʲô��

��ش��룺[Java-advanced](https://github.com/Shouheng88/Java-advanced)