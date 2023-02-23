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
    variant messaging-error {
        payload-too-large(string),
        queue-or-topic-not-found(string),
        insufficient-permissions(string),
        service-unavailable(string),
        delivery-failed(string),
        connection-lost(string),
        unsupported-message-format(string),
        unexpected-error(string),
    }

	// A subscription token that allows receives from a specific subscription
	type subscription-token = string
}

/// An interface for a producer.
interface "wasi:messaging/pub" { 
	/// Publishes an event to a channel.
	publish: func(c: channel, e: event) -> result<_, error>
}

/// An interface for a generic consumer.
interface "wasi:messaging/sub" {
	/// Subscribes to a channel.
  	subscribe: func(c: channel) -> result<subscription-token, error>

	/// Unsubscribes from a channel.
  	unsubscribe: func(st: subscription-token) -> result<_, error>
}

/// An interface for a consumer relying on push-based message delivery.
interface "wasi:messaging/handler" {
	/// Creates an on-receive handler push-based message delivery.
  	on-receive: func(e: event) -> result<_, error>
}

// ...
```

## World

```wit
world "wasi:messaging/service" {
	import pub: "wasi:messaging/pub"
	import sub: "wasi:messaging/sub"
	export message-handler: "wasi:messaging/handler"
}
```

