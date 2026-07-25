# Birleşik Adlandırma Kuralı
diğer adıyla UNC, komut dosyası oluşturucularımız için birleşik bir komut dosyası oluşturma API'si sağlamak amacıyla yürütücü geliştiriciler arasında kurulan bir kuruluştur.

## UNC Emekli Oldu 👋

Küresel olarak belgelenmiş bir API'nin **bu kadar başarılı** olacağını kim düşünebilirdi? Kesinlikle yaptık! UNC harika bir konseptti ve kullanım ömrü bunu kanıtladı; komut dosyası hazırlayanlar için mükemmel bir API oluşturmaya odaklanarak, bu komut dosyalarının harika komut dosyaları oluşturmasına izin vererek yazılımımızı daha kullanışlı hale getirdi.

İki yıl önce UNC, kötü adlandırılmış işlevler sorununun çözülmesine yardımcı oldu. Bugün, ([Script-Ware](https://script-ware.com/)) şirketinin kurucuları, bu işlevlerin kullanıldığı Roblox için komut dosyası yürütme yazılımı mühendisliğini durdurdu.

UNC'yi yazılım için bir kıyaslama noktası olarak kullanmaya devam edebilirsiniz, ancak daha yeni özelliklerle birlikte zamanla geçerliliğini yitirecektir. Üzgünüz! Umarız bir gün bunu telafi ederiz.

---
~~**Daha iyi bilgi için lütfen resmi web sitemize gidin: https://scriptunc.org/**~~

Bu web sitesi o zamandan beri kaldırıldı; aynı bilgilerin tamamını aşağıda bulabilirsiniz.

## Neden?
Yıllar geçtikçe komut dosyası oluşturma, birden fazla uygulayıcıyı desteklemek için giderek daha karmaşık hale geldi. Bunun nedeni, çeşitli uygulayıcıların kullandığı birçok benzersiz adlandırma kuralıdır.

Aşağıdaki senaryoyu düşünün. Bir fonksiyonun uygulayıcıya ait olup olmadığını bilmek istiyorsunuz. Bu kodun tüm uygulayıcılarla çapraz uyumlu olabilmesi için aşağıdaki gibi kodlara ihtiyaç vardır:
```lua
local is_executor_closure = is_syn_closure or is_fluxus_closure or is_sentinel_closure or is_krnl_closure or is_proto_closure or is_calamari_closure or is_electron_closure or is_elysian_closure
```
Bu, senaryolarında çapraz uyumluluk isteyen senaryo yazarları için bir gerçektir. Senaryo yazarlarının sırf çapraz uyumluluk elde etmek için bu kadar zahmetli bir iş yapmak zorunda kalmamaları gerekiyor. UNC, herkesin üzerinde mutabakata vardığı ve takip ettiği adlandırma kurallarını kullanarak bu sorunu çözmeye çalışıyor.

Bir betiğin bir çeşidi, ortamları UNC'ye uygun şekilde uyarlanmış tüm betik yürütücüleri üzerinde doğal olarak çalışmalıdır.

## Nasıl?
UNC, API işlevselliğinin yanı sıra adlandırma kurallarına yönelik standartlar sağlar. Standart, bu GitHub'da işaretlemeyle yazılmıştır. Düzenlemeler veya eklemeler çekme istekleri aracılığıyla yapılır. Düzenlemeler ve eklemeler UNC konseyi tarafından manuel olarak onaylanır ve herkes tarafından tartışılır.

## UNC'yi destekliyoruz
Bir ürün sahibi olarak, API'yi takip ederek UNC'ye verdiğiniz destek, komut dosyası hazırlayanlar için çok daha sorunsuz bir deneyimle sonuçlanacaktır; çünkü onlar **çoğu** üründe çalışacağını güvenle söyleyebilecekleri komut dosyaları üzerinde çalışabilirler. UNC'nin API'sini uyguladıktan sonra rozeti web sitenize, başlığınıza veya uygulamanıza ekleyerek bunu görüntüleyebilirsiniz.

Rozeti burada bulabilirsiniz: ~~https://scriptunc.org/badge~~
(Bu rozet daha sonra web sitemizden kaldırılmıştır)

Bu, komut dosyası hazırlayanlara tüketicilerinizin keyif alabileceği daha kolay bir mühendislik komut dosyası yöntemi sağlama konusundaki ittifakınız konusunda insanları bilgilendirecektir.

DİKKAT: Bir ürün sahibi olarak bu işlevlerin tümüne sahip değilseniz ancak sahip olduklarınızı destekliyorsanız, o zaman UNC'yi destekliyorsunuz demektir! Rozeti web sitenizde sergilemekten daha fazlasını yapabilirsiniz.

## Ortamınız kontrol ediliyor

Yürütücü ortamınızın UNC standardını ne kadar iyi desteklediğini görmek için UNC ortam kontrol komut dosyasını çalıştırabilirsiniz. Komut dosyasını [burada] bulun.(UNCCheckEnv.lua) Komut dosyası neyin eksik olduğunu belirler ve sonuçları çalışma alanı altındaki dosyaya yazar.

## Katkıda Bulunmak
Katkıda bulunma konusunda bir rehber için [buraya](CONTRIBUTING.md) gidin.
