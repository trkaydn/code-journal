# Genel .Net Geliştirici Notları 

*09 Haziran 2024 - Tarık Aydın*

## SOLID Prensipleri

- **Single Responsibility**: Her class bir görev yapmalıdır. Helper class kullanılmamalıdır.  
- **Open-Closed**: Entity’ler gelişime açık, değişime kapalı olmalıdır. Yeni özellik eklerken mevcut kodları değiştirmek gerekmemelidir.  
- **Liskov Substitution**: Kalıtım yaparken alt class, base class’ın tüm özelliklerini karşılamalıdır.  
- **Interface Segregation**: Interface’ler ortak kullanılmamalı ve implement edildiği class’ta tamamıyla karşılanmalıdır.  
- **Dependency Inversion**: Class içinde class new’leyerek bağımlılık oluşturulmamalı, interface kullanarak soyutlanmalıdır.  

---

## Microservices Notları

- **Vertical Slice** → Features klasörü. Tek bir `.cs` dosyasında birçok farklı class bulunur (ör. AddProduct DTO, Handler, Validator vb.).  
- **MediatR Library** → CQRS design pattern için kullanılır (endpoint’ten handler’a command gönderilir).  
- **Marten Library** → PostgreSQL için NoSQL database olarak kullanılır (JSON verileri saklar).  
- **Carter Library** → Minimal API endpoint tanımları için kullanılır (kolay routing ve request handling).  
- **Mapster Library** → AutoMapper alternatifi.  
- **Redis** → Cache için in-memory key-value veri deposu.  
- **Cache-Aside Pattern** → Veriyi önce cache’te kontrol et, yoksa cache veya DB’den al.  
- **Scrutor** → IOC container’a ek yetenekler sağlar.  
- **HealthChecks** → Microservices ve servislerin düzgün çalışıp çalışmadığını kontrol eder.  
- **gRPC** → Internal microservices arasında iletişim protokolü.  
- **AMQP** → RabbitMQ veya Apache Kafka kullanılarak asenkron iletişim protokolü.

---

## .NET Core Asenkron – Multithread Programlama

- **Asenkron Programlama**: Thread’in bloklanmadığı programlamadır. (Non-Blocking)  
- **Multithread Programlama**: İşlemin birden fazla thread tarafından yürütülmesidir.  
- **Main Thread**: Uygulamayı çalıştıran ana thread’tir. UI thread – primary thread olarak da bilinir.  
- **Worker Thread**: Thread pool’da bulunan, iş yapan diğer thread’lerdir.  
- Senkron programlamada tüm işlemler main thread’te yapıldığı için bloklanır.  
- Asenkron metot isimlendirmede metot ismi sonuna **Async** keyword’u eklenir.  
- **CancellationToken**: Uzun süren işlemleri iptal etmeye yarar.  
- Task sonucu almak için `await` kullanılır. `Task.Result` da kullanılabilir. Thread’i bloklar. `Task.Result`, senkron metot içinde asenkron metot çağırmaya yarar.  
- **ValueTask**: Stack’te tutulan task’tır. Daha küçük işlerde Task yerine kullanılması performanslı olacaktır.  


### Task Metotları

- **ContinueWith()**: Task bittiğinde yapılacak işleri tanımlamaya yarar.  
- **WhenAll()**: Tüm task’lar bittiğinde yapılacak işleri tanımlar.  
- **WhenAny()**: İlk biten task’ı döner.  
- **WaitAll()**: WhenAll ile benzerdir. Tek farkı main thread’i bloklar.  
- **WaitAny()**: WhenAny ile benzerdir. Tek farkı main thread’i bloklar.  
- **Delay()**: Mevcut thread’i geciktirir. Main thread bloklanmaz. `Thread.Sleep()`’te ise main thread bloklanır.  
- **Run()**: Ayrı bir thread üzerinde işlem yapar. Ağır işlemlerde kullanılır.  
- **StartNew()**: Run ile benzerdir. Tek farkı parametre alabilir.  
- **FromResult()**: Cache dataları dönmeye yarar.  


### Task Parallel Library (TPL)

- Yapılacak iş çok yoğun ise çoklu thread kullanımı sağlayan library’dir. Default olarak gelir, indirmeye gerek yoktur.  
- **Race Condition**: Birden çok thread, paylaşılan alana aynı anda ulaşmaya çalıştığında oluşur. `Interlocked` kullanılarak önlenebilir.  
- **Parallel.For / Foreach**: Verilen array içindeki item’ları farklı thread’lerde çalıştırır.  


### PLINQ (Parallel Language Integrated Query)

