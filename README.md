
# NEWS - Nim Easy WebSocket.

* Based on https://github.com/treeform/ws
* Support `asyncdispatch` and https://github.com/status-im/nim-chronos

## Example Echo Server:

Example echo server, will repeat what you send it:

```nim
import news, asyncdispatch, asynchttpserver

var server = newAsyncHttpServer()
proc cb(req: Request) {.async.} =
  if req.url.path == "/ws":
    var ws = await newWebsocket(req)
    await ws.send("Welcome to simple echo server")
    while ws.readyState == Open:
      let packet = await ws.receivePacket()
      await ws.send(packet)
  await req.respond(Http200, "Hello World")

asyncCheck server.serve(Port(9001), cb)
runForever()
```

## Websocket client
Send messages to Echo server and receive unswer
```nim
import news, asyncdispatch

proc sendMsg() {.async.} =
    var ws = await newWebSocket("ws://localhost:9001/ws")
    await ws.send("hi")
    while ws.readyState == Open:
        let packet = await ws.receivePacket()
        echo "received ", packet

asyncCheck sendMsg()
runForever()
```

## Websocket with chronos support:
```nim
import chronos

const newsUseChronos = true
include news

proc sendMsg() {.async.} =
    var ws = await newWebSocket("ws://localhost:9001/ws")
    await ws.send("hi")
    while ws.readyState == Open:
        let packet = await ws.receivePacket()
        echo "received ", packet

asyncCheck sendMsg()
runForever()
```
