## NSDSocket.IO

使用的他人已开源项目，方便集成使用，仅针对v0.9版本及其以下的Socket.IO

* [Origin Code](https://github.com/francoisp/socket.IO-objc)

## 使用方法

### 直接拷贝
1.  将github工程中的想要用到的功能对应文件夹下的所有文件复制到您的工程中。
2.  将与文件夹同名的头文件放入到您的pch文件中，或者在需要使用界面布局的源代码位置。

### CocoaPods安装

如果您还没有安装cocoapods则请先执行如下命令：
```
$ gem install cocoapods
```

为了用CocoaPods整合NSDExtendTool到您的Xcode工程, 请建立如下的Podfile:

```
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '11.0'

pod 'NSDSocket.IO'
```
   
然后运行如下命令:

```
$ pod install
```

## Usage

The easiest way to connect to your Socket.IO / node.js server is

``` objective-c
SocketIO *socketIO = [[SocketIO alloc] initWithDelegate:self];
[socketIO connectToHost:@"localhost" onPort:3000];
```

If required, additional parameters can be included in the handshake by adding an `NSDictionary` to the `withParams` option:

``` objective-c
[socketIO connectToHost:@"localhost"
onPort:3000
withParams:[NSDictionary dictionaryWithObjectsAndKeys:@"1234", @"auth_token", nil]
];
```

A namespace can also be defined in the connection details:

``` objective-c
[socketIO connectToHost:@"localhost" onPort:3000 withParams:nil withNamespace:@"/users"];
```

There are different methods to send data to the server

``` objective-c
- (void) sendMessage:(NSString *)data;
- (void) sendMessage:(NSString *)data withAcknowledge:(SocketIOCallback)function;
- (void) sendJSON:(NSDictionary *)data;
- (void) sendJSON:(NSDictionary *)data withAcknowledge:(SocketIOCallback)function;
- (void) sendEvent:(NSString *)eventName withData:(NSDictionary *)data;
- (void) sendEvent:(NSString *)eventName withData:(NSDictionary *)data andAcknowledge:(SocketIOCallback)function;
```

So you could send a normal Message like this

``` objective-c
[socketIO sendMessage:@"hello world"];
```

or an Event (including some data) like this

``` objective-c
NSMutableDictionary *dict = [NSMutableDictionary dictionary];
[dict setObject:@"test1" forKey:@"key1"];
[dict setObject:@"test2" forKey:@"key2"];

[socketIO sendEvent:@"welcome" withData:dict];
```

If you want the server to acknowledge your Message/Event you would also pass a SocketIOCallback block

``` objective-c
SocketIOCallback cb = ^(id argsData) {
NSDictionary *response = argsData;
// do something with response
};
[socketIO sendEvent:@"welcomeAck" withData:dict andAcknowledge:cb];
```

All delegate methods are optional - you could implement the following

``` objective-c
- (void) socketIODidConnect:(SocketIO *)socket;
- (void) socketIODidDisconnect:(SocketIO *)socket disconnectedWithError:(NSError *)error;
- (void) socketIO:(SocketIO *)socket didReceiveMessage:(SocketIOPacket *)packet;
- (void) socketIO:(SocketIO *)socket didReceiveJSON:(SocketIOPacket *)packet;
- (void) socketIO:(SocketIO *)socket didReceiveEvent:(SocketIOPacket *)packet;
- (void) socketIO:(SocketIO *)socket didSendMessage:(SocketIOPacket *)packet;
- (void) socketIO:(SocketIO *)socket onError:(NSError *)error;
```

To process an incoming `message` or `event` just

``` 
// message delegate
- (void) socketIO:(SocketIO *)socket didReceiveMessage:(SocketIOPacket *)packet
{
NSLog(@"didReceiveMessage >>> data: %@", packet.data);
}

// event delegate
- (void) socketIO:(SocketIO *)socket didReceiveEvent:(SocketIOPacket *)packet
{
NSLog(@"didReceiveEvent >>> data: %@", packet.data);
} 
```

## 版本历史
具体请查看 **[CHANGELOG.md](CHANGELOG.md)**
