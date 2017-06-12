+++
date = "2017-06-11T15:02:32+01:00"
draft = false
title = "Protect your Python app from Timeouts"
categories = ["Coding"]
+++

In this post, I show how to better protect your python service when making http calls to other services by guaranteeing a timeout by monkey-patching the *requests* library.

Services tend to talk to other services. These services might be owned by you, another team at work, or made available by a third party (e.g. Google Maps API).

How tolerant is your service to the failure of one of these upstream services? Have you tested what will happen when one of these services isn't responding to your requests they way you expected?

I found this out the hard way recently when I deployed a new feature to my python service that required talking to another service. This upstream service was taking a long time to respond to my high volume of requests.

I use the [requests](https://github.com/requests/requests) library to call the APIs of other services. This library, has no default timeout value for requests that are taking too long. The consequence is that all of my service's resources were quickly exhausted and waiting for responses from this upstream service that were never going to arrive.

The result? My service pretty much ground to a halt.

Other similar libraries written for other languages do have a default timeout setting in place. For instance, my ruby services use [excon](https://github.com/excon/excon) which has a default timeout of 60 seconds. This is probably still too high for most cases but its very straightforward to set your own default

```
# config/initializers/excon.rb
Excon.defaults[:read_timeout] = ENV.fetch('EXCON_DEFAULT_TIMEOUT_SECONDS', 10).to_i
Excon.defaults[:write_timeout] = ENV.fetch('EXCON_DEFAULT_TIMEOUT_SECONDS', 10).to_i
Excon.defaults[:connect_timeout] = ENV.fetch('EXCON_DEFAULT_TIMEOUT_SECONDS', 10).to_i

```

As for the *requests* library...

> Most requests to external servers should have a timeout attached, in case the server is not responding in a timely manner. By default, requests do not time out unless a timeout value is set explicitly. Without a timeout, your code may hang for minutes or more. [link](http://docs.python-requests.org/en/master/user/advanced/#timeouts)

Eeek!

This means if you or another contributor forgets to add a `timeout` paramter to your requests library call you run this risk of a request never timing out (and hogging resources in doing so)!

Ensuring a default timeout whenever the *requests* library is used can be only be achieved (as of requests v.2.17.3 at least) using monkey patching.

I try to only use monkey patching as a last resort as it is brittle and harder than normal to debug but since there is no global constant or environment variable that can be overwritten we'll have to make do with monkey patching!

```
import requests
from requests.adapters import TimeoutSauce
import configuration


class GlobalDefaultTimeoutSauce(TimeoutSauce):
    # A subclass of TimeoutSauce that will use a
    # default timeout setting when overrides are not already specified

    def __init__(self, *args, **kwargs):
        default_timeout_seconds = configuration.REQUESTS_DEFAULT_TIMEOUT_S
        connect = kwargs.get('connect') or default_timeout_seconds
        read = kwargs.get('read') or default_timeout_seconds
        super(GlobalDefaultTimeoutSauce, self).__init__(connect=connect, read=read)


def monkey_patch_requests_timeout_strategy():
    # Subsequent usages of the requests library will use a default
    # timeout if none is specified by the caller.
    # Call me once during your app's init phase, before any requests are made.
    requests.adapters.TimeoutSauce = GlobalDefaultTimeoutSauce
```

To see how this is used take a look at the adapters module in the requests project. [link](https://github.com/requests/requests/blob/master/requests/adapters.py#L416)

Now I can boost the resiliency of my service from degredations of the services I depend on! Hope this helps someone else out!

