# Drawing

**Çizim** sınıfı, oyun penceresinin üzerinde şekil ve metin çizmek için bir arayüz sağlar.

---

## Çizim.yeni

`🏛️ Constructor`

```lua
function Drawing.new(type: string): Drawing
```

Belirtilen türde yeni bir çizim nesnesi oluşturun.

Olası türler 'Çizgi', 'Metin', 'Görüntü', 'Daire', 'Kare', 'Dörtlü' ve 'Üçgen'dir.

### Parametreler

 * `type` - The type of drawing object to create.

### Example

```lua
local circle = Drawing.new("Circle")
circle.Radius = 50
circle.Color = Color3.fromRGB(255, 0, 0)
circle.Filled = true
circle.NumSides = 32
circle.Position = Vector2.new(300, 300)
circle.Transparency = 0.7
circle.Visible = true

task.wait(1)
circle:Destroy()
```

---

## Çizim.Yazı Tipleri

`⭕ Static` `🔒 Read-only`

```lua
Drawing.Fonts: {
	UI: 0,
	System: 1,
	Plex: 2,
	Monospace: 3,
}
```

Kullanılabilir yazı tipi adlarını içeren bir tablo. Her yazı tipinin stili, uygulayıcıya bağlı olarak değişir.

### Fonts

<detaylar>
<summary>Yazı tipi tablosunu göster</summary>

> | Yürütücü | Yazı Tipleri |
> | --------- | ----- |
> | Script-Ware | ![Script-Ware Yazı Tipleri](../images/fonts-sw.png) |
> | Krnl | ![Krnl Yazı Tipleri](../images/fonts-krnl.png) |
</detaylar>

### Example

```lua
for name, font in pairs(Drawing.Fonts) do
	local text = Drawing.new("Text")
	text.Text = "The quick brown fox (" .. name .. ")"
	text.Font = font
	text.Size = 48
	text.Position = Vector2.new(150, 100 + font * 50)
	text.Visible = true
	task.delay(2, function ()
		text:Destroy()
	end)
end
```

---

## Çizim

`🖥️ Class`

```lua
drawing = Drawing.new(type)
```

### Temel Çizim

Tüm çizim nesnelerinin miras aldığı temel sınıf. Örneklenemez.

| Emlak | Tür | Açıklama |
| -------- | ---- | ----------- |
| `Visible` | boolean | Whether the drawing is visible. Defaults to `false` on some executors. |
| `ZIndex` | number | Determines the order in which a Drawing renders relative to other drawings. |
| `Transparency` | number | The opacity of the drawing (1 is opaque, 0 is transparent). |
| `Color` | Color3 | The color of the drawing. |
| `Destroy(): ()` | function | Destroys the drawing. |

### Astar

Renders a line starting at `From` and ending at `To`.

| Emlak | Tür | Açıklama |
| -------- | ---- | ----------- |
| `From` | Vector2 | The starting point of the line. |
| `To` | Vector2 | The ending point of the line. |
| `Thickness` | number | The thickness of the line. |

### Metin

Renders text at `Position`.

| Emlak | Tür | Açıklama |
| -------- | ---- | ----------- |
| `Text` | string | The text to render. |
| `TextBounds` | 🔒 Vector2 | The size of the text. Cannot be set. |
| `Font` | Drawing.Font | The font to use. |
| `Size` | number | The size of the text. |
| `Position` | Vector2 | The position of the text. |
| `Center` | boolean | Whether the text should be centered horizontally. |
| `Outline` | boolean | Whether the text should be outlined. |
| `OutlineColor` | Color3 | The color of the outline. |

### Resim

Draws the image data to the screen. `Data` *must* be the raw image data.

| Property | Type | Description |
| -------- | ---- | ----------- |
| `Data` | string | The raw image data. |
| `Size` | Vector2 | The size of the image. |
| `Position` | Vector2 | The position of the image. |
| `Rounding` | number | The rounding of the image. |

