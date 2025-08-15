# Nesne Yönelimli Programlama (Object Oriented Programming) Notları

*24 Ekim 2021 - Tarık Aydın*

## Değişkenler
C#’ta iki sınıf değişken vardır: **Primitive tipler** ve **Nesne tipleri**.  
- Nesne tipleri tanımlanmazsa `null` değeri alır.  
- Primitive tiplerin varsayılan değerleri vardır: sayılar için `0`, `bool` için `false` gibi.  
- Nesne tipi değişkenler `new` keyword’ü ile yaratılır.

**Not:** `string` 3. bir sınıf gibidir. Primitive gibi davranır ama aslında nesne tipidir.  

- Primitive tipler **stack** üzerinde tutulur.  
- Nesne tipleri ise **heap** üzerinde nesne referansı tutar. Bu referanslar heap üzerindeki çeşitli değişken gruplarını toplu halde ifade eder.  
- String heap’te tutulur referans olarak, ama primitive tip gibi yani stack’te tutuluyormuş gibi davranır.

---

## Tip Dönüşümleri
- **Implicit conversion** → Dolaylı dönüşüm. İki tip arasındaki dönüşümde olası veri kaybı varsa yapılır. Cast işlemi ile de yapılabilir. Sistem, primitive tipler için `Convert.To` metotlarını da sağlar.  
- **Explicit conversion** → Veri kaybı yaşanmayacağı garantiyse otomatik olarak yapılır.  

Örnekler:  
- Cast işlemi, sayı tipini başka sayı tipine çevirir. String’den sayıya çeviremez. Çeviri sırasında sığmayan bitler atılır. Ondalıklı kısımlar atılır; sığmayacak veya aşırı küçük değerlerde yanlış sonuç üretebilir.  
- `Convert.To` metotları hem sayıdan sayıya hem de string’den sayıya çevirir. Girilen biçim yanlışsa hata verir. String `null` ise 0’a çevirir.  
- `Parse` metodu sadece string’den sayıya çevirir. `null` girilirse veya yanlış string girilirse hata verir.

---

## Nesne Oluşturma
- Nesneler `new` keyword’ü ile oluşturulur. Her nesne kendi değişken ve metotlarına sahiptir. Bir nesne üzerinde yapılan işlem, aynı class’ın başka bir nesnesini etkilemez.  
- **Static** → Static değişken ve metotlar nesnelere bağlı değildir. Program genelinde global olarak ve yalnızca bir kez tanımlanırlar. Bir class içindeki statik bir değişkenin değeri değiştiğinde, tüm programda aynı değeri okur ve üzerine yazar. Static metotlar, nesnenin kendisine ait statik olmayan değişkenlere erişemez.  
- **Constructor** → `public ClassAdi()` şeklinde tanımlanır. Bir class’tan nesne yaratıldığı anda ilk olarak bu metot çalışır. Bir class farklı parametreler içeren birden fazla constructor’a sahip olabilir.

---

## Erişim Belirleyiciler (Access Modifiers)
- **Private** → Sadece kendi class’ı içinde erişilebilir. Değişken ve metotlar default olarak private’dır.  
- **Internal** → Sadece kendi projesi içinde (assembly). Class’lar default olarak internal’dır.  
- **Public** → Her yerden erişilebilir.  
- **Protected** → Extend eden class (child class) tarafından erişilebilir.

---

## Encapsulation
Bir class’ın içindeki değişkenin `private` olarak tanımlanıp, `public` bir getter ve setter metot yardımıyla get ve set edilmesine denir.

---

## Polimorphism
- **Overload** → Aynı isimli metodun farklı parametrelere sahip halleri.  
- **Override** → Üst sınıftaki `virtual` bir metodun alt sınıfta `override` keyword’ü ile baştan yazılmasıdır.  
  `Virtual` → Override edilebilir (ezilebilir) metod.

---

## Inheritance
Bir sınıfın başka bir sınıftan miras almasıdır. Üst sınıftaki tüm metod ve değişkenler alt sınıfa aktarılır.  
Ortak özellikleri üst sınıfta tanımlayıp alt sınıflarda kullanmak içindir.  
Yazımı: `altClass : UstClass`.

---

## Abstraction ve Implementation
### Abstraction
Üst class’ın **abstract** bir class olması ile yapılır. Abstract class’lar normal class’tan farklı olarak gövdesiz metod içerebilir. Bu metodlar alt sınıfta ezilip gövdeleri yazılmak zorundadır. Yani abstract class abstract metod barındırabilir.  
Bir class yalnızca bir class’tan inherit edilebilir.

### Implementation
**Interface** ile yapılır. Interface isimleri `I` ile başlar (örn. `ITest`).  
- Interface’ler yalnızca gövdesiz metod barındırabilir.  
- Metotlara `public/private` yazılmaz; tüm metodlar otomatik olarak public olur.  
- Bir class’a sınırsız sayıda interface implemente edilebilir.