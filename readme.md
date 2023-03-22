# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

# Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

    1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
    insert into yazar (yazarad, yazarsoyad) values("KEMAL", "UYUMAZ")


    2) Biyografi türünü tür tablosuna ekleyiniz.
    insert into tur(turadi)
    values("Biyografi")

    3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.
    insert into ogrenci (ograd,ogrsoyad,cinsiyet,sinif)
    values("Çağlar","Üzümcü","E","10A") ,("Leyla","Alagöz","K","9B"),("Ayşe","Bektaş","K","11C")

    4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.
    insert into yazar (yazarad,yazarsoyad) select ograd,ogrsoyad from ogrenci order by rand() limit 1

    5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
    insert into yazar(yazarad,yazarsoyad) select ograd,ogrsoyad from ogrenci where ogrno between 10 and 30

    6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
    (Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
     INSERT INTO yazar (yazarad,yazarsoyad) VALUES ("Nurettin","Belek")
     SELECT @@IDENTITY

    7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
    update ogrenci set sinif="10C" where ogrno="3"

    8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
    update ogrenci set sinif="10A" where sinif="9A"

    9) Tüm öğrencilerin puanını 5 puan arttırın.
    set sql_safe_updates = 0;
    UPDATE ogrenci SET puan = puan + 5;

    10) 25 numaralı yazarı silin.
    delete from yazar where yazarno=25

    11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
    select * from ogrenci
    where dtarih is null

    12) Doğum tarihi null olan öğrencileri silin.
    delete from ogrenci where dtarih is null

    13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
    update kitap set puan= puan+2 where kitapadi like "A%"

    14) Kişisel Gelişim isimli bir tür oluşturun.
    insert into tur (turadi) values("Kişisel Gelişim");

    15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
    UPDATE kitap SET turno = (SELECT turno from tur where turadi = "Kişisel Gelişim") WHERE kitapadi ="Başarı Rehberi"


    16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
    create procedure ogrencilistesi as select * from ogrenci;
    exec ogrencilistesi;

    17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
    create procedure ekle @ograd nvarchar(30), @ogrsoyad nvarchar(30), @sinif nvarchar(15)
    as (begin)
    insert into ogrenci (ograd,ogrsoyad,sinif)
    values (@ograd,@ogrsoyad,@sinif)
    go;

    18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
    create procedure sil @ogrno
    as
    delete from ogrenci where ogrno = @ogrno
    go;

    19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.
    create procedure adsoyadbirlesiklistele as select concat(ograd, ogrsoyad) as adsoyad from ogrenci;
    exec adsoyadbirlesiklistele

    20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
    create procedure search
    as
    select concat(ograd,ogrsoyad) as adSoyad from ogrenci

    21) Daha önceden oluşturduğunu tüm prosedürleri silin.
    drop procedure[procedurler];

    #Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
    22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
    select * from kitap where turno in(select turno from tur where turadi="Dram")

    23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
    select * from kitap where yazarno ın (select yazarno,yazarad from yazar where yazarad like "E%")

    24) Kitap okumayan öğrencileri listeleyiniz.
    select * from ogrenci where ogrno not in (select ogrno from islem)

    25) Okunmayan kitapları listeleyiniz
    select * from kitap where kitapno not in (select kitapno from islem );

    26) Mayıs ayında okunmayan kitapları listeleyiniz.
    select * from kitap where kitapno in (select kitapno from islem where atarih not like "%-05-%")
