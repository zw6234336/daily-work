insetr规范

使用userGenerateKeys=true 使用数据库内部生成的id返回给字段 keyProperty="id" 作为新增语句返回值

多接口参数

```
List<SysRole> selectRolesByUserIdAndRoleAble(Long userId,Integer enable)
```

1. 禁止使用：使用javaBean方式不是最好的，而是一种偷懒行为减低代码阅读难度
2. 不推荐方式：多参数是使用map手动包装。画蛇添足完全没有必要
3. 不推荐方式：多参数接口中不变，xml中使用\#{userId} \#{ebable} 这是错误用法。正确用法是\#{0} 或者\#{param1}
4. 


