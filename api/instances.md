# Örnekler

**Örnek** işlevleri, oyun nesneleri ve bunların özellikleriyle etkileşimde bulunmak için kullanılır.

---

## Fireclickdedektör

```lua
function fireclickdetector(object: ClickDetector, distance: number?, event: string?): ()
```

Dispatches a click or hover event to the given ClickDetector. When absent, `distance` defaults to zero, and `event` defaults to "MouseClick".

Olası giriş olayları arasında 'MouseClick', 'RightMouseClick', 'MouseHoverEnter' ve 'MouseHoverLeave' yer alır.

### Parametreler

 * `object` - The ClickDetector to dispatch to.
 * `distance` - Optional distance to the object.
 * `event` - Optional event to fire.

### Örnek

```lua
local clickDetector = workspace.Door.Button.ClickDetector
fireclickdetector(clickDetector, 10 + math.random(), "MouseClick")
```

---

## geri arama değerini al

```lua
function getcallbackvalue(object: Instance, property: string): function?
```

Returns the function assigned to a callback property of `object`, which cannot be indexed normally.

### Parametreler

 * `object` - The object to get the callback property from.
 * `property` - The name of the callback property.

### Örnek

```lua
local bindable = Instance.new("BindableFunction")

function bindable.OnInvoke()
	print("Hello, world!")
end

print(getcallbackvalue(bindable, "OnInvoke")) --> function()
print(bindable.OnInvoke) --> Throws an error
```

---

## bağlantıları edinin

```lua
function getconnections(signal: RBXScriptSignal): {Connection}
```

Creates a list of Connection objects for the functions connected to `signal`.

### Bağlantı

| Alan | Tür | Açıklama |
| ----- | ---- | ----------- |
| `Enabled` | boolean | Whether the connection can receive events. |
| `ForeignState` | boolean | Whether the function was connected by a foreign Luau state (i.e. CoreScripts). |
| `LuaConnection` | boolean | Whether the connection was created in Luau code. |
| `Function` | function? | The function bound to this connection. Nil when `ForeignState` is true. |
| `Thread` | thread? | The thread that created the connection. Nil when `ForeignState` is true. |

| Yöntem | Açıklama |
| ----- | ----------- |
| `Fire(...: any): ()` | Fires this connection with the provided arguments. |
| `Defer(...: any): ()` | [Defers](https://devforum.roblox.com/t/beta-deferred-lua-event-handling/1240569) an event to connection with the provided arguments. |
| `Disconnect(): ()` | Disconnects the connection. |
| `Disable(): ()` | Prevents the connection from firing. |
| `Enable(): ()` | Allows the connection to fire if it was previously disabled. |

### Parametreler

 * `signal` - The signal to retrieve connections from.

### Örnek

```lua
local connections = getconnections(game.DescendantAdded)

for _, connection in ipairs(connections) do
	connection:Disable()
end
```

---

## özel varlık al

```lua
function getcustomasset(path: string, noCache: boolean): string
```

Returns a `rbxasset://` content id for the asset located at `path`, allowing you to use unmoderated assets. Internally, files are copied to the game's content directory.

If `noCache` is false, the file will be cached, allowing subsequent calls to `getcustomasset` to return the same content id.

### Parametreler

 * `path` - The path to the asset.
 * `noCache` - Whether or not to cache the asset.

### Örnek

```lua
local image = Instance.new("ImageLabel")
image.Image = getcustomasset("image.png")
print(image.Image) --> rbxasset://nTYyO6iSF3mND4FJ/image.png
```

---

## gethiddenproperty

```lua
function gethiddenproperty(object: Instance, property: string): (any, boolean)
```

Returns the value of a hidden property of `object`, which cannot be indexed normally.

If the property is hidden, the second return value will be `true`. Otherwise, it will be `false`.

### Parametreler

 * `object` - The object to index.
 * `property` - The name of the hidden property.

### Örnek

```lua
local fire = Instance.new("Fire")
print(gethiddenproperty(fire, "size_xml")) --> 5, true
print(gethiddenproperty(fire, "Size")) --> 5, false
```

---

## gethui

```lua
function gethui(): Folder
```

Gizli bir GUI kapsayıcısını döndürür. CoreGui ve PlayerGui'ye alternatif olarak kullanılmalıdır.

Bu kapsayıcının üst öğesi olan GUI nesneleri, yaygın algılama yöntemlerinden korunacaktır.

### Example

```lua
local gui = game:GetObjects("rbxassetid://1234")[1]
gui.Parent = gethui()
```

---

## örnek alma

```lua
function getinstances(): {Instance}
```

İstemcide başvurulan her Örneğin listesini döndürür.

### Example

```lua
local objects = getinstances()

local gameCount = 0
local miscCount = 0

for _, object in ipairs(objects) do
	if object:IsDescendantOf(game) then
		gameCount += 1
	else
		miscCount += 1
	end
end

print(gameCount) --> The number of objects in the `game` hierarchy.
print(miscCount) --> The number of objects outside of the `game` hierarchy.
```

---

## örnek alma

```lua
function getnilinstances(): {Instance}
```

Like `getinstances`, but only includes Instances that are not descendants of a service provider.

### Örnek

```lua
local objects = getnilinstances()

for _, object in ipairs(objects) do
	if object:IsA("LocalScript") then
		print(object, "is a LocalScript")
	end
end
```

---

## yazılabilir

`🪲 Compatibility`

```lua
function isscriptable(object: Instance, property: string): boolean
```

Returns whether the given property is scriptable (does not have the `notscriptable` tag).

If `true`, the property is scriptable and can be indexed normally. If `nil`, the property does not exist.

> ### 🪲 Known Issues
> Script-Ware'de bu durum geriye doğru görünüyor. Davranış tutarlı olana kadar örnek verilmeyecektir.

### Parametreler

 * `object` - The object to index.
 * `property` - The name of the property.

---

## gizli özellik ayarla

```lua
function sethiddenproperty(object: Instance, property: string, value: any): boolean
```

Sets the value of a hidden property of `object`, which cannot be set normally. Returns whether the property was hidden.

### Parametreler

 * `object` - The object to index.
 * `property` - The name of the hidden property.
 * `value` - The value to set.

### Example

```lua
local fire = Instance.new("Fire")
print(sethiddenproperty(fire, "Size", 5)) --> false (not hidden)
print(sethiddenproperty(fire, "size_xml", 15)) --> true (hidden)
print(gethiddenproperty(fire, "size_xml")) --> 15, true (hidden)
```

---

## setrbxclipboard

```lua
function setrbxclipboard(data: string): boolean
```

Sets the Studio client's clipboard to the given `rbxm` or `rbxmx` model data. This allows data from the game to be copied into a Studio client.

### Parametreler

 * `data` - The model data to copy to the clipboard.

### Example

```lua
local data = readfile("model.rbxm")
setrbxclipboard(data) -- Can be pasted into Studio
```

---

## ayarlanabilir

`🪲 Compatibility`

```lua
function setscriptable(object: Instance, property: string, value: boolean): boolean
```

Verilen özelliğin kodlanabilir olup olmadığını ayarlayın. Özelliğin değiştirilmeden önce kodlanabilir olup olmadığını döndürür.

> ### 🪲 Bilinen Sorunlar
> This appears to be backwards on Script-Ware. An example will not be provided until behavior is consistent.

### Parametreler

 * `object` - The object to index.
 * `property` - The name of the property.
 * `value` - Whether the property should be scriptable.