# C# Notları    

*02 Mayıs 2021 - Tarık Aydın*

* Bütün class’lar object tipindedir.  

* Switch statement’te case ve default break ile kapatılır, default değer verilebilir.  

* While döngüsü (-iken anlamındadır) koşul sağlanana kadar sürekli tekrar eder.

* String dizi:  
    ```csharp
    string[] sehirler = { "ank", "ist" };
    ```
* Dizilerde küme paranteziyle elemanları belirtirsen `new int` veya `new string` demeye gerek yoktur.  

 * **Sayısal Veri Tipleri**
    * **Float:** 32-bit kayan nokta değerleri (sayı). Ondalıklı ise sona `f` harfi konur (`12.212f` gibi).  
    * **Double:** 64-bit.  
    * **Decimal:** (ondalık): 128-bit. Sonuna `m` harfi alır.  
    * **Byte:** 0 ile 255 arası sayı değişkeni.  
    * **Sbyte:** -128 ile +127 arası sayı değişkeni.  
    * **Short:** Daha yüksek sayı değişkeni.
    * **Ushort:** Short’tan daha büyük sayılar.  
    * Short = Int16, int = Int32, long = Int64. 

* Metotlar kod fazlalığından kurtarır.  
* Sınıf en geniş kümedir. Nesne en alt elemandır.  
* Private erişimde get set kullanılır.  
* RAM’de “stack” değer tiplerini (int, string…) hafızada tutar. “Heap” ise referans türlerini (object, class…) tutar.

* `i += 1` i’yi bir artırır.  
`i++` i’yi bir artırır.  
`i = +1` (i+1) i’ye bir ekler.  

* Const sabit değişkendir. Değiştirilemez. Değeri aynı satırda verilmelidir.  Const olan iki int birbiriyle matematiksel işlem yapabilir.
    ```csharp
    const int a = 1;
    const string b = "asdas";
    ```  

* `"PC, \"bilgisayar\" anlamına gelir."` → Bilgisayarı tırnağa almak için böyle yapılır (kaçış karakteri).  


* Bütün simgeler operatör (+*-<>……), etki ettikleri değişkenler ise operand’tır.  

    ```csharp
    object i = "50";
    string a = i as string; // "as" dönüşüm yapar.
    ```
    ```csharp
    int i = 50;
    bool a = i is int; // "is" ifadenin türünü kontrol eder. True/false olarak yazar. (Şu an i int olduğu için true çıkar)
    ```

* İki case alt alta yazılıp tek break ile kırılınca, her iki durumda da o koda gider:  
    ```csharp
    case 4:
    case 5:
        Console.Write("hello");
        break;
        //4 ve 5 durumunda //hello yazar.
        //default yoksa ve hiçbir case’e uymuyorsa hiçbir şey yapmaz.
    ```


* For döngüsünde şart parantezinin içi boş bırakılabilir fakat noktalı virgüller konmak zorunda:  
    ```csharp
    for(;;) // Sonsuz döngü
    ```

* While, for’un sadece koşuldan oluşan halidir.  
* Do while’da önce bir kere yapar (`do`), sonra koşula uyuyorsa tekrar eder:  
    ```csharp
    do
    {
        // ...
    } while (koşul);
    ```

* **Ternary if:** Durum "e" ise "Teşekkürler!!", değilse "Sağlık olsun..." yazar.  
    ```csharp
    Console.Write(durum == "e" ? "Teşekkürler!!" : "Sağlık olsun...");
    ```
* **Int[,] dizi1 çok boyutlu dizi:** Her bir elemanı bir dizidir. Her elemanın dizi sayısı eşitse matris, değilse düzensiz dizi (`int[][]`).
    ```csharp
    int[,] dizi1 // Çok boyutlu dizi
    ```


* **Metotlar**:  
    ```csharp
    public  static int topla(int a, int b)
    {
        return a + b;
    }
    ```
* Public olursa her class’tan erişilir. Private olursa sadece bulunduğu class’ta erişilebilir.  
Public ya da private yazılmazsa default olarak private sayılır.  
Static ise nesne oluşturmadan o metoda erişilebilmek için kullanılır.  
Metot farklı bir class’ta kullanılacaksa, metotun bulunduğu class ismiyle çağrılır:  
    ```csharp
    ClassIsmi.topla();
    ```
* Metot boş ise `void` kullanılır (geriye değer döndürmüyorsa).  
Return ediyorsa void yerine değişken türü yazılır.  
Metot parantezinde türler tek tek belirtilir. Tür ismi tek yazılıp ortak alınamaz.  
Yani:  
    ```csharp
    static void Ornek(int a, int b) // ✅
    static void Ornek(int a, b)     // ❌
    ```
* Metot’a dışarıdan değer verilmeyecekse parantez boş bırakılır.  

* `ref`, `out`: Metotlarda dizi değil de tek değişken varsa kullanılır.  

    ```csharp
    static int toplam(params int[] sayilar)
    ```
* `params` kelimesi, dizilerde metot’u kullanırken metot’a girilecek parametre sayısını istediğimiz kadar girebilmeyi sağlar.  

    ```csharp
    private static void ekranayaz(Array dizi)
    ```
* Farklı class’tan metot çağrılacaksa, metotun olduğu class’tan bir nesne oluşturmak gerekir (static olursa nesneye gerek yoktur).  

* Bir sınıfın içinde sadece metot veya özellik oluşturulur. Metot ve özelliğin içindeki kodlar dışında sınıfa bir şey yazılmaz.  
Asıl kodlar `Main` içerisindedir.

* Dizide eleman arama:  
    * `IndexOf` → Baştan başlar.  
    * `LastIndexOf` → Sondan başlar.  

 * Dizinin kaçıncı elemanı olduğunu yazma:  
    ```csharp
    Array.IndexOf(dizi1, "arananeleman");
    ```

* `String.Compare` metinleri karşılaştırır. True eklersen küçük/büyük harf duyarlılığı olmaz.  

* `string.Concat` her tip değişkeni string yaparak birleştirir.