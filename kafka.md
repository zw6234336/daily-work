2018年2月25日 11:50:30

使用spring.cloud.stream 包装的kafka。

在生产者中引入自己的接口类（在接口中）



```

public interface Barista {
	@Input("orders")
	SubscribableChannel orders();

	@Output("hotDrinks")
	MessageChannel hotDrinks();

	@Output("coldDrinks")
	MessageChannel coldDrinks();
}
```



@EnableBinding\(Barista.class\)

public class TimerSource {

