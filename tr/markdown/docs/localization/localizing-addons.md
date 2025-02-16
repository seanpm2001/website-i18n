---
title: Eklentileri Yerelleştirme
description: Eklentileri yerelleştirmek kolaydır.
---
Eklentileri yerelleştirmek kolaydır.

## Mesaj ekleme
`addons-l10n/en/` altında, `ADDONID.json` adında bir dosya oluşturun; burada ADDONID, eklentinin kimliğidir (klasörün adıdır). Çevirmek istediğiniz mesajlarınızı buraya yazın:

```json
{
  "ADDONID/meow": "Meow",
  "ADDONID/cats": "{number, plural, one {1 cat} other {# cats}}",
  "ADDONID/eat": "I want to eat {food}!",
  "ADDONID/salmon": "salmon",
  "ADDONID/sardine": "sardine"
}
```

### Yer tutucular
Bazen mesajların dinamik olarak oluşturulmuş şeylere sahip olması gerekir. Örneğin, kedilerin sayısı, veya giriş. Bunu halletmek için `{placeholderName}` gibi yer tutucular kullanabilirsiniz. Yer tutucu adı yalnızca harflerden oluşmalıdır (sayı içermemelidir).

### Çoğul
Ya yer tutucu bir sayıysa? `{placeholderName, plural, one {When There is onething} other {When There # Things}}` gibi çoğullar kullanabiliriz. Yer tutucu 1 ise, "bir şey olduğunda" gösterilir, aksi takdirde "(yer tutucu) şeyler olduğunda" yazar.

## Çevirileri kullanma
Userscript'in ilk satırını aşağıdaki gibi değiştirin:
```
export default async function ({ addon, console }) {
```

ile:
```
export default async function ({ addon, console, msg }) {
```

Eklenen `msg`, çevirileri almak için kullandığınız işlevdir. Örneğin, `text = "Meow"` artık `metin = msg("meow")` olabilir. Eklenti kimliği ve eğik çizgi atlanır.

### Yer tutucular
Yer tutucuya değer verebilirsiniz:
```js
cat = msg("cats", {number: 1}) // "1 cat" yazısını gösterir
cats = msg("cats", {number: 3}) // "3 cats" yazısını gösterir
hungry = msg("eat", {food: "cod"}) // "I want to eat cod!" yazısını gösterir
```

Ayrıca mesajları "iç içe" yerleştirebilirsiniz:
```js
hungry2 = msg("eat", {food: msg("salmon")}) // "I want to eat salmon!" yazısını gösterir
```

### Güvenlik
Ham HTML yazıyorsanız, `msg`, daha güvenli sürümü olan `safeMsg` ile değiştirilmelidir.
