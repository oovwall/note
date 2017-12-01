# WebSocket 火拼俄罗斯

### WebSocket 简单Demo
```javascript
var websocket = new WebSocket('ws://echo.websocket.org')
websocket.onopen = function () {
  console.log('websocket open')
}
websocket.onclose = function () {
  console.log('websocket close')
}
websocket.onmessage = function (e) {
  console.log(e.data)
  document.getElementById('recv').innerHTML = e.data
}
document.getElementById('sendBtn').onclick = function () {
  var txt = document.getElementById('sendTxt').value
  websocket.send(txt)
}
```

