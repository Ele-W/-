

```mysql
str_to_date(createtime, '%Y-%m-%d') BETWEEN str_to_date('2018-08-01', '%Y-%m-%d') AND str_to_date('2018-08-31', '%Y-%m-%d')
```



```mysql
update CS_BASE 
SET CREATE_TIME = TO_DATE('2018-10-23 16:26:12', 'YYYY-MM-DD HH24:MI:SS') 
where UIDPK=1838
```

字符串转换成日期： `str_to_date(str,format)`
日期转换成字符串：`date_format(date,format)`
时间转换成字符串：`time_format(time,format)`

1.mysql日期和字符相互转换方法 
date_format(date,’%Y-%m-%d’) ————–>oracle中的to_char(); 
str_to_date(date,’%Y-%m-%d’) ————–>oracle中的to_date();

%Y：代表4位的年份 
%y：代表2为的年份

%m：代表月, 格式为(01……12) 
%c：代表月, 格式为(1……12)

%d：代表月份中的天数,格式为(00……31) 
%e：代表月份中的天数, 格式为(0……31)

%H：代表小时,格式为(00……23) 
%k：代表 小时,格式为(0……23) 
%h： 代表小时,格式为(01……12) 
%I： 代表小时,格式为(01……12) 
%l ：代表小时,格式为(1……12)

%i： 代表分钟, 格式为(00……59) 【只有这一个代表分钟，大写的I 不代表分钟代表小时】

%r：代表 时间,格式为12 小时(hh:mm:ss [AP]M) 
%T：代表 时间,格式为24 小时(hh:mm:ss)

%S：代表 秒,格式为(00……59) 
%s：代表 秒,格式为(00……59)
