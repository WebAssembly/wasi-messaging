# `wasi-messaging`

A proposed [WebAssembly System Interface](https://github.com/WebAssembly/WASI) API.

## Table of Contents

- [`wasi-messaging`](#wasi-messaging)
  - [Table of Contents](#table-of-contents)
  - [Current Phase](#current-phase)
  - [Champions](#champions)
  - [Phase 4 Advancement Criteria](#phase-4-advancement-criteria)
  - [Introduction](#introduction)
  - [Goals](#goals)
  - [Portability criteria](#portability-criteria)
  - [Dev notes](#dev-notes)

## Current Phase

`wasi-messaging` is currently in [Phase
1](https://github.com/WebAssembly/WASI/blob/main/Proposals.md#phase-1---feature-proposal-cg).

## Champions

- [Dan Chiarlone](https://github.com/danbugs)
- [David Justice](https://github.com/devigned)
- [Jiaxiao Zhou](https://github.com/Mossaka)

## Phase 4 Advancement Criteria

For `wasi-messaging` to advance to Phase 4, it must have at least two independent implementations 
for open-source message brokers (such as Kafka, NATS, MQTT brokers) and two for cloud service providers 
(e.g., Azure Service Bus, AWS SQS).

## Introduction

In modern software systems, different components or applications often need to communicate with each other 
to exchange information and coordinate their actions. Messaging systems facilitate this communication by
allowing messages to be sent and received between different parts of a system.

However, implementing message-based communication can be challenging. It requires dealing with the details
of message brokers, such as connection management, channel setup, and message serialization. This complexity
can hinder development and lead to inconsistent implementations.

The `wasi-messaging` interface is a purposefully small interface focused purely on message passing. It is
designed to express the bare minimum of receiving a message and sending a message, along with an optional
request/reply interface that allows for message-based APIs and/or RPC.

By providing a standard way to interact with message brokers, the `wasi-messaging` interface aims to simplify
this process, hiding the underlying complexity from the user. This aligns with the broader goals of WASI by
promoting interoperability, modularity, and security in WebAssembly applications.

## Goals

The primary goal of this interface is to focus on message passing. The only guarantee offered is 
that publishing a message is handled successfully. No other guarantees are made about the delivery 
of the message or being able to ack/nack a message directly. This minimalist approach provides the 
most basic foundation of messaging, which can be expanded upon by future interfaces or proposals as 
use cases emerge.

This simplicity allows:
- **Ease of Integration**: Components can easily implement the message handler in this interface, 
with details such as work dispatch and queuing handled behind the scenes, invisible to the business logic.
- **Flexibility**: Anything that can send a message can easily be passed into a queue 
(such as a Kafka stream or NATS JetStream) without the knowledge that it is being sent into a queue.
- **Extensibility**: The paradigm can be expanded by future interfaces (like a queue-based work interface) to handle 
more complex messaging scenarios. By focusing solely on message passing, the wasi-messaging interface simplifies the 
development of interoperable WebAssembly modules that can communicate over various messaging systems without being 
tied to any specific implementation.

## Portability criteria

The main portability criteria on which this should be judged is whether a component can receive and send a message 
from all major messaging systems. This includes:
- Message standards like MQTT and AMQP.
- Specific technologies like NATS and Kafka.
- Cloud provider implementations like Azure Service Bus and AWS SQS.

This _does not_ mean it implements the full set of features of each of the messaging systems. In fact, it is expected 
that most implementations will need to do work to adapt their system to this interface (e.g., in Kafka, you'd have 
to mark the message as completed once the call to handle returns).

As mentioned above, this should still be completely compatible with any more advanced use cases of the various 
message systems. For example, if you have a queue of work that is currently being handled by pre-existing software
outside of Wasm components, a component could use this interface to publish messages that get ingested into this queue.

Another way to state the portability criteria is that this implementation should not break the possibility of a 
component consuming this interface to be integrated with a more advanced messaging use case.

## Dev notes

To regenerate the `.md` files, run: 
```sh
wit-bindgen markdown ./wit/ -w imports --html-in-md
wit-bindgen markdown ./wit/ -w imports-request-reply --html-in-md
wit-bindgen markdown ./wit/ -w messaging-core --html-in-md
wit-bindgen markdown ./wit/ -w messaging-request-reply --html-in-md
```

It would make sense for a lot of these functions to be asynchronous, but that is not currently natively supported in 
the component model. Asynchronous support will be added as part of WASI Preview 3. When async support becomes 
available, we plan to update the wasi-messaging interface to incorporate asynchronous patterns.

> **Note**: Ensure you have version 0.34.0 of `wit-bindgen` installed to avoid compatibility issues.
