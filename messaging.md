<h1><a name="messaging">World messaging</a></h1>
<ul>
<li>Imports:
<ul>
<li>interface <a href="#wasi:messaging_messaging_types_0.1.0"><code>wasi:messaging/messaging-types@0.1.0</code></a></li>
<li>interface <a href="#wasi:messaging_producer_0.1.0"><code>wasi:messaging/producer@0.1.0</code></a></li>
<li>interface <a href="#wasi:messaging_consumer_0.1.0"><code>wasi:messaging/consumer@0.1.0</code></a></li>
</ul>
</li>
<li>Exports:
<ul>
<li>interface <a href="#wasi:messaging_messaging_guest_0.1.0"><code>wasi:messaging/messaging-guest@0.1.0</code></a></li>
</ul>
</li>
</ul>
<h2><a name="wasi:messaging_messaging_types_0.1.0">Import interface wasi:messaging/messaging-types@0.1.0</a></h2>
<hr />
<h3>Types</h3>
<h4><a name="client"><code>resource client</code></a></h4>
<p>A connection to a message-exchange service (e.g., buffer, broker, etc.).</p>
<h4><a name="error"><code>resource error</code></a></h4>
<p>TODO(danbugs): This should be eventually extracted as an underlying type for other wasi-cloud-core interfaces.</p>
<h4><a name="channel"><code>type channel</code></a></h4>
<p><code>string</code></p>
<p>There are two types of channels:
- publish-subscribe channel, which is a broadcast channel, and
- point-to-point channel, which is a unicast channel.
<p>The interface doesn't highlight this difference in the type itself as that's uniquely a consumer issue.</p>
<h4><a name="guest_configuration"><code>record guest-configuration</code></a></h4>
<p>Configuration includes a required list of channels the guest is subscribing to, and an optional list of extensions key-value pairs
(e.g., partitions/offsets to read from in Kafka/EventHubs, QoS etc.).</p>
<h5>Record Fields</h5>
<ul>
<li><a name="guest_configuration.channels"><code>channels</code></a>: list&lt;<a href="#channel"><a href="#channel"><code>channel</code></a></a>&gt;</li>
<li><a name="guest_configuration.extensions"><code>extensions</code></a>: option&lt;list&lt;(<code>string</code>, <code>string</code>)&gt;&gt;</li>
</ul>
<h4><a name="format_spec"><code>enum format-spec</code></a></h4>
<p>Format specification for messages</p>
<ul>
<li>more info: https://github.com/clemensv/spec/blob/registry-extensions/registry/spec.md#message-formats</li>
<li>message metadata can further decorate w/ things like format version, and so on.</li>
</ul>
<h5>Enum Cases</h5>
<ul>
<li><a name="format_spec.cloudevents"><code>cloudevents</code></a></li>
<li><a name="format_spec.http"><code>http</code></a></li>
<li><a name="format_spec.amqp"><code>amqp</code></a></li>
<li><a name="format_spec.mqtt"><code>mqtt</code></a></li>
<li><a name="format_spec.kafka"><code>kafka</code></a></li>
<li><a name="format_spec.raw"><code>raw</code></a></li>
</ul>
<h4><a name="message"><code>record message</code></a></h4>
<p>A message with a binary payload, a format specification, and decorative metadata.</p>
<h5>Record Fields</h5>
<ul>
<li><a name="message.data"><code>data</code></a>: list&lt;<code>u8</code>&gt;</li>
<li><a name="message.format"><code>format</code></a>: <a href="#format_spec"><a href="#format_spec"><code>format-spec</code></a></a></li>
<li><a name="message.metadata"><code>metadata</code></a>: option&lt;list&lt;(<code>string</code>, <code>string</code>)&gt;&gt;</li>
</ul>
<hr />
<h3>Functions</h3>
<h4><a name="static_client.connect"><code>[static]client.connect: func</code></a></h4>
<h5>Params</h5>
<ul>
<li><a name="static_client.connect.name"><code>name</code></a>: <code>string</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="static_client.connect.0"></a> result&lt;own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="static_error.trace"><code>[static]error.trace: func</code></a></h4>
<h5>Return values</h5>
<ul>
<li><a name="static_error.trace.0"></a> <code>string</code></li>
</ul>
<h2><a name="wasi:messaging_producer_0.1.0">Import interface wasi:messaging/producer@0.1.0</a></h2>
<hr />
<h3>Types</h3>
<h4><a name="client"><code>type client</code></a></h4>
<p><a href="#client"><a href="#client"><code>client</code></a></a></p>
<p>
#### <a name="channel">`type channel`</a>
[`channel`](#channel)
<p>
#### <a name="message">`type message`</a>
[`message`](#message)
<p>
#### <a name="error">`type error`</a>
[`error`](#error)
<p>
----
<h3>Functions</h3>
<h4><a name="send"><code>send: func</code></a></h4>
<h5>Params</h5>
<ul>
<li><a name="send.c"><code>c</code></a>: own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
<li><a name="send.ch"><code>ch</code></a>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
<li><a name="send.m"><code>m</code></a>: list&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="send.0"></a> result&lt;_, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h2><a name="wasi:messaging_consumer_0.1.0">Import interface wasi:messaging/consumer@0.1.0</a></h2>
<hr />
<h3>Types</h3>
<h4><a name="client"><code>type client</code></a></h4>
<p><a href="#client"><a href="#client"><code>client</code></a></a></p>
<p>
#### <a name="message">`type message`</a>
[`message`](#message)
<p>
#### <a name="channel">`type channel`</a>
[`channel`](#channel)
<p>
#### <a name="error">`type error`</a>
[`error`](#error)
<p>
#### <a name="guest_configuration">`type guest-configuration`</a>
[`guest-configuration`](#guest_configuration)
<p>
----
<h3>Functions</h3>
<h4><a name="subscribe_try_receive"><code>subscribe-try-receive: func</code></a></h4>
<p>Blocking receive for t-milliseconds with ephemeral subscription – if no message is received, returns None</p>
<h5>Params</h5>
<ul>
<li><a name="subscribe_try_receive.c"><code>c</code></a>: own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
<li><a name="subscribe_try_receive.ch"><code>ch</code></a>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
<li><a name="subscribe_try_receive.t_milliseconds"><code>t-milliseconds</code></a>: <code>u32</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="subscribe_try_receive.0"></a> result&lt;option&lt;list&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;&gt;, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="subscribe_receive"><code>subscribe-receive: func</code></a></h4>
<p>Blocking receive until message with ephemeral subscription</p>
<h5>Params</h5>
<ul>
<li><a name="subscribe_receive.c"><code>c</code></a>: own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
<li><a name="subscribe_receive.ch"><code>ch</code></a>: <a href="#channel"><a href="#channel"><code>channel</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="subscribe_receive.0"></a> result&lt;list&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="update_guest_configuration"><code>update-guest-configuration: func</code></a></h4>
<p>'Fit-all' type function for updating a guest's configuration – this could be useful for:</p>
<ul>
<li>unsubscribing from a channel,</li>
<li>checkpointing,</li>
<li>etc..</li>
</ul>
<h5>Params</h5>
<ul>
<li><a name="update_guest_configuration.gc"><code>gc</code></a>: <a href="#guest_configuration"><a href="#guest_configuration"><code>guest-configuration</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="update_guest_configuration.0"></a> result&lt;_, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="complete_message"><code>complete-message: func</code></a></h4>
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
<li><a name="complete_message.m"><code>m</code></a>: <a href="#message"><a href="#message"><code>message</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="complete_message.0"></a> result&lt;_, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="abandon_message"><code>abandon-message: func</code></a></h4>
<h5>Params</h5>
<ul>
<li><a name="abandon_message.m"><code>m</code></a>: <a href="#message"><a href="#message"><code>message</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="abandon_message.0"></a> result&lt;_, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h2><a name="wasi:messaging_messaging_guest_0.1.0">Export interface wasi:messaging/messaging-guest@0.1.0</a></h2>
<hr />
<h3>Types</h3>
<h4><a name="message"><code>type message</code></a></h4>
<p><a href="#message"><a href="#message"><code>message</code></a></a></p>
<p>
#### <a name="guest_configuration">`type guest-configuration`</a>
[`guest-configuration`](#guest_configuration)
<p>
#### <a name="error">`type error`</a>
[`error`](#error)
<p>
----
<h3>Functions</h3>
<h4><a name="configure"><code>configure: func</code></a></h4>
<p>Returns the list of channels (and extension metadata within guest-configuration) that
this component should subscribe to and be handled by the subsequent handler within guest-configuration</p>
<h5>Return values</h5>
<ul>
<li><a name="configure.0"></a> result&lt;<a href="#guest_configuration"><a href="#guest_configuration"><code>guest-configuration</code></a></a>, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
<h4><a name="handler"><code>handler: func</code></a></h4>
<p>Whenever this guest receives a message in one of the subscribed channels, the message is sent to this handler</p>
<h5>Params</h5>
<ul>
<li><a name="handler.ms"><code>ms</code></a>: list&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a name="handler.0"></a> result&lt;_, own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;&gt;</li>
</ul>
