# MessagingKit

A C# message server 

#### Classes
- MKMessageServer

#### Interfaces
- IMKMessageServerDelegate

#### Enums
- MKMessageServerStatus

```csharp
//implement the IMKMessageServerDelegate methods

public void messageServerDidStart(MKMessageServer server, MKMessageServerStatus status){
	//server was successfully started
}

void messageServerDidFailedToStart(MKMessageServer server, MKMessageServerStatus status){
	//server could not start
}
void messageServerDidReceive(MKMessageServer server, string message){
	//server received a packet
}

void messageServerDidStop(MKMessageServer server, MKMessageServerStatus status){
	//server was stopped
}

//create the message server
MKMessageServer server = new MKMessageServer("127.0.0.1",1809);

//set the delegate object
server.MessageServerDelegate = this;

//start the message server
server.Start();
```