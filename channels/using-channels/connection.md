---
title: Connection — Channels — Pusher Docs
layout: channels.njk
eleventyNavigation:
  parent: Using Channels
  key: Connection
---

{% section %}

# Connection

The Channels connection is the fundamental means of communication with the service. It is a bi-directional connection and is able to receive messages as well as emit messages from the server.

{% endsection %}
{% section %}
{% example %}

```js
var pusher = new Pusher("APP_KEY", options);
```

{% endexample %}
{% article %}

## Connecting to Channels

When you create a new `Pusher` object you are automatically connected to Channels.

- `applicationKey` (String) - The application key is a string which is globally unique to your application. It can be found in the API Access section of your application within the Channels user dashboard.
- `options` (Object) _optional_ - See Channels `options` parameter below.

### Channels 'options' parameter

The `options` parameter on the `Pusher` constructor is an optional parameter used to apply configuration on a newly created `Pusher` instance.

```js
{
  cluster: 'APP_CLUSTER',
  forceTLS: true,
  auth: {
    params: {
      param1: 'value1',
      param2: 'value2'
    },
    headers: {
      header1: 'value1',
      header2: 'value2'
    }
  }
}
```

The options are:

#### forceTLS (Boolean)

It’s possible to define if the connection should be made over TLS. For more information see Encrypting connections

#### authEndpoint (String)

Endpoint on your server that will return the authentication signature needed for private and presence channels. Defaults to `'/pusher/auth'`.

For more information see authenticating users.

> If authentication fails a `subscription_error` event is triggered on the channel. For more information see handling authentication problems.

#### authTransport (String)

Defines how the authentication endpoint, defined using `authEndpoint`, will be called. There are two options available:

- **ajax** - The **default** option where an `XMLHttpRequest` object will be used to make a request. The parameters will be passed as `POST` parameters.
- **jsonp** - The authentication endpoint will be called by a `<script>` tag being dynamically created pointing to the endpoint defined by `authEndpoint`. This can be used when the authentication endpoint is on a different domain to the web application. The endpoint will therefore be requested as a `GET` and parameters passed in the query string.

For more information see the Channel authentication transport section of the authenticating users docs.

#### auth (Object)

> This feature was introduced in **version 1.12** of the Channels JavaScript library.

The auth option lets you send additional information with the authentication request. See authenticating users.

When creating a `Pusher` instance the `options` parameter can have an `auth` property set as follows:

```js
var pusher = new Pusher("app_key", {
  auth: {
    params: {
      CSRFToken: "some_csrf_token",
    },
  },
});
```

#### auth.params (Object)

Additional parameters to be sent when the channel authentication endpoint is called. When using ajax authentication the parameters are passed as additional `POST` parameters. When using jsonp authentication the parameters are passed as `GET` parameters. This can be useful with web application frameworks that guard against [CSRF (Cross-site request forgery)](http://en.wikipedia.org/wiki/Cross-site_request_forgery).

#### auth.headers (Object)

**Only applied when using ajax authentication**

Provides the ability to pass additional HTTP Headers to the channel authentication endpoint when authenticating a channel. This can be useful with some web application frameworks that guard against CSRF [CSRF (Cross-site request forgery)](http://en.wikipedia.org/wiki/Cross-site_request_forgery).

```js
var pusher = new Pusher("app_key", {
  auth: {
    headers: {
      "X-CSRF-Token": "some_csrf_token",
    },
  },
});
```

#### cluster (String)

The identifier of the cluster your application was created in. When not supplied, will connect to the `mt1` (`us-east-1`) cluster.

```js
// will connect to the 'eu' cluster
var pusher = new Pusher("app_key", { cluster: "eu" });
```

This parameter is mandatory when the app is created in a any cluster except `mt1` (`us-east-1`). Read more about configuring clusters.

#### disableStats (Boolean)

Disables stats collection, so that connection metrics are not submitted to Pusher’s servers.

#### enabledTransports (Array)

Specifies which transports should be used by Channels to establish a connection. Useful for applications running in controlled, well-behaving environments. Available transports: `ws`, `wss`, `xhr_streaming`,` xhr_polling`, `sockjs`. Additional transports may be added in the future and without adding them to this list, they will be disabled.

#### disabledTransports (Array)

Specifies which transports must not be used by Channels to establish a connection. Useful for applications running in controlled, well-behaving environments. Available transports: `ws`, `wss`, `xhr_streaming`,` xhr_polling`, `sockjs`. Additional transports may be added in the future and without adding them to this list, they will be disabled.

#### wsHost, wsPort, wssPort, httpHost, httpPort, httpsPort

These can be changed to point to alternative Channels URLs (used internally for our staging server).

#### ignoreNullOrigin (Boolean)

Ignores null origin checks for HTTP fallbacks. Use with care, it should be disabled only if necessary (i.e. PhoneGap).

#### activityTimeout (Integer)

After this time (in milliseconds) without any messages received from the server, a ping message will be sent to check if the connection is still working. Default value is supplied by the server, low values will result in unnecessary traffic.

#### pongTimeout (Integer)

After sending a ping message to the server, the client waits `pongTimeout` milliseconds for a response before assuming the connection is dead and closing it. Default value is supplied by the server.

### TLS connections

A Channels client can be configured to only connect over TLS connections. An application that uses TLS should use this option to ensure connection traffic is encrypted. It is also possible to force connections to use TLS by enabling the **Force TLS** setting within an application's dashboard settings.

The option to force TLS is available on all plans.

```js
var pusher = new Pusher("app_key", { forceTLS: true });
```

{% endarticle %}

{% endsection %}
