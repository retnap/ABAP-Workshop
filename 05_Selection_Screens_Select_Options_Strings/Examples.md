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
