<h1><a id="messaging_request_reply"></a>World messaging-request-reply</h1>
<p>The <a href="#messaging_request_reply"><code>messaging-request-reply</code></a> world combines <code>imports-request-reply</code> with the <code>incoming-handler</code>
export. This setup allows the host to interact with the component for both sending messages and
handling incoming messages with request/reply capabilities.</p>
<ul>
<li>Imports:
<ul>
<li>interface <a href="#wasi_messaging_types_0_2_0_draft"><code>wasi:messaging/types@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi_messaging_request_reply_0_2_0_draft"><code>wasi:messaging/request-reply@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi_messaging_producer_0_2_0_draft"><code>wasi:messaging/producer@0.2.0-draft</code></a></li>
</ul>
</li>
<li>Exports:
<ul>
<li>interface <a href="#wasi_messaging_incoming_handler_0_2_0_draft"><code>wasi:messaging/incoming-handler@0.2.0-draft</code></a></li>
</ul>
</li>
</ul>
<h2><a id="wasi_messaging_types_0_2_0_draft"></a>Import interface wasi:messaging/types@0.2.0-draft</h2>
<hr />
<h3>Types</h3>
<h4><a id="metadata"></a><code>type metadata</code></h4>
<p><a href="#metadata"><a href="#metadata"><code>metadata</code></a></a></p>
<p>A type alias for list<tuple<string, string>> to represent metadata attached to a message
<h4><a id="topic"></a><code>type topic</code></h4>
<p><code>string</code></p>
<p>A type alias for string to represent a message topic
<h4><a id="client"></a><code>resource client</code></h4>
<p>A connection to a message-exchange service (e.g., buffer, broker, etc.).</p>
<h4><a id="error"></a><code>variant error</code></h4>
<p>Errors that can occur when using the messaging interface.</p>
<h5>Variant Cases</h5>
<ul>
<li>
<p><a id="error.timeout"></a><code>timeout</code></p>
<p>The request or operation timed out.
</li>
<li>
<p><a id="error.connection"></a><code>connection</code>: <code>string</code></p>
<p>An error occurred with the connection. Includes a message for additional context
</li>
<li>
<p><a id="error.permission_denied"></a><code>permission-denied</code>: <code>string</code></p>
<p>A permission error occurred. Includes a message for additional context
</li>
<li>
<p><a id="error.other"></a><code>other</code>: <code>string</code></p>
<p>A catch all for other types of errors
</li>
</ul>
<h4><a id="message"></a><code>resource message</code></h4>
<h2>A message with a binary payload and additional information</h2>
<h3>Functions</h3>
<h4><a id="static_client_connect"></a><code>[static]client.connect: func</code></h4>
<h5>Params</h5>
<ul>
<li><a id="static_client_connect.name"></a><code>name</code>: <code>string</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="static_client_connect.0"></a> result&lt;own&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_client_disconnect"></a><code>[method]client.disconnect: func</code></h4>
<h5>Params</h5>
<ul>
<li><a id="method_client_disconnect.self"></a><code>self</code>: borrow&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_client_disconnect.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="constructor_message"></a><code>[constructor]message: func</code></h4>
<h5>Params</h5>
<ul>
<li><a id="constructor_message.data"></a><code>data</code>: list&lt;<code>u8</code>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="constructor_message.0"></a> own&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h4><a id="method_message_topic"></a><code>[method]message.topic: func</code></h4>
<p>The topic/subject/channel this message was received or should be sent on</p>
<h5>Params</h5>
<ul>
<li><a id="method_message_topic.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_message_topic.0"></a> <a href="#topic"><a href="#topic"><code>topic</code></a></a></li>
</ul>
<h4><a id="method_message_content_type"></a><code>[method]message.content-type: func</code></h4>
<p>An optional content-type describing the format of the data in the message. This is
sometimes described as the &quot;format&quot; type</p>
<h5>Params</h5>
<ul>
<li><a id="method_message_content_type.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_message_content_type.0"></a> option&lt;<code>string</code>&gt;</li>
</ul>
<h4><a id="method_message_set_content_type"></a><code>[method]message.set-content-type: func</code></h4>
<p>Set the content-type describing the format of the data in the message. This is
sometimes described as the &quot;format&quot; type</p>
<h5>Params</h5>
<ul>
<li><a id="method_message_set_content_type.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a id="method_message_set_content_type.content_type"></a><code>content-type</code>: <code>string</code></li>
</ul>
<h4><a id="method_message_data"></a><code>[method]message.data: func</code></h4>
<p>An opaque blob of data</p>
<h5>Params</h5>
<ul>
<li><a id="method_message_data.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_message_data.0"></a> list&lt;<code>u8</code>&gt;</li>
</ul>
<h4><a id="method_message_set_data"></a><code>[method]message.set-data: func</code></h4>
<p>Set the opaque blob of data for this message, discarding the old value</p>
<h5>Params</h5>
<ul>
<li><a id="method_message_set_data.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a id="method_message_set_data.data"></a><code>data</code>: list&lt;<code>u8</code>&gt;</li>
</ul>
<h4><a id="method_message_metadata"></a><code>[method]message.metadata: func</code></h4>
<p>Optional metadata (also called headers or attributes in some systems) attached to the
message</p>
<h5>Params</h5>
<ul>
<li><a id="method_message_metadata.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_message_metadata.0"></a> option&lt;<a href="#metadata"><a href="#metadata"><code>metadata</code></a></a>&gt;</li>
</ul>
<h4><a id="method_message_add_metadata"></a><code>[method]message.add-metadata: func</code></h4>
<p>Add a new key-value pair to the metadata, overwriting any existing value for the same key</p>
<h5>Params</h5>
<ul>
<li><a id="method_message_add_metadata.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a id="method_message_add_metadata.key"></a><code>key</code>: <code>string</code></li>
<li><a id="method_message_add_metadata.value"></a><code>value</code>: <code>string</code></li>
</ul>
<h4><a id="method_message_set_metadata"></a><code>[method]message.set-metadata: func</code></h4>
<p>Set the metadata</p>
<h5>Params</h5>
<ul>
<li><a id="method_message_set_metadata.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a id="method_message_set_metadata.meta"></a><code>meta</code>: <a href="#metadata"><a href="#metadata"><code>metadata</code></a></a></li>
</ul>
<h4><a id="method_message_remove_metadata"></a><code>[method]message.remove-metadata: func</code></h4>
<p>Remove a key-value pair from the metadata</p>
<h5>Params</h5>
<ul>
<li><a id="method_message_remove_metadata.self"></a><code>self</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a id="method_message_remove_metadata.key"></a><code>key</code>: <code>string</code></li>
</ul>
<h2><a id="wasi_messaging_request_reply_0_2_0_draft"></a>Import interface wasi:messaging/request-reply@0.2.0-draft</h2>
<p>The request-reply interface allows a guest to send a message and await a response. This
interface is considered optional as not all message services support the concept of
request/reply. However, request/reply is a very common pattern in messaging and as such, we have
included it as a core interface.</p>
<hr />
<h3>Types</h3>
<h4><a id="client"></a><code>type client</code></h4>
<p><a href="#client"><a href="#client"><code>client</code></a></a></p>
<p>
#### <a id="message"></a>`type message`
[`message`](#message)
<p>
#### <a id="error"></a>`type error`
[`error`](#error)
<p>
#### <a id="request_options"></a>`resource request-options`
<h2>Options for a request/reply operation. This is a resource to allow for future expansion of
options.</h2>
<h3>Functions</h3>
<h4><a id="constructor_request_options"></a><code>[constructor]request-options: func</code></h4>
<p>Creates a new request options resource with no options set.</p>
<h5>Return values</h5>
<ul>
<li><a id="constructor_request_options.0"></a> own&lt;<a href="#request_options"><a href="#request_options"><code>request-options</code></a></a>&gt;</li>
</ul>
<h4><a id="method_request_options_set_timeout_ms"></a><code>[method]request-options.set-timeout-ms: func</code></h4>
<p>The maximum amount of time to wait for a response. If the timeout value is not set, then
the request/reply operation will block until a message is received in response.</p>
<h5>Params</h5>
<ul>
<li><a id="method_request_options_set_timeout_ms.self"></a><code>self</code>: borrow&lt;<a href="#request_options"><a href="#request_options"><code>request-options</code></a></a>&gt;</li>
<li><a id="method_request_options_set_timeout_ms.timeout_ms"></a><code>timeout-ms</code>: <code>u32</code></li>
</ul>
<h4><a id="method_request_options_set_expected_replies"></a><code>[method]request-options.set-expected-replies: func</code></h4>
<p>The maximum number of replies to expect before returning.</p>
<h5>Params</h5>
<ul>
<li><a id="method_request_options_set_expected_replies.self"></a><code>self</code>: borrow&lt;<a href="#request_options"><a href="#request_options"><code>request-options</code></a></a>&gt;</li>
<li><a id="method_request_options_set_expected_replies.expected_replies"></a><code>expected-replies</code>: <code>u32</code></li>
</ul>
<h4><a id="request"></a><code>request: func</code></h4>
<p>Performs a blocking request/reply operation with an optional set of request options.</p>
<p>The behavior of this function is largely dependent on the options given to the function.
If no options are provided, then the request/reply operation will block until a single
message is received in response. If a timeout is provided, then the request/reply operation
will block for the specified amount of time before returning an error if no messages were
received (or the list of messages that were received). If both a timeout and an expected
number of replies are provided, the function should return when either condition is met
(whichever comes first)—e.g., (1) if no replies were received within the timeout return an
error, (2) if the maximum expected number of replies were received before timeout, return
the list of messages, or (3) if the timeout is reached before the expected number of replies,
return the list of messages received up to that point.</p>
<h5>Params</h5>
<ul>
<li><a id="request.c"></a><code>c</code>: borrow&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
<li><a id="request.message"></a><a href="#message"><code>message</code></a>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a id="request.options"></a><code>options</code>: option&lt;own&lt;<a href="#request_options"><a href="#request_options"><code>request-options</code></a></a>&gt;&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="request.0"></a> result&lt;list&lt;own&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;&gt;, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="reply"></a><code>reply: func</code></h4>
<p>Replies to the given message with the given response message. The details of which topic
the message is sent to is up to the implementation. This allows for reply-to details to be
handled in the best way possible for the underlying messaging system.</p>
<p>Please note that this reply functionality is different than something like HTTP because there
are several use cases in which a reply might not be required for every message (so this would
be a noop). There are also cases when you might want to reply and then continue processing.
Additionally, you might want to reply to a message several times (such as providing an
update). So this function is allowed to be called multiple times, unlike something like HTTP
where the reply is sent and the connection is closed.</p>
<h5>Params</h5>
<ul>
<li><a id="reply.reply_to"></a><code>reply-to</code>: borrow&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
<li><a id="reply.message"></a><a href="#message"><code>message</code></a>: own&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="reply.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h2><a id="wasi_messaging_producer_0_2_0_draft"></a>Import interface wasi:messaging/producer@0.2.0-draft</h2>
<p>The producer interface is used to send messages to a channel/topic.</p>
<hr />
<h3>Types</h3>
<h4><a id="client"></a><code>type client</code></h4>
<p><a href="#client"><a href="#client"><code>client</code></a></a></p>
<p>
#### <a id="message"></a>`type message`
[`message`](#message)
<p>
#### <a id="error"></a>`type error`
[`error`](#error)
<p>
#### <a id="topic"></a>`type topic`
[`topic`](#topic)
<p>
----
<h3>Functions</h3>
<h4><a id="send"></a><code>send: func</code></h4>
<p>Sends the message using the given client.</p>
<h5>Params</h5>
<ul>
<li><a id="send.c"></a><code>c</code>: borrow&lt;<a href="#client"><a href="#client"><code>client</code></a></a>&gt;</li>
<li><a id="send.topic"></a><a href="#topic"><code>topic</code></a>: <a href="#topic"><a href="#topic"><code>topic</code></a></a></li>
<li><a id="send.message"></a><a href="#message"><code>message</code></a>: own&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="send.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h2><a id="wasi_messaging_incoming_handler_0_2_0_draft"></a>Export interface wasi:messaging/incoming-handler@0.2.0-draft</h2>
<hr />
<h3>Types</h3>
<h4><a id="message"></a><code>type message</code></h4>
<p><a href="#message"><a href="#message"><code>message</code></a></a></p>
<p>
#### <a id="error"></a>`type error`
[`error`](#error)
<p>
#### <a id="topic"></a>`type topic`
[`topic`](#topic)
<p>
----
<h3>Functions</h3>
<h4><a id="handle"></a><code>handle: func</code></h4>
<p>Whenever this guest receives a message in one of the subscribed topics, the message is
sent to this handler. The guest is responsible for matching on the topic and handling the
message accordingly. Implementors (such as hosts) calling this interface should make their
own decisions on how to handle errors returned from this function.</p>
<h5>Params</h5>
<ul>
<li><a id="handle.message"></a><a href="#message"><code>message</code></a>: own&lt;<a href="#message"><a href="#message"><code>message</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="handle.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="get_topics"></a><code>get-topics: func</code></h4>
<p>Returns a list of topics (represented as <code>string</code>s) at runtime the guest should be subscribed
Implementors should consider also allowing subscriptions to be made at compile time
via some sort of configuration file. This function is intended to be called at the start of the
guest's lifecycle before any messages are sent.</p>
<h5>Return values</h5>
<ul>
<li><a id="get_topics.0"></a> result&lt;list&lt;<a href="#topic"><a href="#topic"><code>topic</code></a></a>&gt;, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
