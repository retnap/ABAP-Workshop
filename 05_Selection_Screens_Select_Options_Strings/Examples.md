# Senaryo 1 - Kullanıcıdan Malzeme Listesi Alıp Stok Listesi Getirme 

Kullanıcı malzeme numarasını selection screen'e gider -> sistem o malzemeye ait depo bazlı stokları listeler. 

Bu işlem MM - IM (Inventory Management) alanında en sık yapılan sorgudur. 

## Kullanılan DB Tabloları 

### 🗄️ MARA 

+ Malzeme Ana Verisi (malzeme gerçekten var mı kontrolü için)

| Field | Data Element | Açıklama |
| --- | --- | --- |
| MATNR | MATNR | Malzeme Numarası | 
  

### 🗄️ MARD 

+ Malzeme depo seviyesi stok tablosu

| Field | Data Element | Açıklama |
| --- | --- | --- | 
| MATNR | MATNR | Malzeme Numarası | 
| WERKS | WERKS_D | Fabrika | 
| LGORT | LGORT_D | Depo Yeri | 
| LABST | LABST | Kullanılabilir Stok | 

---

# Senaryo 2 - Radio Button ile Raporlama Türü Seçme 

Kullanıcı radio button ile "Malzeme Ana Veri" veya "Stok Verisi" seçer.

## Kullanılan DB Tabloları

### 🗄️ MARA - Malzeme Ana Verisi

### 🗄️ MARD - Depo Stokları 


---

# Senaryo 3 - Kullanıcıdan Metin Alıp String İşlemleri Yapan Rapor 

Kullanıcı bir metin girer, sisteme CONDENSE, FIND, SHIFT gibi işlemleri gösterir. 

---

# Senaryo 4 - Malzeme Açıklamasında Arama Yapan STRING + Selection Screen Raporu 

Bu rapor SAP'de **material search exit**, **data cleaning** ve **ana veri migrasyonu** projelerinde çok kullanılır. 

Kullanıcı:

+ Aranacak metni girer

+ Program tüm malzeme açıklamalarında (MAKT table) arama yapar

+ İçinde o kelime geçen malzemeleri listeler

## Kullanılan DB Tablosu 

### 🗄️ MAKT - Malzeme Açıklamaları 

| Field | Data Element | Açıklama |
| --- | --- | --- | 
| MATNR | MATNR | Malzeme Numarası |
| MAKTX | MAKTX | Malzeme Kısa Açıklaması | 
| SPRAS | SPRAS | Dil (TR, EN, DE..) |


---


# Senaryo 5 - Tarih Aralığına Göre Satış Siparişleri Listeleme 

Kullanıcı **sipariş belge tarih aralığı** girer -> sistem o aralıkta oluşturulan satış siparişlerini listeler. 

Bu tür raporlar ERP SD modülünde çok yaygındır. 

## Kullanılan DB Tablosu 

### 🗄️ VBAK - Satış Siparişi Header 

| Field | Data Element | Açıklama |
| --- | --- | --- |
| VBLEN | VBLEN_VA | Sipariş Numarası |
| ERDAT | ERDAT | Oluşturma Tarihi | 
| AUART | AUART | Belge Türü | 
| KUNNR | KUNNR | Müşteri Numarası | 



