1.参数介绍

2.查询时使用

3.添加时使用

4.修改时使用

实例代码

```xml
useGeneratedKeys="true" keyProperty="data.id" keyColumn="id"  
<insert id="batchInsertData" useGeneratedKeys="true" keyColumn="id" keyProperty="id">
        INSERT INTO t_table (
        column1,
        column2,
        column3
        ) VALUES
        <foreach item="data" collection="list" separator=",">
            (
            #{data.column1},
            #{data.column2},
            #{data.column3}
            )
        </foreach>
</insert>
```

5.删除时使用