## String转成JSON格式

```java
//定义一个json格式的字符串



String message = "{"code":1,"data":{"id":1001,"name":"lisi"}}";



//将字符串转换成json



JSONObject jsonObject = JSONObject.parseObject(message);
```

## 在JSONObject中取出某一个值

```java
//定义一个json格式的字符串



String message = "{"code":1,"data":{"id":1001,"name":"lisi"}}";



//将字符串转换成json



JSONObject jsonObject = JSONObject.parseObject(message);



//取出data里的数据



String code = jsonObject.getString("code");



String data = jsonObject.getString("data");




System.out.println(code); //输出结果为：1



System.out.println(data); //输出结果为：{"name":"lisi","id":1001}
```

## 数组操作

```
JSONArray resultJa = resultJs.getJSONArray("result");
```