
## Requirements

* java 6.0+ 


## License

mtmq-framework is available under the Apache license, see the LICENSE file for more information.

## 使用指南

### 1.消息入队列
获取消息队列全局对象MessageTrunk（可以用spring注入），put入消息即可。

```
        // 获取MessageTrunk实例
        MessageTrunk mt = (MessageTrunk) SpringBeanUtils.getBean("messageTrunk");

        Message<DemoMessage> message = new Message<DemoMessage>(MessageType.DEMO_MESSAGE, new DemoMessage(value));
        // 消息入MT
        mt.put(message);
```

### 2. 处理消息
消息处理器：继承AbstarctMessageHandler抽象类。

```
	public class DemoHandler extends AbstarctMessageHandler<DemoMessage>
{
	private static Log logger = LogFactory.getLog(DemoHandler.class);

	public DemoHandler()
	{
		// 说明该handler监控的消息类型
		super(MessageType.DEMO_MESSAGE);
	}

	/**
	 * 监听到消息后处理方法
	 */
	@Override
	public void handle(DemoMessage message)
	{
        // do handle
	}

	@Override
	public void handleFailed(DemoMessage obj)
	{
		// handle failed
	}

}
```

## 实现原理
基本原理是redis的阻塞取命令： Blpop，该命令移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。

## 性能测试（[测试链接][1]）
测试环境：本人开发机器，4核i5，16GB内存，tomcat和redis在同一台机器
测试结果：最高写入消息速度为57208 ops 