- LINQ sorgularını birden fazla thread içinde çalıştırır.  
- **AsParallel()** metodu ile yapılır.  
- **IEnumerable** dönen sorgular: Veritabanına gidip tüm datayı çeker. Ardından Where koşulunu kod üzerinde çalıştırır. (Performans sorunu)  
- **IQueryable** dönen sorgular: Where koşulunu SQL üzerinde çalıştırır.  
- **ForAll**: Foreach gibidir ancak parallel çalışır. Parallel ise foreach yerine bu kullanılmalıdır.  
- Parallel LINQ’da elinde var olan (DB’den çekilmiş) data üzerinde işlem yapılır. DB’den çekerken parallel değildir. Bu nedenle DB’den tüm datayı çeker.  
- **WithDegreeOfParallelism**: İşlemci sayısını belirler.  
- **WithExecuteMode**: Parallel çalışmayı zorunlu kılar.  
- **AsOrdered**: Parallel olduğunda sıralamayı korur.  

---

## Redis (Remote Dictionary Service) / In-Memory Caching

- **In-Memory Cache**: RAM’de tutulan veri.  
- **Distributed Cache**: Veriler ayrı bir cache servisinde tutulur. Veri tutarsızlığı önlenir.  
  Cache direkt server’da tutulursa birden fazla server olduğunda tutarsızlık oluşur.  
- **Redis**: Bir distributed cache sunucusudur ve NoSQL veritabanıdır. Çok sık güncellenmeyen ve yoğun olarak `SELECT` edilen veriler cache’lenmelidir.

---

## Asp.Net Core Güvenlik
* **IDataProtector:** DI olarak eklenerek request querystring’te id gibi verilerin görünmesini önler.

* **Cross Site Resource Sharing (CORS):** Api’ye farklı domainlerden istek gelmesine izin vermek gereklidir. services.AddCors() ile belirli ya da tüm domain’lerden gelen istekleri kabul etmesi sağlanır. Aksi halde istek default olarak reddedilir. Bu durumda tarayıcı konsolunda No Access-Control-Allow-Origin hatası görülür.
 
* **Güvenlik Açıkları:**
    * **Cross Site Scripting (XSS):** Html.Raw() kullandığında oluşur. Uygulamaya değil kullanıcıya saldırıdır. HtmlSanitizer library ile inputlar temizlenebilir. Ayrıca HtmlEncoder, JavascriptEncoder, UrlEncoder DI olarak eklenerek Encode metodu ile kötü amaçlı kodlar temizlenebilir.
    * **Cross Site Request Forgery (CSRF):** Formlar saldırganın sitesi üzerinden post edilir. Saldırgan araya girer. Sahte linklere tıklandığında yaşanabilir. Asp.net üzerinde RequestVerificationToken default gelerek bu saldırı önlenir. Post action’larında [ValidateAntiForgeryToken] attribute kullanılmalıdır.
    * **Open Redirect Attacks:** ReturnUrl kısmına başka bir site verilerek kullanıcı verilerinin çalınmasıdır. ReturnUrl’in kendi url’imiz olduğunu kontrol etmeliyiz. `if(Url.IsLocalUrl(url))` 
    * **HTTPS:** Client – server arası verilerin şifrelenmesidir. (SSL sertifikası ile) 
    * **HSTS (Http Strict Transport Security):** Sertifikayı cache’lemek için gereklidir.
    * **UseHttpsRedirection()** metodu http istekleri https’e yönlendirir.
    * **UseHsts()** metodu HSTS için gereklidir.

---

## Asp.Net Inversion of Control (IoC)
- Class bağımlılıklarını azaltmaya yönelik prensiplerdir.
- **Dependency Inversion:** Interface – abstract kullanarak soyutlama prensibi.
- **Dependency Injection:** Bir pattern’dir (tasarım kalıbı).
- **Inversion of Control Container:** IoC prensiplerini uygulayan framework’lerdir.
- **Singleton:** Nesne bir kere oluşturulur. 
- **Scoped:** Request başı bir nesne oluşturulur.
- **Transient:** Her injection için bir nesne oluşturulur.

---

## Sık Kullanılan NuGet Paketleri

- **AspNetCoreRateLimit**: Denial of Service saldırısını bu library ile önleyebiliriz. IP rate limit ve client ID rate limit uygulanabilir.
- **Smidge**: ASP.NET Core için bundling-minification library. CSS ve JavaScript’leri sıkıştırmada kullanılabilir.
- **Hangfire**: Windows service veya task scheduler kullanmaya gerek kalmadan job oluşturmaya yarayan library’dir. Küçük projelerde RabbitMQ yerine kullanılabilir. Job’lar veritabanına kayıt edildiği için uygulama kapatılsa da tekrar ayağa kalktığında devam eder. Zamanlanmış, tek seferlik veya sıralı job’lar oluşturulabilir.
- **NLog**: Loglama için library. DB ya da file’a loglama yapabilir.