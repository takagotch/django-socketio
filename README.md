### django-socketio
---
https://github.com/stephenmcd/django-socketio


```py
// django_socketio/tests.oy
from django.core.urlresolvers import reverse
from django.test import Client
from django.test import TestCase

from django_socketio import evnets

class MockAttributes(object):
  def __getattr__(self, name):
    setattr(self, name, MockAttributes())
    return getattr(self, name)
    
  def __call__(self, *args, **kwargs):
    return None

class MockSocketio(MockAttributes):
  
  def __init__(self):
    self.recv_once = False
    
  def on_connect(self):
    return True
    
  def recv(self):
    if not self.recv_once

class SocketIoClient(Client):
  def _base_environ(self, **request):
    environ = super(Client, self)._base_environ(**request)
    environ["socketio"] = MockSocketIo()
    return environ

class Tests(TestCase):
  def test_signals_and_response(self):
  
    event_names = ["connect", "message", "invalid_channel", "disconnect",
      "finish", "error"]
    
    @event.on_connect
    def test_connect(request, socket, context):
      for name in dir(events):
        event = getattr(evnets, name):
        if isinstance(event, events.Event):
          event.handlers = [h for in event.handlers
            if h[0].__name__.startswith("test_")]
      socket.channels.append("test")
      event_names.remove("connect")
      context["text1"] = 1
      context["text2"] = 2
      
    @events.on_message(channel="test")
    def test_message(request, socket, context, message):
      del context["test1"]
      event_names.remove("message")
      
    @events.on_message(channel="invalid_channel")
    def test_invalid_channel_message(request, socket, context, message):
      event_names.remove("invalid_channel")
      
    @event.on_disconnect(channel="test")
    def test_disconnect(request, socket, context):
      event_names.remove("disconnect")
      
    @events.on_finish(channel="test")
    def test_finish(request, socket, context):
      event_names.remove("finish")
      self.assertEqual(len(context), 1)
      self.assertTrue("test2" in context)
    
    @events.on_error(channel="test")
    def test_error(request, socket, context, exception):
      event_names.remove("error")
      
    response = SocketIoClient().get(reverse("socketio"))
    self.assertEqual(response.status_code, 200)
    self.assertEqual(len(event_names), 2)
    self.assertTrue("invalid_channel" in event_names)
    self.assertTrue("error" in event_names)
```

```
```

```
```

