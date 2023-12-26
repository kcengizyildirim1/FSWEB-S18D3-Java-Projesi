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
	
		SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci AS o, islem AS i 
		WHERE o.ogrno=i.ogrno
		ORDER BY o.ogrno DESC
	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
		SELECT t.turadi, k.kitapadi FROM tur AS t, kitap AS k 
		WHERE t.turno=k.turno
		ORDER BY t.turadi ASC
	
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
	
		SELECT o.ogrno, o.ograd, o.ogrsoyad , k.kitapadi 
		FROM ogrenci AS o, islem AS i, kitap AS k 
		WHERE o.ogrno = i.ogrno AND i.kitapno = k.kitapno AND o.sinif IN ('10B', '10C')
		ORDER BY o.ogrno ASC
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
		SELECT o.ograd, o.ogrsoyad ,  i.atarih FROM ogrenci AS o 
		INNER JOIN islem AS i
		WHERE o.ogrno = i.ogrno
		ORDER BY o.ogrno ASC
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
		
		SELECT k.kitapadi  , t.turadi  FROM kitap AS k 
		INNER JOIN tur AS t ON k.turno=t.turno
		WHERE t.turadi IN ('fıkra', 'hikaye')
		ORDER BY t.turadi , k.kitapadi ASC
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
		SELECT o.ogrno , o.ograd, o.ogrsoyad, k.kitapadi FROM ogrenci AS o
		INNER JOIN islem AS i ON o.ogrno = i.ogrno
		INNER JOIN kitap AS k ON k.kitapno = i.kitapno
		WHERE o.sinif IN ("10B", "10C")
		ORDER BY o.ograd 

	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
		SELECT o.ograd , o.ogrsoyad, i.atarih FROM ogrenci AS o
		LEFT JOIN islem AS i ON o.ogrno = i.ogrno
	
	8) Kitap almayan öğrencileri listeleyin.
	
		SELECT o.ograd, o.ogrsoyad ,i.islemno  FROM ogrenci AS o
		LEFT JOIN islem AS i ON o.ogrno = i.ogrno
		WHERE i.islemno IS NULL
		ORDER BY o.ograd , o.ogrsoyad
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
		
		SELECT k.kitapno, k.kitapadi, COUNT(*) AS alim_sayisi
		FROM kitap AS k
		INNER JOIN islem AS i ON k.kitapno = i.kitapno
		GROUP BY k.kitapno, k.kitapadi
		ORDER BY k.kitapno ASC;
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

		SELECT k.kitapno , k.kitapadi , COUNT(*) AS alim_sayisi FROM kitap AS k
		LEFT JOIN islem AS i ON k.kitapno = i.kitapno
		GROUP BY k.kitapno , k.kitapadi

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
		SELECT o.ograd, o.ogrsoyad , k.kitapadi FROM ogrenci AS o
		INNER JOIN islem AS i ON o.ogrno= i.ogrno
		INNER JOIN kitap AS k ON k.kitapno = i.kitapno
		ORDER BY o.ograd, o.ogrsoyad
	
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
		SELECT o.ograd, o.ogrsoyad, k.kitapadi, y.yazarad, y.yazarsoyad, t.turadi, i.atarih
        FROM ogrenci AS o
        LEFT JOIN islem AS i ON o.ogrno=i.ogrno
        LEFT JOIN kitap AS k ON i.kitapno=k.kitapno
        LEFT JOIN yazar AS y ON k.yazarno = y.yazarno
        LEFT JOIN tur as t ON k.turno = t.turno
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	
		SELECT o.ograd, o.ogrsoyad , COUNT(k.kitapno) FROM ogrenci AS o
		INNER JOIN islem AS i ON o.ogrno = i.ogrno
		INNER JOIN kitap AS k ON i.kitapno= k.kitapno
		WHERE o.sinif IN ("10A" , "10B")
		GROUP BY o.ograd, o.ogrsoyad
	
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
		SELECT AVG(k.sayfasayisi)AS ortalama_sayfa FROM kitap AS k
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	
		SELECT k.kitapadi FROM kitap AS k 
		WHERE k.sayfasayisi > (SELECT AVG(k.sayfasayisi) FROM kitap AS k)
		
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
		SELECT COUNT(*)AS ogrenci_sayisi FROM ogrenci 
	
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	
		SELECT COUNT(*)AS ogrenci_sayisi FROM ogrenci 
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
		SELECT ograd, COUNT(ograd) FROM ogrenci
		GROUP BY ograd
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
		SELECT MAX(k.sayfasayisi) FROM kitap AS k
	
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
		SELECT k.kitapadi, k.sayfasayisi
		FROM kitap AS k
		ORDER BY sayfasayisi DESC
		LIMIT 1;
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
		SELECT MIN(k.sayfasayisi) FROM kitap AS k
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
		SELECT k.kitapadi , k.sayfasayisi FROM kitap AS k
		WHERE k.sayfasayisi = (SELECT MIN(k.sayfasayisi) FROM kitap AS k)
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
		SELECT  MAX(k.sayfasayisi) FROM kitap AS k
		INNER JOIN tur AS t
		WHERE t.turadi= "dram"
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	
		SELECT SUM(k.sayfasayisi) FROM kitap AS k
		INNER JOIN islem AS i ON i.kitapno = k.kitapno
		INNER JOIN ogrenci AS o ON o.ogrno = i.ogrno
		WHERE o.ogrno= "15"
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

		SELECT o.ograd , COUNT(*) FROM ogrenci AS o
		GROUP BY o.ograd 
	
	26) Her sınıftaki öğrenci sayısını bulunuz.
	
		SELECT  o. sinif , COUNT(*) FROM ogrenci AS o
		GROUP BY o.sinif 
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
		SELECT o.sinif, 
		SUM(CASE WHEN o.cinsiyet = 'E' THEN 1 ELSE 0 END) AS erkek_ogrenci_sayisi,
		SUM(CASE WHEN o.cinsiyet = 'K' THEN 1 ELSE 0 END) AS kiz_ogrenci_sayisi
		FROM ogrenci AS o
		GROUP BY o.sinif;
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
		SELECT o.ograd, o.ogrsoyad , SUM(k.sayfasayisi) AS toplam_sayfa FROM ogrenci AS o
		INNER JOIN islem AS i  ON i.ogrno = o.ogrno
		INNER JOIN kitap AS k ON k.kitapno = i.kitapno
		GROUP BY o.ograd, o.ogrsoyad
		ORDER BY toplam_sayfa DESC
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.

		SELECT o.ograd, o.ogrsoyad , COUNT(k.kitapadi)  FROM ogrenci AS o
		INNER JOIN islem AS i  ON i.ogrno = o.ogrno
		INNER JOIN kitap AS k ON k.kitapno = i.kitapno
		GROUP BY o.ograd, o.ogrsoyad
