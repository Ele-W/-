序列化：

```text
String jsonString = JSON.toJSONString(obj);
```

反序列化：

```text
VO vo = JSON.parseObject("...", VO.class);
```



```
JSONObject data = json.getJSONObject("data");
```



```
JSONArray data = json.getJSONArray("data");
```



```
data.getString("name")
```

https://zhuanlan.zhihu.com/p/72495484
