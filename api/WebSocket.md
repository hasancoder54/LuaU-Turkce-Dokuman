# WebSocket

**WebSocket** sınıfı, WebSocket bağlantısı üzerinden veri gönderip almak için basit bir arayüz sağlar.

---

## WebSocket.connect

`🏛️ Constructor`

```lua
function WebSocket.connect(url: string): WebSocket
```

Belirtilen URL'ye WebSocket bağlantısı kurar.

### Parametreler

 * `url` - The URL to connect to.

### Örnek

```lua
local ws = WebSocket.connect("ws://localhost:8080")

ws.OnMessage:Connect(function(message)
	print(message)
end)

ws.OnClose:Connect(function()
	print("Closed")
end)

ws:Send("Hello, World!")
```

---

## WebSocket

`🖥️ Class`

```lua
ws = WebSocket.connect(url)
```

### Yöntemler

| Yöntem | Açıklama |
| ------ | ----------- |
| `Send(message: string): ()` | Sends a message over the WebSocket connection. |
| `Close(): ()` | Closes the WebSocket connection. |

### Olaylar

| Etkinlik | Açıklama |
| ----- | ----------- |
| `OnMessage(message: string): ()` | Fired when a message is received over the WebSocket connection. |
| `OnClose(): ()` | Fired when the WebSocket connection is closed. |
