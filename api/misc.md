# Çeşitli

**Çeşitli** işlevler, henüz kategorize edilmemiş işlevlerin geçici bir koleksiyonudur.

---

## Yöneticiyi tanımla

```lua
function identifyexecutor(): (string, string)
```

Geçerli yürütücünün adını ve sürümünü döndürür.

### Takma adlar

 * `getexecutorname`

---

## lz4compress

```lua
function lz4compress(data: string): string
```

Compresses `data` using LZ4 compression.

### Parametreler

 * `data` - The uncompressed data.

### Örnek

```lua
local text = "Hello, world! Hello, world! Goodbye, world!"
print(#text) --> 43
print(#lz4compress(text)) --> 34
```

---

## lz4decompress

```lua
function lz4decompress(data: string, size: number): string
```

Decompresses `data` using LZ4 compression, with the decompressed size specified by `size`.

### Parametreler

 * `data` - The compressed data.
 * `size` - The size of the decompressed data.

### Örnek

```lua
local text = "Hello, world! Hello, world!"
local compressed = lz4compress(text)
print(lz4decompress(compressed, #text)) --> "Hello, world! Hello, world!"
```

---

## mesaj kutusu

`⏰ Yields`

```lua
function messagebox(text: string, caption: string, flags: number): number
```

Belirtilen metni, resim yazısını ve bayrakları içeren bir mesaj kutusu oluşturur. Mesaj kutusu kapatılıncaya kadar devam eder ve kullanıcı giriş kodunu döndürür.

İşaretler ve dönüş kodlarıyla ilgili belgeleri [burada](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-messagebox) bulabilirsiniz.

### Parametreler

 * `text` - The text to display in the message box.
 * `caption` - The caption of the message box.
 * `flags` - The flags to use.

### Örnek

Kullanıcıya üç seçenek ve bir uyarı simgesi içeren bir mesaj kutusuyla bilgi verir:

```lua
local MB_ICONWARNING = 0x00000030
local MB_CANCELTRYCONTINUE = 0x00000006
local MB_DEFBUTTON2 = 0x00000100

local IDCANCEL = 0x00000002
local IDTRYAGAIN = 0x00000004
local IDCONTINUE = 0x00000005

local input = messagebox(
	"Resource not available\nDo you want to try again?",
	"Resource not found",
	bit32.bor(MB_ICONWARNING, MB_CANCELTRYCONTINUE, MB_DEFBUTTON2)
)

if input == IDCANCEL then
	print("Canceled")
elseif input == IDTRYAGAIN then
	print("Try again")
elseif input == IDCONTINUE then
	print("Continue")
end
```

---

## kuyruk_on_teleport

```lua
function queue_on_teleport(code: string): ()
```

Oyuncu farklı bir yere ışınlandıktan sonra belirtilen betiği çalıştırılacak şekilde sıraya koyar.

### Parametreler

 * `code` - The script to execute.

### Takma adlar

 * `queueonteleport` - Will supercede this function in the future.

### Örnek

```lua
local source = game:GetObjects("rbxassetid://1234")[1].Source
queue_on_teleport(source)
loadstring(source)()
```

---

## rica etmek

`⏰ Yields`

```lua
function request(options: HttpRequest): HttpResponse
```

Belirtilen seçenekleri kullanarak bir HTTP isteği gönderir. İstek tamamlanana kadar sonuç verir ve yanıtı döndürür.

### Rica etmek

| Alan | Tür | Açıklama |
| ----- | ---- | ----------- |
| `Url` | string | The URL for the request. |
| `Method` | string | The HTTP method to use. Can be `GET`, `POST`, `PATCH`, or `PUT`. |
| `Body` | string? | The body of the request. |
| `Headers` | table? | A table of headers. |
| `Cookies` | table? | A table of cookies. |

### Cevap

| Alan | Tür | Açıklama |
| ----- | ---- | ----------- |
| `Body` | string | The body of the response. |
| `StatusCode` | number | The number status code of the response. |
| `StatusMessage` | string | The status message of the response. |
| `Success` | boolean | Whether or not the request was successful. |
| `Headers` | table | A dictionary of headers. |

### Başlıklar

Yürütücü, bir web sunucusunda tanımlama için aşağıdaki başlıkları sağlar:

| Başlık | Açıklama |
| ------ | ----------- |
| `PREFIX-User-Identifier` | A string unique to each user, and does not change if the script executor is used across computers. |
| `PREFIX-Fingerprint` | The hardware identifier of the user. |
| `User-Agent` | The name and version of the executor. |

### Parametreler

 * `options` - The options to use.

### Takma adlar

 * `http.request`
 * `http_request`

### Örnek

```lua
local response = request({
	Url = "http://example.com/",
	Method = "GET",
})

print(response.StatusCode .. " - " .. response.StatusMessage) --> 200 - HTTP/1.1 200 OK
```

---

## panoya koymak

```lua
function setclipboard(text: string): ()
```

Copies `text` to the clipboard.

### Parametreler

 * `text` - The text to copy.

### Takma adlar

 * `toclipboard`

### Örnek

```lua
local character = game:GetService("Players").LocalPlayer.Character
local components = table.pack(character.PrimaryPart.CFrame:GetComponents())
setclipboard("CFrame.new(" .. table.concat(components, ", ") .. ")")
```

---

## setfpscap

```lua
function setfpscap(fps: number): ()
```

Sets the in-game FPS cap to `fps`. If `fps` is 0, the FPS cap is disabled.

### Parametreler

 * `fps` - The FPS cap.

### Örnek

```lua
setfpscap(0) -- Unlocks the FPS cap
```