# First Chapter

2017年12月1日 21:11:09    积分活动设计开发

发起积分事件不再本系统，当前项目并没有使用消息组件，所以只能自己模拟一个消息组件，队列化处理积分消息（好处是防止了多线程计算总积分时的错误。能够降速按照懒掌柜的速度去处理），特殊情况必须加锁停止积分队列计算积分。

---

2017年12月6日 21:11:43

想看一下 nginx 上使用lua脚本过滤数据。主要是redis过滤

2017年12月7日 16:24:38

nginx的请求可以划分为主请求和子请求。一个主请求可以划分成多个子请求。这个意义相当大 可以把一个很大的计算任务分别打到很多服务器上进行快速计算充分利用资源

nginx 指令执行顺序可以理解为类似maven的生命周期，虽然这些指令的书写顺序可能发生变化，但是他们存在的生命周期并不会发生改变。所以他们的执行顺序和书写书写顺序其实是没有关系的。只有在相同的阶段（生命周期内才会与书写顺序有关）。

我们可以通过查看该指令的文档 查找当前指令所处的具体阶段 phase :content

nginx的重新启动并不会使nginx主进程关闭（未验证）。

2017年12月12日 16:32:08

nginx 生命周期post-read、server-rewrite、findconfig、rewrite、post-rewrite、preaccess、access、post-access、try-files、 content 以及 log

post-read：执行的是解析请求头。非常有用的模块是ngx\_realip 防止ip攻击。在前端中先把真实ip保存到请求头中。在后端的时候再去解析校验

server-rewrite：简单的认为在server中配置的都是在server-rewrite阶段执行的

find-config：（类似java中的dispatcher）匹配对应的location块。这个是ngix核心完成的









