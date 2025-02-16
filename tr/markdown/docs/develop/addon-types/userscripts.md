---
title: Userscript'ler
description: Userscript'ler, Scratch sayfaları boyunca kod çalıştırmanıza izin verir - düğme ekleme, Scratch düzenleyiciyi geliştirme veya hayal edebileceğiniz herhangi bir şey gibi şeyler yapabilirsiniz.
---
## O da neyin nesi?
Userscript'ler, Scratch sayfaları boyunca kod çalıştırmanıza izin verir - düğme ekleme, Scratch düzenleyiciyi geliştirme veya hayal edebileceğiniz herhangi bir şey gibi şeyler yapabilirsiniz.

## Nasıl bir userscript eklerim?
**Eklentinizde herhangi bir değişiklik yaptıktan sonra Scratch Eklentilerini `chrome://extensions` dan yenilemeyi unutmayın.**
Eklentinizin (addon.json) manifest dosyasına gidin ve `userscripts"` adında bir özellik ekleyin.
Bu özellik bir dizi olmalıdır.
Dizinin her bir ögesi şu özelliklere sahip olmalıdır: `"url"` ve `"eşleşir"`.
`"url"`, bir JavaScript dosyasına göreli bir bağlantı olmalıdır.
`"matches"`, userscript'leri çalıştırmak istediğiniz bağlantıların bir dizisi olmalıdır. Yıldız işaretlerini kullanabilirsiniz.
Örnek manifest:
```json
{
  "name": "Scratch Mesajlaşma",
  "description": "Scratch mesajlarınızı kolayca okumanızı ve yanıtlamanızı sağlar.",
  "userscripts": [
    {
      "url": "userscript.js",
      "matches": ["https://scratch.mit.edu/*"]
    },
    {
      "url": "second_userscript.js",
      "matches": ["https://scratch.mit.edu/projects/*", "https://scratch.mit.edu/users/*"]
    }
  ],
  "tags": ["community"],
  "enabledByDefault": false
}
```

## JavaScript dosyası nasıl görünüyor?
Userscript'ler, JS dosyalarının çalışması için belirli bir yapı gerekir.
Userscript'ler için, tüm kodunuzu şuna benzeyen bir işlevin içine sarmanız **zorunludur**:
```js
export default async function ({ addon, global, console }) {
  console.log("Merhaba, " + await addon.auth.fetchUsername());
}
```
Daha temiz bir koda sahip olmak için kendi fonksiyonlarınızı yazmak istiyorsanız, bunları ana fonksiyonların içine dâhil etmelisiniz:
**Bu çalışacak:**
```js
export default async function ({ addon, global, console }) {
  // Bu çalışır!
  merhabaDe();
  async function merhabaDe() {
    console.log("Merahaba, " + await addon.auth.fetchUsername());
  }
}
```
**Bu işe YARAMAZ:**
```js
export default async function ({ addon, global, console }) {
  // Bu ÇALIŞMAZ!
  merhabaDe();
}
async function merhabaDe() {
  console.log("Merhaba, " + await addon.auth.fetchUsername());
  // Hata: eklenti tanımlanamadı!
}
```

## [`addon.*` API'leri](/docs/developing/addon-apis-reference)
Bazı `addon.*` API'lerine, userscript'lerden erişebilirsiniz. Daha fazla bilgi için belgelere bakın.

## Userscript'lerin teknik yönleri
Userscript'ler, Scratch sayfası tamamen yüklendikten sonra çalışır - başka bir deyişle, `defer` modunda çalışırlar.
Teknik olarak konuşursak, her userscript, bir işlevi dışa aktaran bir JavaScript modülüdür. JavaScript modülleri her zaman "katı mod"da çalışır.
Bu, aynı eklentinin userscript'lerinin değişkenleri ve işlevleri PAYLAŞMADIĞI anlamına gelir! Bunu yapmak istiyorsanız, `global` nesnesini kullanmalısınız (daha fazla bilgi aşağıda).
Scratch Eklentileri daha sonra dışa aktarılan bu işlev modüllerini çağırır ve özel sarmalayıcıların yanı sıra `addon.*` API'lerine erişmesini sağlar:
- `addon`: Userscript'lere  [`addon.*` API'lerine](/docs/developing/addon-apis-reference) erişim sağlar.
- `global`: bu, aynı eklentinin tüm userscript'ler arasında paylaşılan bir nesnedir. **Örnek kullanım:**
```js
// userscript-1.js
export default async function ({ addon, global, console }) {
  global.merhabaDe = () => console.log("Merhaba, " + addon.auth.fetchUsername());
}

// userscript-2.js
export default async function ({ addon, global, console }) {
  global.sayMerhaba();
  // Bu, eklenti bildiriminde olduğu sürece çalışır, userscript-1.js, userscripts dizisindeki userscript-2.js'den öncedir.
}
```
- `console`: Bu, gördüğünüz günlüğü hangi eklentinin tetiklediğini kolayca görmenizi sağlayan bir sarmalayıcıdır.

## Userscript'lerde hata ayıklama
**Eklentinizde herhangi bir değişiklik yaptıktan sonra Scratch Eklentilerini `chrome://extensions`'dan yenilemeyi unutmayın.**
Userscript'lerde hata ayıklamak için öncelikle eklentinizin etkin olduğundan emin olun.
Ardından, userscript'inizin çalışması gerektiğini belirttiğiniz bir URL'ye gidin.
Ctrl+Shift+J tuşlarına basarak konsolu açın.
Sizinki de dâhil olmak üzere eklentilere göre konsol günlüklerini görmelisiniz. Bir devtools uzmanıysanız, kodunuzda kesme noktaları ayarlamakta herhangi bir sorun yaşamayacaksınız.
İpucu: Dosyanızı her seferinde değiştirmeden `addon.*` API'sini test etmek istiyorsanız, eklentinizi `window.addon = addon;` yapın (ana işlevin içinde) ve eklentinizin konsoldan `addon` nesnesi. Bu depoya katkıda bulunmadan önce bu satırı kaldırdığınızdan emin olun! Userscript'ler, global nesneyi kirletmemelidir.
