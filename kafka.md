2018年2月25日 11:50:30

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
//在生产中引入接口类
@EnableBinding(Barista.class)
public class TimerSource {
      @InboundChannelAdapter(value = "hotDrinks", poller = @Poller(fixedDelay = "1000", maxMessagesPerPoll = "1"))
    public Map<String,String> timerMessageSourceTest() {
```



