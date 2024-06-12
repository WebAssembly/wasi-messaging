<h1><a name="imports">World imports</a></h1>
<ul>
<li>Imports:
<ul>
<li>interface <a href="#wasi:messaging_types_0.2.0_draft"><code>wasi:messaging/types@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi:messaging_producer_0.2.0_draft"><code>wasi:messaging/producer@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi:messaging_consumer_0.2.0_draft"><code>wasi:messaging/consumer@0.2.0-draft</code></a></li>
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
optional list of extensions key-value pairs</p>
<h5>Record Fields</h5>
<ul>
<li><a name="guest_configuration.channels"></a><code>channels</code>: list&lt;<a href="#channel"><a href="#channel"><code>channel</code></a></a>&gt;</li>
</ul>
<h4><a name="message"></a><code>resource message</code></h4>
<h2>A message with a binary payload and additional information</h2>
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
<h4><a name="constructor_message"></a><code>[constructor]message: func</code></h4>
<h5>Params</h5>
<ul>
<li><a name="constructor_message.topic"></a><code>topic</code>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
<li><a name="constructor_message.data"></a><code>data</code>: list&lt;<code>u8</code>&gt;</li>
<li><a name="constructor_message.content_type"></a><code>content-type</code>: option&lt;<code>string</code>&gt;</li>
<li><a name="constructor_message.metadata"></a><code>metadata</code>: option&lt;list&lt;(<code>string</code>, <code>string</code>)&gt;&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="constructor_message.0"></a> own&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h4><a name="method_message.topic"></a><code>[method]message.topic: func</code></h4>
<p>The topic/subject/channel this message was received or should be sent on</p>
<h5>Params</h5>
<ul>
<li><a name="method_message.topic.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="method_message.topic.0"></a> <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
</ul>
<h4><a name="method_message.set_topic"></a><code>[method]message.set-topic: func</code></h4>
<p>Set the topic/subject/channel this message should be sent on</p>
<h5>Params</h5>
<ul>
<li><a name="method_message.set_topic.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a name="method_message.set_topic.topic"></a><code>topic</code>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
</ul>
<h4><a name="method_message.content_type"></a><code>[method]message.content-type: func</code></h4>
<p>An optional content-type describing the format of the data in the message. This is
sometimes described as the &quot;format&quot; type</p>
<h5>Params</h5>
<ul>
<li><a name="method_message.content_type.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="method_message.content_type.0"></a> option&lt;<code>string</code>&gt;</li>
</ul>
<h4><a name="method_message.set_content_type"></a><code>[method]message.set-content-type: func</code></h4>
<p>Set the content-type describing the format of the data in the message. This is
sometimes described as the &quot;format&quot; type</p>
<h5>Params</h5>
<ul>
<li><a name="method_message.set_content_type.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a name="method_message.set_content_type.content_type"></a><code>content-type</code>: <code>string</code></li>
</ul>
<h4><a name="method_message.data"></a><code>[method]message.data: func</code></h4>
<p>An opaque blob of data</p>
<h5>Params</h5>
<ul>
<li><a name="method_message.data.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="method_message.data.0"></a> list&lt;<code>u8</code>&gt;</li>
</ul>
<h4><a name="method_message.set_data"></a><code>[method]message.set-data: func</code></h4>
<p>Set the opaque blob of data for this message, discarding the old value</p>
<h5>Params</h5>
<ul>
<li><a name="method_message.set_data.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a name="method_message.set_data.data"></a><code>data</code>: list&lt;<code>u8</code>&gt;</li>
</ul>
<h4><a name="method_message.metadata"></a><code>[method]message.metadata: func</code></h4>
<p>Optional metadata (also called headers or attributes in some systems) attached to the
message</p>
<h5>Params</h5>
<ul>
<li><a name="method_message.metadata.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="method_message.metadata.0"></a> option&lt;list&lt;(<code>string</code>, <code>string</code>)&gt;&gt;</li>
</ul>
<h4><a name="method_message.add_metadata"></a><code>[method]message.add-metadata: func</code></h4>
<p>Add a new key-value pair to the metadata, overwriting any existing value for the same key</p>
<h5>Params</h5>
<ul>
<li><a name="method_message.add_metadata.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a name="method_message.add_metadata.key"></a><code>key</code>: <code>string</code></li>
<li><a name="method_message.add_metadata.value"></a><code>value</code>: <code>string</code></li>
</ul>
<h4><a name="method_message.complete"></a><code>[method]message.complete: func</code></h4>
<p>Completes/acks the message</p>
<p>A message can exist under several statuses:
(1) available: the message is ready to be read,
(2) acquired: the message has been sent to a consumer (but still exists in the queue),
(3) accepted (result of complete): the message has been received and ACK-ed by a consumer and can be safely removed from the queue,
(4) rejected (result of abandon): the message has been received and NACK-ed by a consumer, at which point it can be:</p>
<ul>
<li>deleted,</li>
<li>sent to a dead-letter queue, or</li>
<li>kept in the queue for further processing.</li>
</ul>
<h5>Params</h5>
<ul>
<li><a name="method_message.complete.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="method_message.complete.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a name="method_message.abandon"></a><code>[method]message.abandon: func</code></h4>
<p>Abandon/nacks the message</p>
<p>A message can exist under several statuses:
(1) available: the message is ready to be read,
(2) acquired: the message has been sent to a consumer (but still exists in the queue),
(3) accepted (result of complete): the message has been received and ACK-ed by a consumer and can be safely removed from the queue,
(4) rejected (result of abandon): the message has been received and NACK-ed by a consumer, at which point it can be:</p>
<ul>
<li>deleted,</li>
<li>sent to a dead-letter queue, or</li>
<li>kept in the queue for further processing.</li>
</ul>
<h5>Params</h5>
<ul>
<li><a name="method_message.abandon.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="method_message.abandon.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
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
<li><a name="send.m"></a><code>m</code>: own&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="send.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h2><a name="wasi:messaging_consumer_0.2.0_draft"></a>Import interface wasi:messaging/consumer@0.2.0-draft</h2>
<p>The consumer interface allows a guest to dynamically update its subscriptions and configuration</p>
<hr />
<h3>Types</h3>
<h4><a name="error"></a><code>type error</code></h4>
<p><a href="#error"><a href="#error"><code>error</code></a></a></p>
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
