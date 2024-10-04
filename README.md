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
- [Taylor Thomas](https://github.com/thomastaylor312)

## Phase 4 Advancement Criteria

`wasi-messaging` should have at least two implementations for both open source message brokers (such
as Kafka, or NATS) and two for cloud service providers

## Introduction

The wasi-messaging interface is a purposefully small interface that is focused purely on _message
passing_. It is meant to express the bare minimum of receiving a message and sending a message along
with an optional request/reply interface that allows for message based APIs and/or RPC.

## Goals

The messaging interfaces aim to address the need for a standard way to handle message-based
communication in modern software systems. In complex software systems, different components or even
different applications need to communicate with each other to exchange information and coordinate
their actions.

However, implementing message-based communication can be challenging, as it requires dealing with
the details of message brokers, such as connection management, channel setup, and message
serialization. The messaging interfaces aim to simplify this process by providing a standard way to
interact with message brokers, hiding the underlying complexity from the user.

As mentioned in the introduction, the primary goal of this interface is to focus on message passing.
The only guarantee offered is that publishing a message is handled successfully. No other guarantees
are made about delivery of the message or being able to ack/nack a message directly. This is by far
the simplest representation of the complex problem of message-based communication. The idea behind
this is straightforward. If something can pass or receive a message, that paradigm can easily be
expanded upon by future interfaces (like a queue-based work interface). Also, many pieces of
business logic can implement something simple like the message handler in this interface and have
details such as work dispatch and queuing be backed behind that implementation, invisible to the
business logic (and/or shared with multiple business logic components). Conversely, anything that
can send a message can easily be passed into a queue (such as a Kafka stream or NATS JetStream)
without the knowledge that it is being sent into a queue. Essentially, this gives us the most basic
foundation of messaging which we can add more to, or propose additional interfaces as the use cases
emerge.

## Portability criteria

The main portability criteria on which this should be judged is whether a component can receive and
send a message from all major messaging systems. This includes anything implementing message
standards like MQTT and AMQP, specific technologies like NATS and Kafka, and cloud provider
implementations like Azure Service Bus and AWS SQS. This _does not_ mean it implements the full set
of features of each of the messaging systems. In fact, it is expected that most implementations will
need to do work to adapt their system to this interface (i.e., in Kafka, you'd have to mark the
message as completed once the call to `handle` returns). As mentioned above, this should still be
completely compatible with any more advanced use cases of the various message systems. For example,
if you have a queue of work that is currently being handled by a pre-existing piece of software
outside of Wasm components, a component could use this interface to publish messages that get
ingested into this queue. Another way to state the portability criteria is that this implementation
should not break the possibility of a component consuming this interface to be integrated in with a
more advanced messaging use case

## Dev notes

To regenerate the `.md` files, run: 
```sh
wit-bindgen markdown ./wit/ -w imports --html-in-md
wit-bindgen markdown ./wit/ -w imports-request-reply --html-in-md
wit-bindgen markdown ./wit/ -w messaging-core --html-in-md
wit-bindgen markdown ./wit/ -w messaging-request-reply --html-in-md
```

It would make sense for a lot of these functions to be async, but that is not currently natively supported in
the component model. Async support will be added as part of WASI Preview 3.