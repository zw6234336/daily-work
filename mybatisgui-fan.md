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
<foreach collection="List" open="(" close=")" separator="'" item="id" index="id"> 
    #{id}
```



