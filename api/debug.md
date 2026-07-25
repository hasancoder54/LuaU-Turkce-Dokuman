# Hata ayıklama

**debug** kitaplığı, Luau hata ayıklama kitaplığının bir uzantısıdır ve Luau işlevleri üzerinde daha fazla kontrol sağlar.

---

## hata ayıklama.getconstant

`⛔ Exception`

```lua
function debug.getconstant(func: function | number, index: number): any
```

Returns the constant at `index` in the constant table of the function or level `func`. Throws an error if the constant does not exist.

### Parametreler

 * `func` - A function or stack level.
 * `index` - The numerical index of the constant to retrieve.

### Örnek

```lua
local function foo()
	print("Hello, world!")
end

print(debug.getconstant(foo, 1)) --> "print"
print(debug.getconstant(foo, 2)) --> nil
print(debug.getconstant(foo, 3)) --> "Hello, world!"
```

---

## hata ayıklama.getconstants

```lua
function debug.getconstants(func: function | number): {any}
```

Returns the constant table of the function or level `func`.

> ### 🔎 İpucu
> Traversing the table with `ipairs` is not recommended, as constants can be `nil` or skipped entirely.

### Parametreler

 * `func` - A function or stack level.

### Örnek

```lua
local function foo()
	local num = 5000 .. 50000
	print("Hello, world!", num, warn)
end

for i, v in pairs(debug.getconstants(foo)) do
	print(i, v)
end
--> 1 50000
--> 2 "print"
--> 4 "Hello, world!"
--> 5 "warn"
```

---

## hata ayıklama.getinfo

`🪲 Inconsistent`

```lua
function debug.getinfo(func: function | number): DebugInfo
```

Bir işlev veya yığın düzeyi hakkında hata ayıklayıcı bilgilerini döndürür.

### Hata AyıklamaBilgisi

| Alan | Tür | Açıklama |
| ----- | ---- | ----------- |
| `source` | string | The name of the chunk that created the function. |
| `short_src` | string | A "printable" version of `source` to be used in error messages. |
| `func` | function | The function itself. |
| `what` | string | The string "Lua" if the function is a Luau function, or "C" if it is a C function. |
| `currentline` | number | The current line where the given function is executing. When no line information is available, `currentline` is set to -1. |
| `name` | string | The name of the function. If it cannot find a name, then `name` is a blank string. |
| `nups` | number | The number of upvalues in the function. |
| `numparams` | number | The number of parameters in the function (always 0 for C functions). |
| `is_vararg` | number | Whether the function has a variadic argument (1 if it does, 0 if it does not). |

> ### 🪲 Uyumluluk
> Bazı uygulayıcılarda belirli alanlar eksik.

### Parametreler

 * `func` - A function or stack level.

### Örnek

```lua
local function foo()
	print("Hello, world!")
end

for k, v in pairs(debug.getinfo(foo)) do
	print(k, v, "(" .. type(v) .. ")")
end
```

---

## hata ayıklama.getproto

`⛔ Exception` `🛡️ Security`

```lua
function debug.getproto(func: function | number, index: number, active: boolean?): function | {function}
```

Returns the proto at `index` in the function or level `func` if `active` is false.

If `active` is true, then every active function of the proto is returned.

> ### 🛡️ Güvenlik
> In some executors, the proto is non-functional if `active` is false. Debug information is preserved.\
> To retrieve a callable function, you can set `active` to true and index the first proto.

### Parametreler

 * `func` - A function or stack level.
 * `index` - The numerical index of the proto to retrieve.
 * `active` - Whether to return its list of active closures.

### Örnek

```lua
local function myFunction()
	local function proto()
		print("Hello, world!")
	end
end

local proto = debug.getproto(myFunction, 1, true)[1]
proto() --> Hello, world!
```

---

## hata ayıklama.getprotos

`🛡️ Security`

```lua
function debug.getprotos(func: function | number): {function}
```

Returns a list of protos of the function or level `func`.

