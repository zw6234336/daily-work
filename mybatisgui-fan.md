### insetr规范

使用userGenerateKeys=true 使用数据库内部生成的id返回给字段 keyProperty="id" 作为新增语句返回值

### 多接口参数

```
List<SysRole> selectRolesByUserIdAndRoleAble(Long userId,Integer enable)
```

1. 禁止使用：使用javaBean方式不是最好的，而是一种偷懒行为减低代码阅读难度
2. 不推荐方式：多参数是使用map手动包装。画蛇添足完全没有必要
3. 不推荐方式：多参数接口中不变，xml中使用\#{userId} \#{ebable} 这是错误用法。正确用法是\#{0} 或者\#{param1}
4. 推荐写法：使用注解方式简单明了，xml中直接使用\#{userId} \#{ebable}。升级方式@Param中参数也可以使javaBean。在xml中使用.的方式获取参数

```
List<SysRole> selectRolesByUserIdAndRoleAble(@Param（"userId"）Long userId,@Param(enable) Integer enable)
```

### 动态SQl

```
<if test="userName !=null and userName !=''">
```

test是一个OGNL表达式这个表达式的结果可以是 true false,初次之外所有的非0值都是true.只有0是false。

choose 相当于if else

where 帮我们做了一些sql语句拼写错误自动整理的功能、set同样功能

id in\(\) 使用${ids}不安全的可能发生sql注入。一定要使用\#{}。foreach最终会全部转换成list来处理

```
List<SysUser> selectByIdList(List<Long> idList)
<foreach collection="List" open="(" close=")" separator="'" item="id" index="i"> 
    #{id}
```

collection 必填，值是要迭代循环的属性名，属性值可能有很多。

iteam 变量名 从迭代对象中去除的每一个值

index 索引的属性名，当循环对象是Map时 这个值为Map的key键值

foreach也可以用于批量插入操作

![](/assets/import.png)

### 链表复杂查询

只是记个写法规则方便以后大家使用

1. 多model联合查询

```
private SysRole role;
private String userName;
private String userEmail;
```

![](/assets/import1.png)

注意别名的写法。当然上面的写法也可以使用resultMap来写，并且resultMap是支持继承的可以自定义resultMap继承基础resultMap简化写法

### 缓存

非常完整的测试说明

[https://tech.meituan.com/mybatis\_cache.html](https://tech.meituan.com/mybatis_cache.html)

一级缓存默认开启

```
SysUser model = dao.selectById(1);
model.setName("zhangsan");
SysUser model2 = dao.selectById(1);
model2.getName();值也为zhangsan
```

通过日志可以看到当前两次数据库查询只是执行了一次。在同一个sqlsession生命周期内，Mybatis会把方法和参数统计过计算存入Map中。当方法和参数完全相同时通过计算就会得到缓存中Map的值。所以必须禁止在原对象进行修改操作。insert、update、delete会更新一级缓存。

二级缓存的生命周是整个sqlsessionFactory，比一级缓存的生命周期更长。所以起的作用也会更大。二级缓存所有的select都会被缓存，insert、update、delete、同样清空缓存

