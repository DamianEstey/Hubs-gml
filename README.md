# Hubs, here is how it works:

```
//Put this in the create event of your controller.
masterHub = new Hub(reader_capacity: 64);

// You should initialize the Hub this way; if you do not it'll be buggy. The default value is the initial signal it sends to all readers currently reading from hub.
masterHub.Ready(defaultsignal: 0);
```

Now that the hub is instantiated and initialized; you'll want to create HubReaders!

```
slave = new HubReader(hub: masterHub, name: "slave");
```

Now a transmission can happen between master, and slave! The data being sent can be anything; I always use a format like:

```
{ context: context.foo, data: "hello world!" } 
```

The transmission:

```
//Hub sends data.
masterHub.PassTo("slave", somedata );

//Hubreader receives data.
var input = slave.ReadInput();

//Oh yeah, slave can send feedback as well.
slave.GiveFeedback(somedata);

//And the hub will read the feedback.
var feedback = masterHub.ReadFrom("slave");
```
