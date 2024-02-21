<h1><a name="imports">World imports</a></h1>
<ul>
<li>Imports:
<ul>
<li>interface <a href="#wasi:messaging_messaging_types_0.2.0_draft"><code>wasi:messaging/messaging-types@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi:messaging_producer_0.2.0_draft"><code>wasi:messaging/producer@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi:messaging_consumer_0.2.0_draft"><code>wasi:messaging/consumer@0.2.0-draft</code></a></li>
</ul>
</li>
</ul>
<h2><a name="wasi:messaging_messaging_types_0.2.0_draft"></a>Import interface wasi:messaging/messaging-types@0.2.0-draft</h2>
<hr />
<h3>Types</h3>
<h4><a name="client"></a><code>resource client</code></h4>
<p>A connection to a message-exchange service (e.g., buffer, broker, etc.).</p>
<h4><a name="error"></a><code>resource error</code></h4>
<p>TODO(danbugs): This should be eventually extracted as an underlying type for other wasi-cloud-core interfaces.</p>
<h4><a name="channel"></a><code>type channel</code></h4>
<p><code>string</code></p>
<p>There are two types of channels:
- publish-subscribe channel, which is a broadcast channel, and
- point-to-point channel, which is a unicast channel.
<p>The interface doesn't highlight this difference in the type itself as that's uniquely a consumer issue.</p>
<h4><a name="guest_configuration"></a><code>record guest-configuration</code></h4>
<p>Configuration includes a required list of channels the guest is subscribing to, and an optional list of extensions key-value pairs
(e.g., partitions/offsets to read from in Kafka/EventHubs, QoS etc.).</p>
<h5>Record Fields</h5>
<ul>
<li><a name="guest_configuration.channels"></a><code>channels</code>: list&lt;<a href="#channel"><a href="#channel"><code>channel</code></a></a>&gt;</li>
<li><a name="guest_configuration.extensions"></a><code>extensions</code>: option&lt;list&lt;(<code>string</code>, <code>string</code>)&gt;&gt;</li>
</ul>
<h4><a name="format_spec"></a><code>enum format-spec</code></h4>
<p>Format specification for messages</p>
<ul>
<li>more info: https://github.com/clemensv/spec/blob/registry-extensions/registry/spec.md#message-formats</li>
<li>message metadata can further decorate w/ things like format version, and so on.</li>
</ul>
<h5>Enum Cases</h5>
<ul>
<li><a name="format_spec.cloudevents"></a><code>cloudevents</code></li>
<li><a name="format_spec.http"></a><code>http</code></li>
<li><a name="format_spec.amqp"></a><code>amqp</code></li>
<li><a name="format_spec.mqtt"></a><code>mqtt</code></li>
<li><a name="format_spec.kafka"></a><code>kafka</code></li>
<li><a name="format_spec.raw"></a><code>raw</code></li>
</ul>
<h4><a name="message"></a><code>record message</code></h4>
<p>A message with a binary payload, a format specification, and decorative metadata.</p>
<h5>Record Fields</h5>
<ul>
<li><a name="message.data"></a><code>data</code>: list&lt;<code>u8</code>&gt;</li>
<li><a name="message.format"></a><code>format</code>: <a href="#format_spec"><a href="#format_spec"><code>format-spec</code></a></a></li>
<li><a name="message.metadata"></a><code>metadata</code>: option&lt;list&lt;(<code>string</code>, <code>string</code>)&gt;&gt;</li>
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
<li><a name="static_client.connect.0"></a> result&lt;own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="static_error.trace"></a><code>[static]error.trace: func</code></h4>
<h5>Return values</h5>
<ul>
<li><a name="static_error.trace.0"></a> <code>string</code></li>
</ul>
<h2><a name="wasi:messaging_producer_0.2.0_draft"></a>Import interface wasi:messaging/producer@0.2.0-draft</h2>
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
<h5>Params</h5>
<ul>
<li><a name="send.c"></a><code>c</code>: own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
<li><a name="send.ch"></a><code>ch</code>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
<li><a name="send.m"></a><code>m</code>: list&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="send.0"></a> result&lt;_, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h2><a name="wasi:messaging_consumer_0.2.0_draft"></a>Import interface wasi:messaging/consumer@0.2.0-draft</h2>
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
<h4><a name="subscribe_try_receive"></a><code>subscribe-try-receive: func</code></h4>
<p>Blocking receive for t-milliseconds with ephemeral subscription – if no message is received, returns None</p>
<h5>Params</h5>
<ul>
<li><a name="subscribe_try_receive.c"></a><code>c</code>: own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
<li><a name="subscribe_try_receive.ch"></a><code>ch</code>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
<li><a name="subscribe_try_receive.t_milliseconds"></a><code>t-milliseconds</code>: <code>u32</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="subscribe_try_receive.0"></a> result&lt;option&lt;list&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;&gt;, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="subscribe_receive"></a><code>subscribe-receive: func</code></h4>
<p>Blocking receive until message with ephemeral subscription</p>
<h5>Params</h5>
<ul>
<li><a name="subscribe_receive.c"></a><code>c</code>: own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
<li><a name="subscribe_receive.ch"></a><code>ch</code>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="subscribe_receive.0"></a> result&lt;list&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="update_guest_configuration"></a><code>update-guest-configuration: func</code></h4>
<p>'Fit-all' type function for updating a guest's configuration – this could be useful for:</p>
<ul>
<li>unsubscribing from a channel,</li>
<li>checkpointing,</li>
<li>etc..</li>
</ul>
<h5>Params</h5>
<ul>
<li><a name="update_guest_configuration.gc"></a><code>gc</code>: <a href="#guest_configuration"><a href="#guest_configuration"><code>guest-configuration</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="update_guest_configuration.0"></a> result&lt;_, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
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
<li><a name="complete_message.0"></a> result&lt;_, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="abandon_message"></a><code>abandon-message: func</code></h4>
<h5>Params</h5>
<ul>
<li><a name="abandon_message.m"></a><code>m</code>: <a href="#message"><a href="#message"><code>message</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="abandon_message.0"></a> result&lt;_, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