> ### 🛡️ Güvenlik
> Bazı uygulayıcılarda protokol işlevsel değildir ancak hata ayıklama bilgileri korunur.\
> To retrieve a callable function, see [`debug.getproto`](#debuggetproto).

### Parametreler

 * `func` - A function or stack level.

### Örnek

```lua
local function myFunction()
	local function _1()
		print("Hello,")
	end
	local function _2()
		print("world!")
	end
end

for i in ipairs(debug.getprotos(myFunction)) do
	local proto = debug.getproto(myFunction, i, true)[1]
	proto()
end
--> Hello,
--> world!
```

---

## hata ayıklama.getstack

`⛔ Exception`

```lua
function debug.getstack(level: number, index: number?): any | {any}
```

Returns the value at `index` in the stack frame `level`. Throws an error if no value could be found.

If `index` is not specified, then the entire stack frame is returned.

### Parametreler

 * `level` - The stack frame to look up.
 * `index` - The numerical index of the value to retrieve.

### Örnek

```lua
local _ = "a" .. "b"
print(debug.getstack(1, 1)) --> ab
```

```lua
local _ = "a" .. "b"
table.foreach(debug.getstack(1), print)
--> ab
--> table.foreach()
--> debug.getstack()
--> 1
```

---

## hata ayıklama.getupvalue

`⛔ Exception`

```lua
function debug.getupvalue(func: function | number, index: number): any
```

Returns the upvalue at `index` in the function or level `func`. Throws an error if the upvalue does not exist.

Yukarı değer, bir iç fonksiyon tarafından kullanılan yerel bir değişkendir ve aynı zamanda *harici yerel değişken* olarak da adlandırılır.

[Lua görünürlük kuralları](http://www.lua.org/manual/5.1/manual.html#2.6) hakkında daha fazla bilgi edinin.

> ### 🔎 Not
> Bazı Luau optimizasyonları, dizeler ve tamsayılar gibi belirli sabitleri otomatik olarak satır içine alır.\
> They can be retrieved through [`debug.getconstant`](#debuggetconstant) instead.

### Parametreler

 * `func` - A function or stack level.
 * `index` - The numerical index of the upvalue to retrieve.

### Örnek

```lua
local upvalue = function ()
end

local function foo()
	print(upvalue)
end

print(debug.getupvalue(foo, 1)) --> upvalue
```

Luau optimizasyonuna bir örnek:

```lua
local upvalue = "Hello, world!"

local function foo()
	print(upvalue)
end

print(debug.getupvalue(foo, 1)) --> Errors "upvalue index out of range"
print(debug.getconstant(foo, 3)) --> Hello, world!
```

---

## hata ayıklama.getupvalues

```lua
function debug.getupvalues(func: function | number): {any}
```

Returns a list of upvalues of the function or level `func`.

> ### 🔎 İpucu
> Traversing the table with `ipairs` is not recommended, as upvalues can be `nil` or skipped entirely.

### Parametreler

 * `func` - A function or stack level.

### Örnek

```lua
local upvalue1, upvalue2 = function () end, function () end

local function foo()
	print(upvalue1, upvalue2)
end

for k, v in pairs(debug.getupvalues(foo)) do
	print(k, v, "(" .. type(v) .. ")")
end
--> 1 upvalue1() (function)
--> 2 upvalue2() (function)
```

---

## hata ayıklama.setconstant

`⛔ Exception`

```lua
function debug.setconstant(func: function | number, index: number, value: any): ()
```

Sets the constant at `index` in the function or level `func` to `value`.

> ### ⛔ İstisna
> The type of `value` must match the type of the constant at `index`.

### Parametreler

 * `func` - A function or stack level.
 * `index` - The numerical index of the constant to set.
 * `value` - The value to set.

### Örnek

```lua
local function foo()
	print("Goodbye, world!")
end

debug.setconstant(foo, 3, "Hello, world!")
foo() --> Hello, world!
```

---

## hata ayıklama.setstack

`⛔ Exception`

```lua
function debug.setstack(level: number, index: number, value: any): ()
```

Sets the register at `index` in the stack frame `level` to `value`.

> ### ⛔ İstisna
> The type of `value` must match the type of the register at `index`.

### Parametreler

 * `level` - The stack frame to look up.
 * `index` - The numerical index of the register to set.
 * `value` - The value to set.

### Örnek

```lua
local function foo()
	-- Change the first value from "Goodbye, world!" to "Hello, world!"
	return "Goodbye, world!", debug.setstack(1, 1, "Hello, world!")
end

print(foo()) --> Hello, world!
```

---

## hata ayıklama.kurulum değeri

```lua
function debug.setupvalue(func: function | number, index: number, value: any): ()
```

Sets the upvalue at `index` in the function or level `func` to `value`.

### Parametreler

 * `func` - A function or stack level.
 * `index` - The numerical index of the upvalue to set.
 * `value` - The value to set.

### Örnek

```lua
local function somethingImportant()
	print("Goodbye, world!")
end

local function foo()
	somethingImportant()
end

debug.setupvalue(foo, 1, function ()
	print("Hello, world!")
end)

foo() --> Hello, world!
```
