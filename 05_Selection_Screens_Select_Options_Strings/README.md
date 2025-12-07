# 📘 04. Selection Screen & Select Options & Strings

## Bu bölümde hangi konular olacak ? 

Bu bölümde, ABAP raporlarının kullanıcıdan giriş almasını sağlayan **Selection Screen* yapılarını, **Select Options** işlemlerini ve **STRING** manipülasyonlarını ele alacağız. 

---

# 🧩 SELECTION SCREEN (Seçim Ekranı) İşlemleri 

**Selection Screen**, bir ABAP raporu çalıştırıldığında kullanıcıdan parametre veya kriter girmesini sağlayan bir SAP arayüzüdür. 

## :fleur_de_lis: Tekil Değer Alımı: PARAMETERS

**PARAMETERS** komutu, Selection Screen'de kullanıcıdan tek bir değer (tek bir alan) almak için kullanılır. Alınan değer metin, tarih veya sayı biçiminde olabilir.

| Komut | Açıklama | Kullanımı | 
| --- | --- | --- |
| PARAMETERS | Tekil giriş alanı oluşturur. | PARAMETERS p_kunnr TYPE kunnr. |
| DEFAULT | Alan için varsayılan bir değer atar. | PARAMETERS p_tarih TYPE sy-datum DEFAULT sy-datum. |
| AS CHECKBOX | Boolean değer almak için onay kutusu oluşturur. | PARAMETERS p_test AS CHECKBOX. |
|RADIOBUTTON GROUP | Belirli bir grup içindeki seçeneklerden sadece birinin seçilebilmesini sağlar. | PARAMETERS r_opt1 RADIOBUTTON GROUP grp1 DEFAULT 'X'.|






