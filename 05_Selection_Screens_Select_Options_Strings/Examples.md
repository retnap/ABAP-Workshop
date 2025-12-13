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


## Kod 

<img width="637" height="608" alt="01_Code" src="https://github.com/user-attachments/assets/4e3ad7f3-8010-4ac0-a9f1-a9c998c6f211" />

## Çıktılar 

<img width="807" height="146" alt="01_Print_1" src="https://github.com/user-attachments/assets/4db21187-add2-4cc9-b840-95d75acb7bc1" />

<img width="566" height="131" alt="01_Print_2" src="https://github.com/user-attachments/assets/63a6e3c7-f4ca-4b9f-a756-ae2c8df1c684" />


---

# Senaryo 2 - Radio Button ile Raporlama Türü Seçme 

Kullanıcı radio button ile "Malzeme Ana Veri" veya "Stok Verisi" seçer.

## Kullanılan DB Tabloları

### 🗄️ MARA - Malzeme Ana Verisi

### 🗄️ MARD - Depo Stokları 

## Kod

<img width="746" height="710" alt="02_Code" src="https://github.com/user-attachments/assets/c2b38f23-287e-4b06-b485-ce170fcb52b0" />

## Çıktılar 

<img width="813" height="203" alt="02_Print_1" src="https://github.com/user-attachments/assets/40c2afa3-cd49-4cb5-aff7-7d2907078159" />

<img width="538" height="143" alt="02_Print_2" src="https://github.com/user-attachments/assets/7d7dd4d2-8dbc-4435-a344-63a23a56d6fa" />

<img width="777" height="211" alt="02_Print_3" src="https://github.com/user-attachments/assets/3a6779b5-2816-428c-b753-d0ae703723d2" />

<img width="543" height="157" alt="02_Print_4" src="https://github.com/user-attachments/assets/1a19002c-1bc8-4ea5-a042-c269257aae00" />


---

# Senaryo 3 - Kullanıcıdan Metin Alıp String İşlemleri Yapan Rapor 

Kullanıcı bir metin girer, sisteme CONDENSE, FIND, SHIFT gibi işlemleri gösterir. 

## Kod 

<img width="607" height="392" alt="03_Code" src="https://github.com/user-attachments/assets/db5a4706-b9f0-4d9b-85dd-3c2cb768a21f" />

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

## Kod 

<img width="815" height="435" alt="04_Code" src="https://github.com/user-attachments/assets/3539e37c-5e7d-43e3-b730-211e3cb695cc" />


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


## Kod 

<img width="582" height="651" alt="05_Code" src="https://github.com/user-attachments/assets/1f709513-166e-4146-9b44-481fe823efb0" />

## Çıktılar 

<img width="842" height="146" alt="05_Print_1" src="https://github.com/user-attachments/assets/d425b97c-384d-4da1-97a4-2e31a4a11baa" />

<img width="535" height="876" alt="05_Print_2" src="https://github.com/user-attachments/assets/f5ba9aef-5640-442c-afda-a412f572fda9" />




