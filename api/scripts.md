# Komut dosyaları

**script** işlevleri, komut dosyası ortamlarına ve dahili duruma erişim sağlar.

---

## getgc

```lua
function getgc(includeTables: boolean?): {function | userdata | table}
```

Luau çöp toplayıcısındaki nesnelerin listesini döndürür.

If `includeTables` is false, tables will not be included in the list.

### Parametreler

 * `includeTables` - Whether or not to include tables in the list.

### Örnek

**Yapılacaklar** - Gerçek dünyadan örnek bir kullanım senaryosu yazın.

---

## getgenv

```lua
function getgenv(): { [string]: any }
```

Yürütücünün özel genel ortamını döndürür. Komut dosyaları arasında global işlevler eklemek veya değişkenleri paylaşmak için kullanılabilir.

### Örnek

Bir betiğin iki kez çalıştırılmasını önleyin:

```lua
if getgenv().__IS_LOADED then
	error("This script is already loaded!")
end

getgenv().__IS_LOADED = true
```

---

## yüklenen modüller

```lua
function getloadedmodules(excludeCore: boolean?): {ModuleScript}
```

Returns a list of ModuleScripts that have been loaded. If `excludeCore` is true, CoreScript-related modules will not be included in the list.

### Parametreler

 * `excludeCore` - Whether or not to exclude core modules from the list.

### Örnek

```lua
local modules = getloadedmodules(true)

for _, module in ipairs(modules) do
	print(module:GetFullName())
end
```

---

## getrenv

```lua
function getrenv(): { [string]: any }
```

Oyun istemcisinin genel ortamını döndürür. LocalScripts ve ModuleScripts'in kullandığı genel işlevlere erişmek için kullanılabilir.

### Örnek

PlayerScript'lerdeki komut dosyalarının gerekli olmasını önleyin:

```lua
local refs = {}
local bannedScripts = game:GetService("Players").LocalPlayer.PlayerScripts

refs.require = hookfunction(require, function(...)
	local module = ...
	if
		typeof(module) == "Instance"
		and module:IsA("ModuleScript")
		and module:IsDescendantOf(bannedScripts)
	then
		error("You are not allowed to require this module!")
	end
	return refs.require(...)
end)
```

---

## getrunningscripts

```lua
function getrunningscripts(): {LocalScript | ModuleScript}
```

Şu anda çalışmakta olan komut dosyalarının listesini döndürür.

### Örnek

```lua
local scripts = getrunningscripts()

for _, object in ipairs(scripts) do
	print(object:GetFullName(), "(" .. object.ClassName .. ")")
end
```

---

## betikbytecode'u al

```lua
function getscriptbytecode(script: LocalScript | ModuleScript): string
```

Verilen betiğin ham Luau bayt kodunu döndürür.

### Parametreler

 * `script` - A client-running LocalScript or ModuleScript.

### Takma adlar

 * `dumpstring`

### Örnek

```lua
local animate = game:GetService("Players").LocalPlayer.Character.Animate
local bytecode = getscriptbytecode(animate)
```

---

## getscriptclose

```lua
function getscriptclosure(script: LocalScript | ModuleScript): function
```

Generates a new closure using the bytecode of `script`.

### Parametreler

 * `script` - The script to recreate.

### Takma adlar

 * `getscriptfunction`

### Örnek

Bir ModuleScript'in dönüş değerini karşılaştırın:

```lua
local module = game:GetService("CoreGui").RobloxGui.Modules.Common.Constants

local constants = getrenv().require(module)
local generatedConstants = getscriptclosure(module)()

print(constants == generatedConstants) --> false
for k, v in pairs(constants) do
	print(k, typeof(v) == typeof(generatedConstants[k])) --> true
end
```

---

## getscripthash

```lua
function getscripthash(script: LocalScript | ModuleScript): string
```

Betiğin bayt kodunun SHA384 karmasını döndürür. Bu, bir betiğin kaynak kodundaki değişiklikleri tespit etmek için kullanışlıdır.

### Parametreler

 * `script` - A client-running LocalScript or ModuleScript.

### Örnek

```lua
local animate = game:GetService("Players").LocalPlayer.Character.Animate
local hash = getscripthash(animate)

task.delay(1.5, function ()
	animate.Source = "print('Hello World!')"
end)

for i = 1, 5 do
	task.wait(0.5)

	local newHash = getscripthash(animate)

	if hash ~= newHash then
		print("The script has changed!")
		hash = newHash
	else
		print("The script has not changed.")
	end
end
```

---

## komut dosyaları

```lua
function getscripts(): {LocalScript | ModuleScript}
```

Oyundaki her komut dosyasının bir listesini döndürür.

### Örnek

```lua
local scripts = getscripts()

for _, object in ipairs(scripts) do
	print(object:GetFullName(), "(" .. object.ClassName .. ")")
end
```

---

## getsenv

```lua
function getsenv(script: LocalScript | ModuleScript): { [string]: any }
```

Verilen betiğin genel ortamını döndürür. Yerel olarak tanımlanmayan değişkenlere ve işlevlere erişmek için kullanılabilir.

### Parametreler

 * `script` - A client-running LocalScript or ModuleScript.

### Örnek

```lua
local animate = game:GetService("Players").LocalPlayer.Character.Animate
local environment = getsenv(animate)

for k, v in pairs(environment) do
	print(k, v, "(" .. typeof(v) .. ")")
end
```

---

## getthreadidentity

```lua
function getthreadidentity(): number
```

Geçerli iş parçacığının kimliğini döndürür.

Konu kimlikleri hakkında daha fazla bilgiyi [buradan](https://roblox.fandom.com/wiki/Security_context) alabilirsiniz.

### Takma adlar

 * `getidentity`
 * `getthreadcontext`

### Örnek

```lua
local identity = getthreadidentity()
print(identity) --> 7
```

---

## setthreadidentity

```lua
function setthreadidentity(identity: number): ()
```

Geçerli iş parçacığı kimliğini ayarlar.

Konu kimlikleri hakkında daha fazla bilgiyi [buradan](https://roblox.fandom.com/wiki/Security_context) alabilirsiniz.

### Takma adlar

 * `setidentity`
 * `setthreadcontext`

### Örnek

```lua
setthreadidentity(3)
print(getthreadidentity()) --> 3
```