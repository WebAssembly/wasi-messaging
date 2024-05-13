<h1><a name="messaging">World messaging</a></h1>
<ul>
<li>Imports:
<ul>
<li>interface <a href="#wasi:messaging_types_0.2.0_draft"><code>wasi:messaging/types@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi:messaging_producer_0.2.0_draft"><code>wasi:messaging/producer@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi:messaging_consumer_0.2.0_draft"><code>wasi:messaging/consumer@0.2.0-draft</code></a></li>
</ul>
</li>
<li>Exports:
<ul>
<li>interface <a href="#wasi:messaging_guest_0.2.0_draft"><code>wasi:messaging/guest@0.2.0-draft</code></a></li>
</ul>
</li>
</ul>
<h2><a name="wasi:messaging_types_0.2.0_draft"></a>Import interface wasi:messaging/types@0.2.0-draft</h2>
<hr />
<h3>Types</h3>
<h4><a name="client"></a><code>resource client</code></h4>
<p>A connection to a message-exchange service (e.g., buffer, broker, etc.).</p>
<h4><a name="error"></a><code>variant error</code></h4>
<p>Errors that can occur when using the messaging interface.</p>
<h5>Variant Cases</h5>
<ul>
<li>
<p><a name="error.unauthorized"></a><code>unauthorized</code></p>
<p>The requested option is not authorized. This could be a topic it doesn't have
permission to subscribe to, or a permission it doesn't have to perform a specific
action. This error is mainly used when calling `update-guest-configuration`.
</li>
<li>
<p><a name="error.timeout"></a><code>timeout</code></p>
<p>The request or operation timed out.
</li>
<li>
<p><a name="error.connection"></a><code>connection</code>: <code>string</code></p>
<p>An error occurred with the connection. Includes a message for additional context
</li>
<li>
<p><a name="error.other"></a><code>other</code>: <code>string</code></p>
<p>A catch all for other types of errors
</li>
</ul>
<h4><a name="channel"></a><code>type channel</code></h4>
<p><code>string</code></p>
<p>There are two types of channels:
- publish-subscribe channel, which is a broadcast channel, and
- point-to-point channel, which is a unicast channel.
<p>The interface doesn't highlight this difference in the type itself as that's uniquely a consumer issue.</p>
<h4><a name="guest_configuration"></a><code>record guest-configuration</code></h4>
<p>Configuration includes a required list of channels the guest is subscribing to, and an
optional list of extensions key-value pairs (e.g., partitions/offsets to read from in
Kafka/EventHubs, QoS etc.).</p>
<h5>Record Fields</h5>
<ul>
<li><a name="guest_configuration.channels"></a><code>channels</code>: list&lt;<a href="#channel"><a href="#channel"><code>channel</code></a></a>&gt;</li>
<li><a name="guest_configuration.extensions"></a><code>extensions</code>: option&lt;list&lt;(<code>string</code>, <code>string</code>)&gt;&gt;</li>
</ul>
<h4><a name="message"></a><code>record message</code></h4>
<p>A message with a binary payload and additional information</p>
<h5>Record Fields</h5>
<ul>
<li>
<p><a name="message.topic"></a><code>topic</code>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></p>
<p>The topic/subject/channel this message was received or should be sent on
</li>
<li>
<p><a name="message.content_type"></a><code>content-type</code>: option&lt;<code>string</code>&gt;</p>
<p>An optional content-type describing the format of the data in the message. This is
sometimes described as the "format" type
</li>
<li>
<p><a name="message.reply_to"></a><code>reply-to</code>: option&lt;<code>string</code>&gt;</p>
<p>An optional topic for use in request/response scenarios. Senders and consumers of
messages must not assume that this field is set and should handle it in their code
accordingly.
</li>
<li>
<p><a name="message.data"></a><code>data</code>: list&lt;<code>u8</code>&gt;</p>
<p>An opaque blob of data
</li>
<li>
<p><a name="message.metadata"></a><code>metadata</code>: option&lt;list&lt;(<code>string</code>, <code>string</code>)&gt;&gt;</p>
<p>Optional metadata (also called headers or attributes in some systems) attached to the
message
</li>
</ul>
<hr />
<h3>Functions</h3>
<h4><a name="static_client.connect"></a><code>[static]client.connect: func</code></h4>
<h5>Params</h5>
<ul>
<li><a name="static_client.connect.name"></a><code>name</code>: <code>string</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="static_client.connect.0"></a> result&lt;own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h2><a name="wasi:messaging_producer_0.2.0_draft"></a>Import interface wasi:messaging/producer@0.2.0-draft</h2>
<p>The producer interface is used to send messages to a channel/topic.</p>
<hr />
<h3>Types</h3>
<h4><a name="client"></a><code>type client</code></h4>
<p><a href="#client"><a href="#client"><code>client</code></a></a></p>
<p>
#### <a name="channel"></a>`type channel`
[`channel`](#channel)
<p>
#### <a name="message"></a>`type message`
[`message`](#message)
<p>
#### <a name="error"></a>`type error`
[`error`](#error)
<p>
----
<h3>Functions</h3>
<h4><a name="send"></a><code>send: func</code></h4>
<p>Sends a message to the given channel/topic. If the channel/topic is not empty, it will
override the channel/topic in the message.</p>
<h5>Params</h5>
<ul>
<li><a name="send.c"></a><code>c</code>: own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
<li><a name="send.ch"></a><code>ch</code>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
<li><a name="send.m"></a><code>m</code>: list&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="send.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h2><a name="wasi:messaging_consumer_0.2.0_draft"></a>Import interface wasi:messaging/consumer@0.2.0-draft</h2>
<p>The consumer interface allows a guest to dynamically update its subscriptions and configuration
as well as functionality for completing (acking) or abandoning (nacking) messages.</p>
<hr />
<h3>Types</h3>
<h4><a name="client"></a><code>type client</code></h4>
<p><a href="#client"><a href="#client"><code>client</code></a></a></p>
<p>
#### <a name="message"></a>`type message`
[`message`](#message)
<p>
#### <a name="channel"></a>`type channel`
[`channel`](#channel)
<p>
#### <a name="error"></a>`type error`
[`error`](#error)
<p>
#### <a name="guest_configuration"></a>`type guest-configuration`
[`guest-configuration`](#guest_configuration)
<p>
----
<h3>Functions</h3>
<h4><a name="update_guest_configuration"></a><code>update-guest-configuration: func</code></h4>
<p>'Fit-all' type function for updating a guest's configuration – this could be useful for:</p>
<ul>
<li>unsubscribing from a channel,</li>
<li>checkpointing,</li>
<li>etc..</li>
</ul>
<p>Please note that implementations that provide <code>wasi:messaging</code> are responsible for ensuring
that guests are not allowed to subscribe to channels that they are not configured to
subscribe to (or have access to). Failure to do so can result in possible breakout or access
to resources that are not intended to be accessible to the guest. This means implementations
should validate that the configured topics are valid topics the guest should have access to or
enforce it via the credentials used to connect to the service.</p>
<h5>Params</h5>
<ul>
<li><a name="update_guest_configuration.gc"></a><code>gc</code>: <a href="#guest_configuration"><a href="#guest_configuration"><code>guest-configuration</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="update_guest_configuration.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a name="complete_message"></a><code>complete-message: func</code></h4>
<p>A message can exist under several statuses:
(1) available: the message is ready to be read,
(2) acquired: the message has been sent to a consumer (but still exists in the queue),
(3) accepted (result of complete-message): the message has been received and ACK-ed by a consumer and can be safely removed from the queue,
(4) rejected (result of abandon-message): the message has been received and NACK-ed by a consumer, at which point it can be:</p>
<ul>
<li>deleted,</li>
<li>sent to a dead-letter queue, or</li>
<li>kept in the queue for further processing.</li>
</ul>
<h5>Params</h5>
<ul>
<li><a name="complete_message.m"></a><code>m</code>: <a href="#message"><a href="#message"><code>message</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="complete_message.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a name="abandon_message"></a><code>abandon-message: func</code></h4>
<h5>Params</h5>
<ul>
<li><a name="abandon_message.m"></a><code>m</code>: <a href="#message"><a href="#message"><code>message</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="abandon_message.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h2><a name="wasi:messaging_guest_0.2.0_draft"></a>Export interface wasi:messaging/guest@0.2.0-draft</h2>
<hr />
<h3>Types</h3>
<h4><a name="message"></a><code>type message</code></h4>
<p><a href="#message"><a href="#message"><code>message</code></a></a></p>
<p>
#### <a name="guest_configuration"></a>`type guest-configuration`
[`guest-configuration`](#guest_configuration)
<p>
#### <a name="error"></a>`type error`
[`error`](#error)
<p>
----
<h3>Functions</h3>
<h4><a name="handler"></a><code>handler: func</code></h4>
<p>Whenever this guest receives a message in one of the subscribed channels, the message is
sent to this handler</p>
<h5>Params</h5>
<ul>
<li><a name="handler.ms"></a><code>ms</code>: list&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="handler.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
