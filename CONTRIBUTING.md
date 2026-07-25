# Katkıda Bulunmak

## Yönergeler
* Yeni bir işlev eklemek, TEMPLATE.md dosyasını kopyalayıp, yeniden adlandırarak ve gerektiği şekilde düzenleyerek yapılabilir.
* Mevcut fonksiyonların düzenlemelerini sağlayabilirsiniz, sadece düzenleyip gönderebilirsiniz.
* Mümkün olduğunda dosyaları ilgili kategori klasörlerinde düzenleyin.
* İşlev imzasını yazarken lütfen **geçerli Luau sözdizimi** olduğundan emin olun. Yazılan luau'ya ilişkin belgeleri [burada](https://luau-lang.org/typecheck#union-types) bulabilirsiniz.
* When referencing arguments of a function in the description, please use `` in order to make `it look nice like so`.
* Please confirm a function has not already been added to the API before you submit it.

* Functions must be named appropriately, if you are contributing one - the following criteria applies for the naming:
  * No brand names should be visible in your documentation.
    * This also includes function alias' - UNC aims to be a vanilla naming convention, not a branded one
   * The function name must be **descriptive of what the function does**.
   * Aliases for shortening function names without good reason are *not* allowed. For example, `hookfunc` is not a alias supported by UNC. Function names should be written out in full.
  
* Açıklamanın tutarlı olması gerekir; eğer işlev bir açıklamayı garanti etmeyecek kadar basitse, açıklamaya "Yok" yazabilirsiniz.
* İşlevler her zaman bir takma ad gerektirmez; bu alana N/A da girebilirsiniz.

## Düzenlemelerinizi ve/veya eklemelerinizi gönderme
Bu, [github pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposed-changes-to-your-work-with-pull-requests/about-pull-requests) aracılığıyla yapılabilir. Depoyu klonlayın, değişikliklerinizi yapın ve ardından bir çekme isteği gönderin. Çekme talebi, birleşmeden önce UNC'nin birkaç üyesi tarafından incelenecek.
