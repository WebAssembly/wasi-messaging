# `wasi-messaging` API

## Interfaces

```wit
/// An interface grouping all necessary types for messaging.
interface "wasi:messaging/types" {
    /// An event type that follows the CloudEvents specification (https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md). We
    /// assume the type of the data is a byte sequence. It is up to the data schema to determine what type of the data payload the event 
    /// contains.
    record event {
		specversion: string,
		ty: string,
		source: string,
		id: string,
		data: option<list<u8>>,
		datacontenttype: option<string>,
		dataschema: option<string>,
		subject: option<string>,
		time: option<string>,	
		extensions: option<list<tuple<string, string>>>
    }
    
    /// Channels specify where a published message should land. There are two types of channels:
    /// - queue: competitive consumers, and
    /// - topic: non-competitive consumers.
    variant channel {
		queue(string),
		topic(string)
    }
    
    /// An error type
    type error = string
}

/// An interface for a producer.
interface "wasi:messaging/pub" { 
	/// Publishes an event to a channel.
	publish: func(c: channel, e: event) -> result<_, error>
}

/// An interface for a generic consumer.
interface "wasi:messaging/sub" {
	/// Subscribes to a channel.
  	subscribe: func(c: channel) -> result<_, error>

	/// Unsubscribes from a channel.
  	unsubscribe: func(c: channel) -> result<_, error>
}

/// An interface for a consumer relying on push-based message delivery.
interface "wasi:messaging/push" {
	/// Creates an on-receive handler push-based message delivery.
  	on-receive: func(e: event) -> result<_, error>
}

/// An interface for a consumer relying on basic pull-based message delivery.
interface "wasi:messaging/basic/pull" {
	/// Pulls a message.
  	receive: func(time-to-live-in-milliseconds: u32) -> result<event, error>
}

/// An interface for a consumer relying on pull-based message delivery via streaming.
interface "wasi:messaging/stream/pull" {
	/// Pulls a stream of messages.
  	stream-receive: func() -> result<stream<event>, error> 
}

// ...
```

## World Examples

```wit
// ...

/// Two worlds that connect "sub" together "basic/pull"/"stream/pull", as they are intrinsically linked.
world "wasi:messaging/basic/sub-pull" {
	import sub: "wasi:messaging/basic/sub"
	
	export pull: "wasi:messaging/basic/pull"
}

world "wasi:messaging/stream/sub-pull" {
	import sub: "wasi:messaging/basic/sub"
	
	export stream-pull: "wasi:messaging/stream/pull"
}

/// Two typical usage examples.
world "wasi:messaging/pull-pubsub" {
    import pub: "wasi:messaging/pub"
    import sub-pull: "wasi:messaging/basic/sub-pull" // or "wasi:messaging/stream/sub-pull"
  
    export handle-receive: "wasi:http/handler"
    export handle-subscribe: "wasi:http/handler"
    export handle-publish: "wasi:http/handler"
}

world "wasi:messaging/sub-pull" {
	import pub: "wasi:messaging/pub"
	
	export on-receive: "wasi:messaging/push"
}
```

