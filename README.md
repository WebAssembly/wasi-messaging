# `wasi-messaging`

A proposed [WebAssembly System Interface](https://github.com/WebAssembly/WASI) API.

### Current Phase

`wasi-messaging` is currently in [Phase 1](https://github.com/WebAssembly/WASI/blob/main/Proposals.md#phase-1---feature-proposal-cg).

### Champions

- [Dan Chiarlone](https://github.com/danbugs)
- [David Justice](https://github.com/devigned)
- [Jiaxiao Zhou](https://github.com/Mossaka)
- [Taylor Thomas](https://github.com/thomastaylor312)

### Phase 4 Advancement Criteria

`wasi-messaging` should have at least two implementations (i.e., from service providers, and or cloud providers), and, at the very minimum, pass the testsuite for Windows, Linux, and MacOS.

## Table of Contents

- [Introduction](#introduction)
- [Goals](#goals)
- [Non-goals](#non-goals)

### Introduction

The messaging interfaces aim to provide a generic and flexible way for producers and consumers to communicate through message brokers. The `producer` interface allows producers to publish events to a specific channel in a broker, while the `consumer` interface allows consumers to subscribe to a channel and receive events through a push-based mechanism. The handler interface provides an on-receive function that can be used to process received events with full abstraction of the underlying broker implementation.

### Goals

The messaging interfaces aim to address the need for a standard way to handle message-based communication in modern software systems. In complex software systems, different components or even different applications need to communicate with each other to exchange information and coordinate their actions.

However, implementing message-based communication can be challenging, as it requires dealing with the details of message brokers, such as connection management, channel setup, and message serialization. The messaging interfaces aim to simplify this process by providing a standard way to interact with message brokers, hiding the underlying complexity from the user.

This standardization can benefit various scenarios, such as 

- Microservice architectures, where each microservice can communicate with other microservices using the messaging service interfaces. Similarly, applications that need to handle event-driven or streaming data can benefit from the push-based message delivery mechanism provided by the `consumer` and `handler` interfaces;

- Local use cases such as communication channels between online and offline browser-based Web applications and local WASI applications.


Overall, the messaging interfaces aim to make it easier to build complex and scalable software systems by providing a common foundation for message-based communication.

### Non-goals

- The messaging service interfaces do not aim to provide advanced features of message brokers, such as broker clustering, message persistence, or guaranteed message delivery. These are implementation-specific details that are not addressed by the interfaces.
- The messaging service interfaces do not aim to provide support for every possible messaging pattern or use case. Instead, they focus on the common use cases of pub-sub and push-based message delivery. Other messaging patterns, such as request-reply or publish-confirm-subscribe, are outside the scope of the interfaces.
- The messaging service interfaces do not aim to provide a specific implementation of a message broker. Instead, they provide a standard way to interact with any message broker that supports the interfaces.

### API walk-through

For a full API walk-through, see [wasi-messaging-demo](https://github.com/danbugs/wasi-messaging-demo).

> Note: This README needs to be expanded to cover a number of additional fields suggested in the
[WASI Proposal template](https://github.com/WebAssembly/wasi-proposal-template).
