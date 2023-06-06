# Examples

## Example 1: Guest Skeleton Usage

The guest will implement the methods in [`wit/guest.wit`](/wit/guest.wit):
- `configure()`, which will be called by the host to setup long-lived subscriptions, and receive any other implementor specific extensions/config..
- `handler()`, which will be called by the host to handle messages sent to the channels subscribed to in `configure()`.

As Wasm modules/components are mostly intended to be short-lived, the host is meant to create new instances for configuration and each message handling.

### Rust

```rust
use crate::messaging_types::{GuestConfigurationResult, MessageResult};

wit_bindgen::generate!({
    path: "../wit",
});

struct MyGuest;

impl guest::Guest for MyGuest {
    fn configure() -> Result<GuestConfigurationResult, u32> {
        // Configure the messaging system
    }

    fn handler(_ms: Vec<MessageResult>) -> Result<(), u32> {
        // Handle the message
    }
}

export_messaging!(MyGuest);
```

### C/C++

```c
#include "messaging.c"

messaging_result_guest_configuration_error_t messaging_configure(void) {
    // Configure the messaging system
}

messaging_result_void_error_t messaging_handle(messaging_list_message_t *message) {
    // Handle the message
}
```

### Go

```go
package main

import (
	msging "app/gen"
)

func init() {
	a := MsgingImpl{}
	msging.SetExportsWasiMessagingMessagingGuest(a)
}

type MsgingImpl struct {
}

func (e MessagingImpl) Configure msging.Result[msging.WasiMessagingConsumerGuestConfiguration, msging.WasiMessagingMessagingGuestError] {
	// Configure the messaging system
}

func (e MessagingImpl) Handler msging.Result[struct{}, msging.WasiMessagingMessagingGuestError] {
	// Handle the message
}

func main() {}
```

## Example 2: Guest Usage

### Rust

```rust
use crate::messaging_types::{GuestConfigurationResult, MessageResult, connect, disconnect};

wit_bindgen::generate!({
    path: "../wit",
});

struct MyGuest;

impl guest::Guest for MyGuest {
    fn configure() -> Result<GuestConfigurationResult, u32> {
        // This function will be called by the host, who will be maintaining a 
        // long-lived client connection to a broker or other messaging system.
        // The client will be subscribed to channels a, b, and c) with no extra configuration.
        // As soon as configuration is set, the host should kill the Wasm instance.
        Ok(GuestConfigurationResult {
            channels: vec!["a".to_string(), "b".to_string(), "c".to_string()],
            ..Default::default()
        })
    }

    fn handler(ms: Vec<MessageResult>) -> Result<(), u32> {
        // Whenever a message is received on a subscribed channel (from configure()),
        // the host will call this function. Once the message has been handled,
        // the host is expected to kill the Wasm instance.
        
        for m in ms {

            // match on message metadata for channel name
            match m.metadata {
                Some(metadata) => {
                    for (k, v) in metadata {
                        if k == "channel" {
                            match v.as_str() {
                                "a" => {
                                    // handle message from channel a
                                    // [...]

                                    // unsubscribe from channel a
                                    update_guest_configuration(GuestConfigurationResult {
                                        channels: vec!["b".to_string(), "c".to_string()],
                                        ..Default::default()
                                    }).unwrap()

                                    // abandon message
                                    consumer::abandon_message(m).unwrap();
                                }
                                "b" => {
                                    // handle message from channel b
                                    // [...]

                                    // request-reply from channel d
                                    let client = connect("some-broker").unwrap();
                                    let msgs = subscribe_try_receive(client, "d", 100).unwrap();

                                    // do something with msgs
                                    // [...]

                                    // disconnect client
                                    disconnect(client);

                                    // complete message
                                    consumer::complete_message(m).unwrap();
                                }
                                "c" => {
                                    // handle message from channel c
                                    // [...]

                                    // send message to channel d
                                    let client = connect("some-broker").unwrap();
                                    let message = MessageParam {
                                        data: "hello from guest".as_bytes(),
                                        format: messaging_types::FormatSpec::Raw,
                                        metadata: None,
                                    };
                                    producer::send(client, "d", &[message]).unwrap();
                                    disconnect(client);

                                    // complete message
                                    consumer::complete_message(m).unwrap();
                                }
                                _ => {
                                    // handle message from unknown channel
                                }
                            }
                        }
                    }
                }
                None => {
                    // handle message with no metadata
                }
            }
        }
    }
}

export_messaging!(MyGuest);
```