# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.

	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

	select o.ograd, o.ogrsoyad, i.atarih  from ogrenci o , islem i
	where o.ogrno = i.ogrno
	order by o.ogrno

	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	select * from kitap k, tur t
	where k.turno = t.turno
	having t.turadi in ('Fıkra', 'Hikaye')

	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
	
	select o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi from ogrenci o, islem i, kitap k
	where o.ogrno = i.ogrno and i.kitapno = k.kitapno and (o.sinif = '10B' or o.sinif = '10C')

	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
	select o.ograd, o.ogrsoyad, i.atarih from ogrenci o
	join islem i on o.ogrno = i.ogrno

	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	select k.kitapadi, t.turadi from kitap k
	join tur t on k.turno = t.turno
	where t.turadi = 'Fıkra' or t.turadi = 'Hikaye'
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
	select o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi from ogrenci o
	left join islem i on o.ogrno = i.ogrno
	left join kitap k on i.kitapno = k.kitapno
	where o.sinif = '10B' or o.sinif = '10C'
	order by o.ograd 

	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
	select o.ograd, o.ogrsoyad, i.atarih from ogrenci o
	left join islem i on o.ogrno = i.ogrno
	left join kitap k on i.kitapno = k.kitapno

	8) Kitap almayan öğrencileri listeleyin.
	
	select o.ograd, o.ogrsoyad, i.atarih from ogrenci o
	left join islem i on o.ogrno = i.ogrno
	left join kitap k on i.kitapno = k.kitapno
	where i.atarih is null

	select ograd, ogrsoyad from ogrenci
	where ogrno not in (select ogrno from islem)
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	
	select k.kitapadi, count(i.islemno) 'Okunma Sayisi' from kitap k
	join islem i on k.kitapno = i.kitapno
	group by k.kitapadi
	order by k.kitapno 
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

	select k.kitapadi, count(i.islemno) 'Okunma Sayisi' from kitap k
	left join islem i on k.kitapno = i.kitapno
	group by k.kitapadi
	order by k.kitapno 

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
	select o.ograd, o.ogrsoyad, k.kitapadi from ogrenci o
	left join islem i on o.ogrno = i.ogrno
	left join kitap k on i.kitapno = k.kitapno
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
	select o.ograd, o.ogrsoyad, k.kitapadi, y.yazarad, y.yazarsoyad, t.turadi, i.atarih from ogrenci o
	left join islem i on o.ogrno = i.ogrno
	left join kitap k on i.kitapno = k.kitapno
	left join yazar y on y.yazarno = k.yazarno
	left join tur t on t.turno = k.turno
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	
	select o.ograd, o.ogrsoyad, count(i.islemno) from ogrenci o
	left join islem i on o.ogrno = i.ogrno
	where o.sinif = '10A' or o.sinif = '10B'
	group by o.ogrno 
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG

	select avg(k.sayfasayisi) from kitap k
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	
	select k.kitapadi  from kitap k
	where k.sayfasayisi > (select avg(k.sayfasayisi) from kitap k)

	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
	select count(o.ogrno) from ogrenci o
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	
	select count(o.ogrno) 'Toplam Sayi' from ogrenci o
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
	select count(distinct(o.ograd)) 'Kac Farki Adda Ogrenci Var' from ogrenci o
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	select k.sayfasayisi  from kitap k
	order by k.sayfasayisi desc
	limit 1
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	select k.kitapadi, k.sayfasayisi  from kitap k
	order by k.sayfasayisi desc
	limit 1

	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	select k.sayfasayisi  from kitap k
	order by k.sayfasayisi
	limit 1

	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	select k.kitapadi, k.sayfasayisi  from kitap k
	order by k.sayfasayisi
	limit 1
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
	
	select k.sayfasayisi  from kitap k
	join tur t on k.turno = t.turno
	where t.turadi = 'Dram'
	order by k.sayfasayisi desc
	limit 1
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	
	select sum(k.sayfasayisi) from ogrenci o
	left join islem i on o.ogrno = i.ogrno
	left join kitap k on i.kitapno = k.kitapno
	where o.ogrno = 15
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

	select o.ograd, count(o.ograd) from ogrenci o
	group by o.ograd
	
	26) Her sınıftaki öğrenci sayısını bulunuz.
	
	select o.sinif, count(o.sinif) from ogrenci o
	group by o.sinif

	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
	select o.sinif, count(o.ogrno) from ogrenci o
	group by o.sinif, o.cinsiyet
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
	select o.ograd, o.ogrsoyad, sum(k.sayfasayisi) from ogrenci o
	left join islem i on o.ogrno = i.ogrno
	left join kitap k on i.kitapno = k.kitapno
	group by o.ogrno

	29) Her öğrencinin okuduğu kitap sayısını getiriniz.

	select o.ograd, o.ogrsoyad, count(o.ogrno) from ogrenci o
	left join islem i on o.ogrno = i.ogrno
	left join kitap k on i.kitapno = k.kitapno
	group by o.ogrno
