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

所有订阅了topic的组都会得到一份发布数据的副本，但是只有组中一个订阅成员默认接收topic。如果不明确组名。spring cloud会自动给每个消费者命名一个组。（所以还是必须手动命名，防止重复消费）

### MIME TYPE

spring cloud提供一下几种转换类型支持。简单来说就是发布者定义的消息类型（直接才java pojo的形式是不行的，类型中会记录对象全路径，消费者不能解析这样的消息类型），可以采用json形式方便两端解析（性能方面还没做测试那种最好）。

```
    //消息类型是由生产者方法的返回值决定的
    @InboundChannelAdapter(value =FHSocketChannel.FH_CMS_CHANNEL, poller = @Poller(fixedDelay = "1000", maxMessagesPerPoll = "1"))
    public String timerMessageSourceTest() {
        ChatMessage messageModel = new ChatMessage();
        messageModel.setContents("测试内容");
        messageModel.setFrom("懒掌柜");
        messageModel.setTo("2017");
        messageModel.setTime(new Date().getTime());
        String aa = JSON.toJSONString(messageModel);
        return aa;
    }
```

## Declaring and Binding Channels（声明 绑定channel）

spring cloud stram 通过@EnableBinding 注解绑定触发。只需要在spring boot中加入 注解即可使用 spring.cloud.stream。@EnableBinding 注解中已经包含@Configuration 等注解。表示channel的接口方法可以作为参数放在@EnableBinding中。注意在生产者中一个channel只能定义一个@Output\(channel名称\)注解，不能再次定义@Input\(channel名称\)

```
public interface FHSocketChannel {
    String FH_CMS_CHANNEL="fhCmsChannel";
    @Output(FH_CMS_CHANNEL)
    MessageChannelfhCmsChannelOutput();
}

@InboundChannelAdapter(value =FHSocketChannel.FH_CMS_CHANNEL, 
poller = @Poller(fixedDelay = "1000", maxMessagesPerPoll = "1"))
public String timerMessageSourceTest() {
        ChatMessage messageModel = new ChatMessage();
        messageModel.setContents("测试内容");
        messageModel.setFrom("懒掌柜");
        messageModel.setTo("2017");
        messageModel.setTime(new Date().getTime());
        String aa = JSON.toJSONString(messageModel);
        return aa;
    }
```



