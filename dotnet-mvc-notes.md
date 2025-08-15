# .NET MVC (Model-View-Controller) Notları

*24 Ekim 2021 - Tarık Aydın*

## Views
- Views içindeki **Shared** klasörü, Layout ve master page’leri içerir. Paylaşılan page’lerin isimleri `_` ile başlar.  
- `RenderBody` metodu, layout’lar arasını sayfa içeriği (değişen kısım) ile doldurur. Layout, tüm sitede sabit olan kısımlardır. `RenderBody` metodu `_Layout.cshtml` dosyasında çağırılır.  
- `_ViewStart` dosyasında default `_Layout` dosyası belirtilir.  
- **ViewModel**, birden fazla model gönderirken model class’larını viewmodel’de prop olarak tanımlar.  
- **Razor** → view’da C# kodu yazmaktır. `@` işareti ile yapılır.

## Controller ve Model
- Controller, database’e bağlanarak bilgileri getirir. Bu bilgiler bir Model içerisine aktarılır ve view’a gönderilir. Model bir class’tır.  
- Controller’ın döndürdüğü Model, View’da da tanımlanmalıdır:  
```csharp
@model modeltipi
```
Örnek:  
```csharp
@model string[]
```
- **ViewBag** → Model dışında bilgi taşımak için kullanılır.  
```csharp
ViewBag.degisken = 1; // Controller
@ViewBag.degisken      // View
```
- Partial metodu overload olarak kullanılabilir: `(PartialViewAdı, Model)` — Model zorunlu değil.  
- Html.Action("actionadı","controlleradı")  
- Form oluşturmak için: `using (Html.BeginForm()) { … }`  

## Entity Framework
- Veritabanı işlerini yerine getirmek için hazır kütüphane. NuGet üzerinden eklenir. ADO.NET yerine gelmiş ama ADO.NET altyapısını kullanır.  
- **Context** → projenin veritabanı ile bağlı kısmıdır. Database işlemleri context üzerinden yapılır.  
- Context class’ı `DbContext`’ten kalıtım alır ve tablolar property olarak eklenir. Context adı, connection string’in name kısmıyla aynı olmalıdır.  
```csharp
public DbSet<Kategori> Kategoriler { get; set; } // Liste yerine DbSet kullanılır
```
- Değişkenler kapsüllenmelidir (CTRL + R, E kısayolu).  

### Connection String
- App.Config veya Web.Config:  
```xml
<connectionStrings>
    <add connectionString="server=TARIK-PC;database=DenemeDb;integrated security=true;" providerName="System.Data.SqlClient" name="UrunBaglanti" />
</connectionStrings>
```
- Context class’ında constructor ile belirtilir:  
```csharp
public VeriContext() : base("UrunBaglanti") { }
```

### LINQ
- Language Integrated Query. Tek kayıt seçilecekse `FirstOrDefault` metodu kullanılır.  
- Bir tablonun primary key’i, ilişkili başka tabloda foreign key olur.  
- Entity’de tablo yaparken `Id` ismi özeldir. Farklı isim kullanılırsa `[Key]` eklenir.

### Katmanlı Mimari
1. **Entity Layer** → DBye karşılık gelen class’lar. `ProjeAdi.Entities` olarak class library oluşturulur.  
2. **DataAccessLayer (DAL)** → Veritabanı CRUD işlemleri ve veri return edilir. `ProjeAdi.Dal` olarak class library oluşturulur. **Entity Framework sadece bu katmanda kullanılır.**  
3. **BusinessLogic Layer (BLL)** → İş katmanı. Verilere kimlerin erişeceği buradadır, ekleme işleminde kontroller yapılır. DAL’dan dönen verileri return eden metotlar barındırır. `ProjeAdi.Bll` olarak class library oluşturulur.  
4. **Presentation Layer** → User Interface (MVC’nin bulunduğu kısım). `ProjeAdi.UI`. Bu katmanda veritabanı bağlantıları yapılmamalıdır.  

### MVC View Örnekleri
```csharp
var categories = db.tbl_categories.ToList().ToPagedList(page, 4);
return View(categories);
```
View’da:  
```csharp
@Html.PagedListPager((IPagedList)Model, page => Url.Action("Index", new { page }))
```

### Entity Framework Türleri
- DB First, Model First ve Code First  
- DataAnnotations veya Fluent API kullanılır. `[Required]`, `[StringLength]` DataAnnotation örnekleridir.

### Migration
- Database değişikliklerini günceller.  
- NuGet Console: `Enable-Migrations` yazılır, gelen metodun false’i true yapılır.  
- `Update-Database` komutu ile güncelleme yapılır.

### Tabloların S Takısını Kaldırma
Context içerisine eklenir:  
```csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    base.OnModelCreating(modelBuilder);
}
```

### Formlar ve Validation
- `Html.EditorForModel()` metodu, form içinde tüm inputları otomatik oluşturur. Label’lar için modele `[DisplayName]` verilebilir.  
- Unobtrusive Validation: NuGet’den `jquery.validation` ve `unobtrusive.validation` paketleri indirilir. Modele bir class eklenir: `KisiValidator : AbstractValidator<Kisi>`  

### İlişkiler ve Naming Convention
- İlişkiler için eklediğin modellerde `virtual` kullanırsan, örn. kitabın kategorisini de çeker.  
- Class içindeki değişkenler (field’ler) `_` ile başlar. Metot içinde camelCase kullanılır.  
- Property’ler PascalCase ile yazılır (ilk harfler büyük).  

### Örnekler ve Notlar
```csharp
Car bmwBase = new Bmw(); // Bmw, Car’dan kalıtım aldığı için nesne oluşturulabilir. Sadece Car özelliklerini barındırır.
```
- **Sealed Class** → Kalıtım vermeyen class.  
- **Struct** → Stack’te tutulur. Value type. Ctor’da tüm parametreler set edilmeli, boş ctor alınamaz. Max 16 byte performanslıdır.  
- **Dynamic** → JS’deki var mantığıyla aynı. Her türe dönüşebilen değişken. Tavsiye edilmez.  
- **Object** → int, string vb. tüm tipler object’e atanabilir.  

### .NET 6 Güncellemeleri
- **Startup.cs** kaldırıldı. .NET 5’te startup.cs içinde konfigürasyonlar yapılır ve program.cs'de bildirilirdi.  
- .NET 6’da tüm konfigürasyonlar **program.cs** içinde yapılmaktadır.  
- **Top Level Statements** → Using’ler, namespace vb. yazmaya gerek olmadan direkt yazdığın kod **main** metotta sayılır. Program.cs .NET 6’da bu şekilde yapılandırılmıştır.  
- .NET 6’da **WebApplication** ve **WebApplicationBuilder** türevleri geldi. .NET 5’te bunun karşılığı **IHostBuilder** ve **Host** idi. Bu yapı, uygulamayı ayağa kaldırmak için kullanılır.  
- Uygulama portları artık 5000 türevi yerine rastgele gelmektedir.