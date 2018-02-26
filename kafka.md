2018年2月25日 11:50:30

//参考文档

[https://docs.spring.io/spring-cloud-stream/docs/Brooklyn.RELEASE/reference/htmlsingle/\#\_configuration\_options](https://docs.spring.io/spring-cloud-stream/docs/Brooklyn.RELEASE/reference/htmlsingle/#_configuration_options)

使用spring.cloud.stream 包装的kafka。

在生产者中引入自己的接口类（在接口中）

```
//引入自己的接口类定义kafka中的topic
public interface Barista {
    @Input("orders")
    SubscribableChannel orders();

    @Output("hotDrinks")
    MessageChannel hotDrinks();

    @Output("coldDrinks")
    MessageChannel coldDrinks();
}
```

```
//在生产中引入接口类。发送到指定topic中。实现动态确定topic
@EnableBinding(Barista.class)
public class TimerSource {
      @InboundChannelAdapter(value = "hotDrinks", poller = @Poller(fixedDelay = "1000", maxMessagesPerPoll = "1"))
    public Map<String,String> timerMessageSourceTest() {
```

消费者组一定要定义。这个概念好像在应用中整体定义在最外面就行 这些应用属于这个topic的组

---

## 消费者组概念

通过分享topic的方式更方便的连接应用，我们可以在同一组中创建多个应用实例，但是每个实例都是竞争关系，一个消息只能被一个实例处理。

spring cloud stream使用了组的概念（同kafka中组）。每个消费者都可绑定一个组通过spring.cloud.stream.bindings.&lt;channelName&gt;.group。

所有订阅了topic的组都会得到一份发布数据的副本，但是只有组中一个订阅成员默认接收topic。

