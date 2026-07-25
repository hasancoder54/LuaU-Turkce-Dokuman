# Konsol

**konsol** işlevleri bir konsol penceresiyle etkileşimde bulunmak için kullanılır.

Bu sayfada belgelenen davranış ve örnekler Script-Ware'e dayanmaktadır.

---

## rconsoleclear

```lua
function rconsoleclear(): ()
```

Konsol penceresinin çıktısını temizler.

### Takma adlar

 * `consoleclear`

### Örnek

```lua
-- Create the console window
rconsolesettitle("New console")
rconsoleprint("Hello, world!")
rconsolecreate()

-- Clears the output "Hello, world!"
rconsoleclear()
```

---

## rconsolecreate

```lua
function rconsolecreate(): ()
```

Konsol penceresini açar. Daha önce konsola gönderilen metinler silinmeyecektir.

> ### 🔎 Not
> Some executors also allow functions like `rconsoleprint` to open the console.\
> Bu, güvenilmemesi gereken kafa karıştırıcı bir davranıştır.

### Takma adlar

 * `consolecreate`

### Örnek

Dağlık bir manzara oluşturan bir program oluşturun:

```lua
-- Create the console window
rconsolesettitle("Beautiful Mountains")
rconsolecreate()

local function generate()
	-- Generate a random decimal number for noise
	local seed = math.random(100, 999) + math.random()

	-- Prints 25 lines of text
	for i = 1, 25 do
		local noise = math.noise(i / 8, seed) + 0.5
		local height = math.floor(noise * 50)
		local line = string.rep("*", height)
		rconsoleprint(line .. "\n")
	end

	-- Prompts the user to generate a new set of mountains
	-- or exit the console window
	rconsoleprint("\nEnter 'Y' to generate a new landscape, or nothing to exit\n")

	local input = rconsoleinput()

	if string.lower(input) == "y" then
		rconsoleclear()
		generate()
	else
		rconsoledestroy()
	end
end

generate()
```

---

## Rconsoleddestroy

```lua
function rconsoledestroy(): ()
```

Konsol penceresini kapatır ve çıktısını temizler. Başlık değiştirilmeyecektir.

### Takma adlar

 * `consoledestroy`

### Örnek

```lua
-- Create a console window titled "New console" and with the output "Hello, world!"
rconsolesettitle("New console")
rconsoleprint("Hello, world!")
rconsolecreate()

-- Close the console window, clearing its output
rconsoledestroy()

-- Reopen the console window titled "New console" with no output
rconsolecreate()
```

---

## rconsole girişi

`⏰ Yields`

```lua
function rconsoleinput(): string
```

Kullanıcının konsol penceresine metin girmesini bekler. Sonucu döndürür.

### Takma adlar

 * `consoleinput`

### Örnek

```lua
-- Create the console window
rconsolesettitle("Your Info")
rconsoleprint("What is your name?\nMy name is: ")
rconsolecreate()

-- Retrieve the user's input
local name = rconsoleinput()
rconsoleprint("Hello, " .. name .. "!")

-- Cleanup
task.wait(1)
rconsoledestroy()
```

---

## rconsoleprint

```lua
function rconsoleprint(text: string): ()
```

Prints `text` to the console window. Does not clear existing text or create a new line.

### Parametreler

* `text` - The text to append to the output.

### Takma adlar

 * `consoleprint`

### Örnek

```lua
-- Create a console window titled "New console" with the
-- output "Hello, world!! How are you today?"
rconsolesettitle("New console")
rconsoleprint("Hello, world!")
rconsoleprint("! How are you today?")
rconsolecreate()
```

---

## rconsolesettitle

```lua
function rconsolesettitle(title: string): ()
```

Sets the title of the console window to `title`.

### Parametreler

 * `title` - The new title.

### Takma adlar

 * `rconsolename`
 * `consolesettitle`

### Örnek

```lua
-- Create a console window titled "My console"
rconsolesettitle("My console")
rconsolecreate()
```