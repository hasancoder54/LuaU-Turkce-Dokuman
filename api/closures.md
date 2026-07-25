# Kapanışlar

**kapatma** işlevleri Luau kapanışlarını oluşturmak, tanımlamak ve bunlarla etkileşimde bulunmak için kullanılır.

---

## çek arayan

```lua
function checkcaller(): boolean
```

Şu anda çalışmakta olan işlevin yürütücü tarafından çağrılıp çağrılmadığını döndürür.

Bu, oyun tarafından çağrıldığında farklı davranan meta yöntem kancaları için kullanışlıdır.

### Örnek

Prevent the executor from invoking `__namecall` with the global `game` object:

```lua
local refs = {}

refs.__namecall = hookmetamethod(game, "__namecall", function(...)
	local self = ...
	local isRunningOnExecutor = checkcaller()

	if isRunningOnExecutor then
		-- The executor invoked the __namecall method, so this will not affect the
		-- scripts in the game.
		if self == game then
			error("No __namecall on game allowed")
		end
	end

	return refs.__namecall(...)
end)

game:Destroy() --> Error "No __namecall on game allowed"
```

---

## klon işlevi

```lua
clonefunction<T>(func: T): T
```

Generates a new closure based on the bytecode of function `func`.

### Parametreler

 * `func` - The function to recreate.

### Örnek

```lua
local function foo()
	print("Hello, world!")
end

local bar = clonefunction(foo)

foo() --> Hello, world!
bar() --> Hello, world!
print(foo == bar) --> false
```

---

## getcallingscript

```lua
function getcallingscript(): BaseScript
```

Şu anda çalışan işlevden sorumlu betiği döndürür.

### Örnek

Prevent scripts in PlayerGui from invoking the `__namecall` hook:

```lua
local refs = {}
local bannedScripts = game:GetService("Players").LocalPlayer.PlayerGui

refs.__namecall = hookmetamethod(game, "__namecall", function(...)
	local caller = getcallingscript()

	-- Use '.' notation to call the IsDescendantOf method without invoking
	-- __namecall and causing a recursive loop.
	local isBanned = caller.IsDescendantOf(caller, bannedScripts)

	if isBanned then
		error("Not allowed to invoke __namecall")
	end

	return refs.__namecall(...)
end)
```

---

## kanca işlevi

```lua
function hookfunction<T>(func: T, hook: function): T
```

Replaces `func` with `hook` internally, where `hook` will be invoked in place of `func` when called.

Returns a new function that can be used to access the original definition of `func`.

> ### ⚠️ Uyarı
> If `func` is a Luau function (`islclosure(func) --> true`), the upvalue count of `hook` must be less than or equal to that of `func`.\
> [Lua görünürlük kurallarında](http://www.lua.org/manual/5.1/manual.html#2.6) artış değerleri hakkında daha fazla bilgi edinin.

### Parametreler

 * `func` - The function to hook.
 * `hook` - The function to redirect calls to.

### Takma adlar

 * `replaceclosure`

### Örnek

```lua
local function foo()
	print("Hello, world!")
end

local fooRef = hookfunction(foo, function(...)
	print("Hooked!")
end)

foo() --> Hooked!
fooRef() --> Hello, world!
```

---

## kapatma

```lua
function iscclosure(func: function): boolean
```

Returns whether or not `func` is a closure whose source is written in C.

### Parametreler

 * `func` - The function to check.

### Örnek

```lua
print(iscclosure(print)) --> true
print(iscclosure(function() end)) --> false
```

---

## kapatma

```lua
function islclosure(func: function): boolean
```

Returns whether or not `func` is a closure whose source is written in Luau.

### Parametreler

 * `func` - The function to check.

### Örnek

```lua
print(islclosure(print)) --> false
print(islclosure(function() end)) --> true
```

---

## icracı kapatma

```lua
function isexecutorclosure(func: function): boolean
```

Returns whether or not `func` was created by the executor.

### Parametreler

 * `func` - The function to check.

### Takma adlar

 * `checkclosure`
 * `isourclosure`

### Örnek

```lua
print(isexecutorclosure(isexecutorclosure)) --> true
print(isexecutorclosure(function() end)) --> true
print(isexecutorclosure(print)) --> false
```

---

## yük dizesi

```lua
function loadstring(source: string, chunkname: string?): (function?, string?)
```

Verilen kaynak kodundan bir parça üretir. Döndürülen fonksiyonun ortamı global ortamdır.

If there are no compilation errors, the chunk is returned by itself; otherwise, it returns `nil` plus the error message.

`chunkname` is used as the chunk name for error messages and debug information. When absent, it defaults to a **random string**.

> ### ⛔ Tehlike
> Vanilla Lua allows `source` to contain Lua bytecode, but it is a security vulnerability.\
> Bu uygulanmaması gereken bir özelliktir.

### Parametreler

 * `source` - The source code to compile.
 * `chunkname` - Optional name of the chunk.

### Örnek

```lua
local func, err = loadstring("print('Hello, world!')")
assert(func, err)() --> Hello, world!

local func, err = loadstring("print('Hello")
assert(func, err)() --> Errors "Malformed string"
```

---

## yeni kapatma

```lua
function newcclosure<T>(func: T): T
```

Returns a C closure that wraps `func`. The result is functionally identical to `func`, but identifies as a C closure, and may have different metadata.

> ### ⚠️ Uyarı
> Bir C kapanışının içinde boyun eğmeye çalışmak hataya yol açacaktır.\
> Bunun yerine, eylemleri farklı iş parçacıklarına ertelemek için görev kitaplığını kullanın.

### Parametreler

 * `func` - The function to wrap.

### Örnek

```lua
local foo = function() end
local bar = newcclosure(foo)

print(iscclosure(foo)) --> false
print(iscclosure(bar)) --> true
```