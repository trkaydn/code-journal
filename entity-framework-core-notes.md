# Entity Framework Core Notları 

*28 Ocak 2024 - Tarık Aydın*

- Entity Framework Core Kurulum
  + Microsoft.EntityFrameworkCoreTools Paketi (Code First için)  
  + Microsoft.EntityFrameworkCore Paketi  
  + Microsoft.EntityFrameworkCore.SqlServer Paketi  
  + NuGet console: `Scaffold-DbContext "ConnectionString"`

* Database first kullanmak gerekiyorsa scaffold-dbcontext komutu ile context ve entity’ler otomatik oluşturulur.  
- Code First:  
	+ Migration’lara manuel olarak müdahale edilmemelidir.  
	+ Migration’ların sırası önemlidir.  
	+ Önceki migration’a dönmek için: update-database mig_adi  
	+ Sadece en son migration silinebilir. Aradan silme yapılmaz.  
	+ script-migration komutu migration’ı SQL sorgusu olarak verir.  
	+ Default transaction’da SaveChanges metodunda bir hata olursa tüm değişiklikler geri alınır.  

- ChangeTracker:  
	+ ToList() ile data çekilip herhangi bir silme/güncelleme yapılmayacaksa track edilmemelidir. AsNoTracking metodu kullanılmalıdır. `context.Products.AsNoTracking().ToList();`  
	+ Memory’de track edilen nesnelere ChangeTracker üzerinden erişilebilir.  
	+ DataAnnotations tag’leri tercih edilmemelidir. Konfigürasyonlar Context üzerinde OnModelCreating metodunda Fluent API ile ayarlanmalıdır.  
 	+ Entity Framework default olarak bir veriyi silerken ilişkili tablolardan da verileri siler.  
	+ Bire bir ilişkide primary key, foreign key olarak kullanılabilir, ekstra bir ID tutmaya gerek yoktur.  
	+ AsNoTracking() yerine global olarak track kapatılabilir. Bu durumda update metodunu çağırmadan veri güncellenmez. `UseSqlServer().UseQueryTrackingBehavior();` metodu ile kapatılabilir.  

- İlişkili Verileri Çekme:  
	+ Eager Loading: Include() ve ThenInclude() metotlarıyla yapılır.  
	+ Explicit Loading: Eğer ilişkili veri, ana veriyi çektikten daha sonra lazım olduysa kullanılır. Bire çok ise Collection(), bire bir ise Reference() metodu ile yapılır. `context.Entry(entity).Collection(x => x.PropName).Load()`  
	+ Lazy Loading: Ef.Proxies library kullanılarak yapılır. Lazy loading’de navigation property’ler virtual olmalıdır. Navigation property’ler ilk sorguda yüklenmez. Eğer kodda çağırırsan yükler.  

- Entity Framework’te Inheritance:  
 	+ Table per hierarchy: Base class’ı DbSet olarak eklemeyip child’ları eklersen, child’daki property’ler ve base’deki property’ler child tablolara sütun olarak eklenir. Base ve child’ların hepsi DbSet olarak tanımlanırsa, veritabanında sadece base tablo oluşur ve child’lardaki sütunlar eklenir. “Discriminator” sütunu oluşur ve entity tipini tutar. Ortak olmayan sütunlar null atanır.  
	+ Table per type: Her entity’e karşı bir tablo oluşur. Base tablo ile child tablolar ID ile many-to-many ilişki kurar.  

- Entity Framework’te Entity Types:  
 	+ **Owned entity type:** Inheritance benzeri (alternatif).  
 	+ **Keyless entity type:** Primary key yoktur. Track edilmez. CRUD yapılmaz. Custom SQL sorgularında kullanılabilir.  
 	+ **FromSqlRaw:** Custom SQL sorgusu çalıştırmaya yarayan metottur.  
 	+ **Unicode:** False olursa varchar olur. Default olarak true’dur (nvarchar).  

- Entity Framework’te Index’ler:  
 	+ OnModelCreating()’de HasIndex() metodu kullanılabilir.  
 	+ Where koşulunda kullanılan sütunlar indexlenir.  
 	+ Index’ler storage maliyetini artırır.  
 	+ Included columns: Select’te çekilen sütunlara karşılık gelir.  

- Entity Framework’te Join:  
 	+ Eğer navigation property yoksa join yapılır.  
 	+ Join() metodu ile yapılır. `From x in …` ile başlanır. (Query syntax)  

- Entity Framework’te SQL Sorgusu Yazma:  
 	+ FromSqlRaw() veya FromSqlInterpolated() ile çekilecek model DbSet olarak tanımlanmalıdır. Bu durumda migration ekleyince DB’ye tablo olarak eklemeye çalışacaktır. Bunu engellemek için ToView() metodunu kullanabiliriz. Ya da migration içindeki Up() metodunu düzenleyebiliriz.  
 	+ ModelBuilder’de model.ToSqlQuery() kullanılabilir.  
 	+ ModelBuilder’de model.ToView kullanılabilir. View önce veritabanında manuel olarak oluşturulmalıdır.  
 	+ HasQueryFilter: Tüm sorgularda geçerli filter yazmaya yarar. `isDeleted = false` gibi.  
 	+ Query Tag: TagWith() metodu ile sorguya yorum satırı ekler. SQL tarafında sorgunun nereden geldiğini görmek için kullanılabilir. Ya da IQueryable dönen metotlardan sonra where kullanıldığında koşul SQL sorgusuna eklenir. Performanslı bir yöntemdir.  
 	+ DTO (Data Transfer Object): MVC’deki ViewModel ile aynıdır.  

- Entity Framework’te AutoMapper:  
 	+ Entity Framework’te tüm sütunları çekip sonradan map’lersek gereksiz veri çekmiş oluruz. ProjectTo() metodu ile bunu önleyebiliriz. `context.Products.ProjectTo(dto).ToList()`  

- Entity Framework’te Transaction:  
 	+ SaveChanges()’te hata oluşursa default davranış olarak tüm değişiklikleri rollback eder.  
 	+ Birden fazla SaveChanges() kullanılacaksa transaction kullanılmalıdır. Try-catch bloğuna gerek kalmaz.  
        
        ```csharp
        using (var transaction = context.Database.BeginTransaction())
        {
            context.SaveChanges();
            transaction.Commit();
        }
        ```

- Entity Framework’te Isolation:  
 	+ Eş zamanlı okuma işlemlerinde veri tutarlılığını ayarlamak içindir.  
 	+ **Read uncommitted:** Commit edilmemiş dataları okur.  
 	+ **Read committed:** Commit edilmiş dataları okur. Default davranıştır.  
 	+ **Repeatable read:** Commit edilmiş dataları okur. Okurken başka bir transaction update–delete yapamaz.  
 	+ **Snapshot:** Transaction sırasında CRUD yapabilir. Güncel transaction’a bir değişiklik yansımaz.  
 	+ **Serializable:** Repeatable ile benzerdir. Tek farkı commit edilmeden insert de yapılamaz.  
 	+ Aşağı doğru indikçe tutarlılık artar, performans düşer (DB lock nedeniyle), transaction’lar birbirini bloklar, eş zamanlı okuma azalır.  

- Entity Framework’te Concurrency (Eş Zamanlılık):  
 	+ Bir datayı farklı kişiler güncellemek isterse versiyonlamak gereklidir.  
 	+ RowVersion isminde byte[] tipinde property eklenir. Fluent API ile ayarlanır. Kullanıcı update yaparken mevcut versiyon değişmiş ise DbConcurrencyException patlatır. Handle edip hata mesajı döndürülür.  