# Önbellek

**cache** kitaplığı, dahili Örnek önbelleğini değiştirmeye yönelik yöntemler sağlar.

Note that some of the methods are only available as global functions. They will be tagged with `🌎 Global`.

---

## önbellek.geçersiz kılma

```lua
function invalidate(object: Instance): ()
```

Deletes `object` from the Instance cache. Effectively invalidates `object` as a reference to the underlying Instance.

### Parameters

 * `object` - The object to invalidate.

### Örnek

```lua
local Lighting = game:GetService("Lighting")
cache.invalidate(game:GetService("Lighting"))
print(Lighting, Lighting == game:GetService("Lighting")) --> Lighting, false
```

---

## önbellek.iscached

```lua
function iscached(object: Instance): boolean
```

Checks whether `object` exists in the Instance cache.

### Parameters

 * `object` - The object to find.

### Örnek

```lua
local Lighting = game:GetService("Lighting")
cache.invalidate(Lighting)
print(cache.iscached(Lighting)) --> false
```

---

## önbellek.değiştir

```lua
function replace(object: Instance, newObject: Instance): ()
```

Replaces `object` in the Instance cache with `newObject`.

### Parametreler

 * `object` - The object to replace.
 * `newObject` - The new object to replace `object` with.

### Example

```lua
local Lighting = game:GetService("Lighting")
local Players = game:GetService("Players")

cache.replace(Lighting, Players)

print(Lighting) --> Players
```

---

## cloneref

`🌎 Global`

```lua
function cloneref(object: Instance): Instance
```

Returns a copy of the Instance reference to `object`. This is useful for managing an Instance without directly referencing it.

### Parametreler

 * `object` - The Instance to clone.

### Örnek

```lua
local Lighting = game:GetService("Lighting")
local LightingClone = cloneref(Lighting)

print(Lighting == LightingClone) --> false
```

---

## örnekleri karşılaştırmak

`🌎 Global`

```lua
function compareinstances(a: Instance, b: Instance): boolean
```

Returns whether objects `a` and `b` both reference the same Instance.

### Parametreler

 * `a` - The first Instance to compare.
 * `b` - The second Instance to compare.

### Örnek

```lua
local Lighting = game:GetService("Lighting")
local LightingClone = cloneref(Lighting)

print(Lighting == LightingClone) --> false
print(compareinstances(Lighting, LightingClone)) --> true
```