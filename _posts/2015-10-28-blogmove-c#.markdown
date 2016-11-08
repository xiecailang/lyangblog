---
layout:     post
title:      "[博客搬家]C#串口通讯（变参、委托、线程安全队列）"
subtitle:   " \"from csdn\""
date:       2015-10-28 12:00:00
author:     "cailang.X"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2015/move-post-bg.jpg?x-oss-process=image"
catalog: true
tags:
    - move
---

# 需求

C#的串口通讯，参考了以前做的Android项目所用的设计模式：

![这里写图片描述](http://img.blog.csdn.net/20151028180017967)

父类DataTransport设计成单例模式，程序始终保持只有一种通讯链路，DataTransport中的方法都由子类实现。
由于对Ｃ＃不是很熟练，调试过程中遇到了较多问题，最终通过努力找到了解决方法，稍微总结一下。

# 变参函数

串口、蓝牙和Wifi的设置参数不同，它们各自的setConfig函数的形参也不同，如果设计不同的方法，父类就不能统一继承了。使用C#的__arglist来解决这个问题。

DataTransport.cs
```csharp
public virtual void setConfig(__arglist) { }
```
Usart.cs
```csharp
public override void setConfig(__arglist)
{
    ArgIterator args = new ArgIterator(__arglist);
    if (args.GetRemainingCount() > 0)
    {                
        port.PortName = TypedReference.ToObject(args.GetNextArg()).ToString();
        port.BaudRate = Convert.ToInt32(TypedReference.ToObject(args.GetNextArg()));
        port.DataBits = Convert.ToInt32(TypedReference.ToObject(args.GetNextArg()));
        port.StopBits = (StopBits)TypedReference.ToObject(args.GetNextArg());
        port.Parity = (Parity)TypedReference.ToObject(args.GetNextArg());
    }         
}
```

# 串口数据的收发和解析

串口之间的通讯需要读写并发，读取port中的数据并解析这个过程使用了线程安全的Queue作为缓存，避免另一端发送数据太快导致数据不能及时解析而丢失

![这里写图片描述](http://img.blog.csdn.net/20151028180041199)

在使用Java编写这一段逻辑的时候，使用了线程安全的ArrayBlockingQueue，然而C#并没有提供此类的API，可以使用Queue.Synchronized来保证多线程访问Queue同步


```java
DQueue = Queue.Synchronized(new Queue());
```

数据接收可以用线程不断的读取串口，也可以用事件触发的方式，我采用了后者：


```java
port.DataReceived += Port_DataReceived;
private void Port_DataReceived(object sender, SerialDataReceivedEventArgs e)
{
    try {
        byte[] readBuffer = new byte[port.BytesToRead];
        int count = port.Read(readBuffer, 0, readBuffer.Length);

        for (int i = 0; i < count; i++)
        {
            DQueue.Enqueue(readBuffer[i]);
        }               
    } catch(Exception ex) {
        throw ex;
    }
}
```

解析用线程实现，循环判断Queue的大小，Dequeue元素：


```java
try
{
    b = (byte)DataTransport.getInstance().DQueue.Dequeue();
}
catch (Exception e)
{
    throw e;
}
```

# 数据广播（分发）到前台UI更新

数据解析完成后要在界面上实时更新，也可能需要发送给另外的业务逻辑模块进一步处理，在Android平台使用Handler来实现：


```csharp
switch(command)
{
case CommandHelper.COMMAND_RESET:
	if(ShareValues.msgHandler != null)
	{
		Message msg = ShareValues.msgHandler.obtainMessage();
		msg.what = CommandHelper.COMMAND_RESET;
		ShareValues.msgHandler.sendMessage(msg);
	}		
	break;
case CommandHelper.COMMAND_REALTIMEDATA:
	RealTimeData rt = CommUtils.getRealTimeData(read_one);
	if(ShareValues.msgHandler != null)
	{
		Message msg = ShareValues.msgHandler.obtainMessage();
		msg.what = ShareValues.MSG_SUC;
		msg.obj = rt;
		ShareValues.msgHandler.sendMessage(msg);
	}							
	break;
case CommandHelper.COMMAND_ENCODER:
	if(ShareValues.msgHandler != null)
	{
		Message msg = ShareValues.msgHandler.obtainMessage();
		msg.what = CommandHelper.COMMAND_ENCODER;
		msg.obj = CommUtils.byteArrayToInt(read_one, 4, 1);
		msg.arg1 = CommUtils.byteArrayToInt(read_one, 5, 2);
		msg.arg2 = CommUtils.byteArrayToInt(read_one, 7, 2);
		ShareValues.msgHandler.sendMessage(msg);
	}																				
	break;
case CommandHelper.COMMAND_BATTERY_ID:/

	break;
case CommandHelper.COMMAND_RELOAD_WIFI:

default:break;
}
```
	C#中需要用到EventHandler和delegate来实现：

```csharp

class MessageEventArgs : EventArgs {
 private byte[] src;
 public byte[] Src
 {
     get{return src;}set
     {src = value;}
 }
}
public delegate void changeEventHandler(object sender , EventArgs args);
public event changeEventHandler changed;
protected virtual void onChanged(EventArgs args)
{
    if (this.changed != null)
        this.changed(this, args);
}
MessageEventArgs args = new MessageEventArgs();
args.Src = read_one;
onChanged(args);
```

数据接收事件触发后处理函数：
```csharp
private void textChanged(object sender,EventArgs args) {
    MessageEventArgs mArgs = (MessageEventArgs)args;
    receiveLen += mArgs.DataLen;
    txt_msg.Dispatcher.Invoke(new Action(delegate { txt_msg.Text = "收到数据："+receiveLen+"   发送数据："+sendLen; }));
}
...csharp
AnalysisData adata = new AnalysisData();
adata.changed += textChanged;
```

上一段代码中Dispatcher.Invoke是将接收和发送的数据显示到TextBox中，前台用的WPF，UI的更新必须在UI线程，如果直接在这里更新会报异常。
	最终效果：

![这里写图片描述](http://img.blog.csdn.net/20151028180102167)


调试工具用到了虚拟串口VSPD,SSCOM串口调试助手。


——cailang.X 2016-11
