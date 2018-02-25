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

消费者组一定要定义（这个能不能动态的根据接收消息方法定义）