### Daire

Draws a circle that is centered at `Position`.

This is not a perfect circle! The greater the value for `NumSides`, the more accurate the circle will be.

| Emlak | Tür | Açıklama |
| -------- | ---- | ----------- |
| `NumSides` | number | The number of sides of the circle. |
| `Radius` | number | The radius of the circle. |
| `Position` | Vector2 | The position of the center of the circle. |
| `Thickness` | number | If `Filled` is false, specifies the thickness of the outline. |
| `Filled` | boolean | Whether the circle should be filled. |

### Kare

Draws a rectangle starting at `Position` and ending at `Position` + `Size`.

| Emlak | Tür | Açıklama |
| -------- | ---- | ----------- |
| `Size` | Vector2 | The size of the square. |
| `Position` | Vector2 | The position of the top-left corner of the square. |
| `Thickness` | number | If `Filled` is false, specifies the thickness of the outline. |
| `Filled` | boolean | Whether the square should be filled. |

### Dörtlü

Dört noktanın her birine bağlanan dört kenarlı bir şekil çizer.

| Emlak | Tür | Açıklama |
| -------- | ---- | ----------- |
| `PointA` | Vector2 | The first point. |
| `PointB` | Vector2 | The second point. |
| `PointC` | Vector2 | The third point. |
| `PointD` | Vector2 | The fourth point. |
| `Thickness` | number | If `Filled` is false, specifies the thickness of the outline. |
| `Filled` | boolean | Whether the quad should be filled. |

### Üçgen

Draws a triangle connecting to each of the three points.

| Emlak | Tür | Açıklama |
| -------- | ---- | ----------- |
| `PointA` | Vector2 | The first point. |
| `PointB` | Vector2 | The second point. |
| `PointC` | Vector2 | The third point. |
| `Thickness` | number | If `Filled` is false, specifies the thickness of the outline. |
| `Filled` | boolean | Whether the triangle should be filled. |

---

## cleardrawcache

`🌎 Global`

```lua
function cleardrawcache(): ()
```

Önbellekteki tüm çizim nesnelerini yok eder. Çizim nesnelerine yapılan referansları geçersiz kılar.

### Örnek

```lua
for i = 1, 10 do
	local circle = Drawing.new("Circle")
	circle.Radius = 50
	circle.Color = Color3.fromRGB(255, 0, 0)
	circle.Filled = true
	circle.NumSides = 32
	circle.Position = Vector2.new(math.random(300, 1200), math.random(300, 1200))
	circle.Transparency = 0.7
	circle.Visible = true
end

task.wait(1)
cleardrawcache()
```

---

## getrenderproperty

`🌎 Global`

```lua
function getrenderproperty(drawing: Drawing, property: string): any
```

Gets the value of a property of a drawing. Functionally identical to `drawing[property]`.

### Parametreler

 * `drawing` - The drawing to get the property of.
 * `property` - The property to get.

### Örnek

```lua
local circle = Drawing.new("Circle")
getrenderproperty(circle, "Color")
```

---

## isrenderobj

`🌎 Global`

```lua
function isrenderobj(object: any): boolean
```

Verilen nesnenin geçerli bir Çizim olup olmadığını döndürür.

### Parametreler

 * `object` - Any object.

### Örnek

```lua
print(isrenderobj(Drawing.new("Circle"))) --> true
print(isrenderobj({})) --> false
```

---

## setrenderözelliği

`🌎 Global`

```lua
function setrenderproperty(drawing: Drawing, property: string, value: any): ()
```

Sets the value of a property of a drawing. Functionally identical to `drawing[property] = value`.

### Parametreler

 * `drawing` - The drawing to set the property of.
 * `property` - The property to set.
 * `value` - The value to set the property to.

### Örnek

```lua
local circle = Drawing.new("Circle")
setrenderproperty(circle, "Color", Color3.fromRGB(255, 0, 0))
```