# Kripto

**crypt** kitaplığı, dize verilerinin şifrelenmesi ve şifresinin çözülmesi için yöntemler sağlar.

Bu sayfada belgelenen davranış ve örnekler Script-Ware'e dayanmaktadır.

---

## crypt.base64encode

```lua
function crypt.base64encode(data: string): string
```

Bir bayt dizisini Base64'e kodlar.

### Parametreler

 * `data` - The data to encode.

### Takma adlar

 * `crypt.base64.encode`
 * `crypt.base64_encode`
 * `base64.encode`
 * `base64_encode`

### Örnek

```lua
local base64 = crypt.base64encode("Hello, World!")
local raw = crypt.base64decode(base64)

print(base64) --> SGVsbG8sIFdvcmxkIQ==
print(raw) --> Hello, World!
```

---

## crypt.base64decode

```lua
function crypt.base64decode(data: string): string
```

Base64 dizesinin kodunu bir bayt dizisine dönüştürür.

### Parametreler

 * `data` - The data to decode.

### Takma adlar

 * `crypt.base64.decode`
 * `crypt.base64_decode`
 * `base64.decode`
 * `base64_decode`

### Örnek

```lua
local base64 = crypt.base64encode("Hello, World!")
local raw = crypt.base64decode(base64)

print(base64) --> SGVsbG8sIFdvcmxkIQ==
print(raw) --> Hello, World!
```

---

## kripto.şifreleme

`🪲 Compatibility` `🔎 RFC`

```lua
function crypt.encrypt(data: string, key: string, iv: string?, mode: string?): (string, string)
```

AES şifrelemesini kullanarak kodlanmamış bir dizeyi şifreler. Base64 ile kodlanmış ve şifrelenmiş dizeyi ve IV'ü döndürür.

AES IV sağlanmazsa sizin için rastgele bir tane oluşturulacak ve 2. base64 kodlu dize olarak döndürülecektir.

Şifreleme modları 'CBC', 'ECB', 'CTR', 'CFB', 'OFB' ve 'GCM'dir. Varsayılan 'CBC'dir.

> ### 🪲 Uyumluluk sorunları
> Çok az uygulayıcı bu işlevi desteklemektedir ve güvenilir bir örnek yapılamaz.

### Parametreler

 * `data` - The unencoded content.
 * `key` - A base64 256-bit key.
 * `iv` - Optional base64 AES initialization vector.
 * `mode` - The AES cipher mode.

---

## şifre.şifreyi çöz

`🪲 Compatibility` `🔎 RFC`

```lua
function crypt.decrypt(data: string, key: string, iv: string, mode: string): string
```

Base64 ile kodlanmış ve şifrelenmiş içeriğin şifresini çözer. Ham dizeyi döndürür.

Şifreleme modları 'CBC', 'ECB', 'CTR', 'CFB', 'OFB' ve 'GCM'dir.

> ### 🪲 Uyumluluk sorunları
> Çok az uygulayıcı bu işlevi desteklemektedir ve güvenilir bir örnek yapılamaz.

### Parametreler

 * `data` - The base64 encoded and encrypted content.
 * `key` - A base64 256-bit key.
 * `iv` - The base64 AES initialization vector.
 * `mode` - The AES cipher mode.

---

## crypt.generatebytes

```lua
function crypt.generatebytes(size: number): string
```

Verilen boyutta rastgele bir bayt dizisi oluşturur. Diziyi base64 ile kodlanmış bir dize olarak döndürür.

### Parametreler

 * `size` - The number of bytes to generate.

### Örnek

```lua
local bytes = crypt.generatebytes(16)
print(bytes) --> bXlzcWwgYm9vbGVhbnM=
print(#crypt.base64decode(bytes)) --> 16
```

---

## crypt.generatekey

```lua
function crypt.generatekey(): string
```

Generates a base64 encoded 256-bit key. The result can be used as the second parameter for the `crypt.encrypt` and `crypt.decrypt` functions.

### Örnek

```lua
local bytes = crypt.generatekey()
print(#crypt.base64decode(bytes)) --> 32 (256 bits)
```

---

## kripto.karma

```lua
function crypt.hash(data: string, algorithm: string): string
```

Verilen algoritmayı kullanarak verilere karma oluşturma işleminin sonucunu döndürür.

Bazı algoritmalar arasında 'sha1', 'sha384', 'sha512', 'md5', 'sha256', 'sha3-224', 'sha3-256' ve 'sha3-512' bulunur.

### Parametreler

 * `data` - The unencoded content.
 * `algorithm` - A hash algorithm.

### Örnek

```lua
local hash = crypt.hash("Hello, World!", "md5")
print(hash) --> 65A8E27D8879283831B664BD8B7F0AD4
```