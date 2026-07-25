# Giriş

**input** işlevleri, kullanıcı adına girdi göndermenize olanak tanır.

---

## isrbxactive

```lua
function isrbxactive(): boolean
```

Oyunun penceresinin odakta olup olmadığını döndürür. Diğer giriş işlevlerinin çalışması için doğru olması gerekir.

### Takma adlar

 * `isgameactive`

### Örnek

```lua
if isrbxactive() then
	mouse1click()
end
```

---

## fare1 tıklaması

```lua
function mouse1click(): ()
```

Sol fare düğmesi tıklamasını gönderir.

---

## fare1 tuşuna basın

```lua
function mouse1press(): ()
```

Sol fare düğmesine basmayı gönderir.

---

## fare1serbest bırakma

```lua
function mouse1release(): ()
```

Sol fare düğmesinin serbest bırakılmasını gönderir.

---

## fare2tıklama

```lua
function mouse2click(): ()
```

Sağ fare düğmesi tıklamasını gönderir.

---

## fare2press

```lua
function mouse2press(): ()
```

Sağ fare düğmesine basmayı gönderir.

---

## fare2release

```lua
function mouse2release(): ()
```

Sağ fare düğmesinin serbest bırakılmasını gönderir.

---

## fare hareketleri

```lua
function mousemoveabs(x: number, y: number): ()
```

Fare imlecini belirtilen mutlak konuma taşır.

### Parametreler

 * `x` - The x-coordinate of the mouse cursor.
 * `y` - The y-coordinate of the mouse cursor.

### Örnek

İmleci ekranın etrafında bir daire içinde hareket ettirin:

```lua
-- Wait for the game window to be selected
while not isrbxactive() do
	task.wait()
end

local size = workspace.CurrentCamera.ViewportSize
	
for i = 0, 50 do
	local x = math.sin(i / 50 * math.pi * 2) / 2 + 0.5
	local y = math.cos(i / 50 * math.pi * 2) / 2 + 0.5
	mousemoveabs(x * size.X, y * size.Y)
	task.wait(0.05)
end
```

---

## fare hareket ettirici

```lua
function mousemoverel(x: number, y: number): ()
```

Fare imlecini belirtilen göreli miktara göre ayarlar.

### Parametreler

 * `x` - The x-offset of the mouse cursor.
 * `y` - The y-offset of the mouse cursor.

### Örnek

İmleci küçük bir daire içinde hareket ettirir:

```lua
-- Wait for the game window to be selected
while not isrbxactive() do
	task.wait()
end

for i = 0, 20 do
	local x = math.sin(i / 20 * math.pi * 2)
	local y = math.cos(i / 20 * math.pi * 2)
	mousemoverel(x * 100, y * 100)
	task.wait(0.05)
end
```

---

## fare kaydırma

```lua
function mousescroll(pixels: number): ()
```

Belirtilen piksel sayısına göre bir fare kaydırması gönderir.

### Parametreler

 * `pixels` - The number of pixels to scroll.