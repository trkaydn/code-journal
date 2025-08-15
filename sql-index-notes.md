# Sql Index Notları 

*08 Ocak 2024 - Tarık Aydın*

- **SET STATISTICS IO ON** → Sorgu çalışırken ne kadar okuma yaptığını detaylı gösterir.  
- **sp_spaceused** → Tablo ve indexlerin ne kadar depolama kullandığını gösterir.  

## Index Türleri

- **Clustered Index** → Datanın tabloya fiziken yazılma sırasıdır. Genellikle Primary Key ile oluşur. Sadece 1 tane olabilir.  
- **Non-Clustered Index** → Primary key dışındaki, istediğin sütuna göre sıralanmış ikinci bir tablodur. Sadece index key columns’da seçilen sütunları içerir.  
- **Included Columns** → Çekilecek sütunları index’e ekler. Lookup yapmaya gerek kalmaz. Eklerken depolama açısından dikkat edilmeli.  

## Scan Türleri

- **Table Scan** → En kötü arama şeklidir. Primary key veya index yokken yapılır, tüm tabloyu tarar.  
- **Index Scan** → Primary key ile yapılan arama.  
- **Index Seek** → Eklediğin Non-Clustered index üzerinden yapılan arama. En performanslı yöntemdir.  

## Fragmentation ve Page

- **Fragmentation** → Yeni veri geldikçe index’in sonuna eklendiği için indexlerin zamanla bozulmasıdır. Rebuild edilmesi gerekir. Rebuild sırasında tablo kilitlenir.  
- **Index Page** → Tablonun sayfalarına belirli aralıklarla boşluk bırakır. Yeni kayıt en yakın boşluğa eklenir, fragmentation süresini uzatır.  
  - **Fillfactor** → Sayfanın yüzde kaçı boş bırakılacağını belirler. 80-90 civarı best practice olarak önerilir.  

## Maintenance

- SQL Server Management Studio içindeki **Maintenance Plan** ile stored procedure’ler, index rebuildler vb. job olarak ayarlanabilir.