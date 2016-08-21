# tWebPush is neutral version of PyWebPush for http client library

It is small fork for developers who want to use WebPush without request library.
In my use case I use Tornado framework for async push.

# Webpush Data encryption library for Python

This is a work in progress.
This library is available on [pypi as
pywebpush](https://pypi.python.org/pypi/pywebpush).
Source is available on [github](https://github.com/jrconlin/pywebpush)

## Installation

You'll need to run `python virtualenv`.
Then
```
bin/pip install -r requirements.txt
bin/python setup.py develop
```

## Usage

In the browser, the promise handler for
[registration.pushManager.subscribe()](https://developer.mozilla.org/en-US/docs/Web/API/PushManager/subscribe)
returns a
[PushSubscription](https://developer.mozilla.org/en-US/docs/Web/API/PushSubscription)
object. This object has a .toJSON() method that will return a JSON
object that contains all the info we need to encrypt and push data.

As illustration, a subscription info object may look like:
```
{"endpoint": "https://updates.push.services.mozilla.com/push/v1/gAA...", "keys": {"auth": "k8J...", "p256dh": "BOr..."}}
```

How you send the PushSubscription data to your backend, store it
referenced to the user who requested it, and recall it when there's
new a new push subscription update is left as an excerise for the
reader.

The data can be any serial content (string, bit array, serialized
JSON, etc), but be sure that your receiving application is able to
parse and understand it. (e.g. `data = "Mary had a little lamb."`)

gcm_key is the API key obtained from the Google Developer Console.
It is only needed if endpoint is
https://android.googleapis.com/gcm/send

`headers` is a `dict`ionary of additional HTTP header values (e.g.
[VAPID](https://github.com/mozilla-services/vapid/tree/master/python)
self identification headers). It is optional and may be omitted.

get data:
```
endpoint, body, headers = WebPusher(subscription_info).send(data, headers)
```
get data for Chrome:
```
endpoint, body, headers = WebPusher(subscription_info).send(data, headers, ttl, gcm_key)
```

You can also simply encode the data to send later by calling (but without headers and etc)

```
encoded = WebPush(subscription_info).encode(data)
```
