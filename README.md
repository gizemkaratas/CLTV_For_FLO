# FLO Müşteri Yaşam Boyu Değeri (CLTV) Tahmini

Bu proje, FLO satış ve pazarlama faaliyetleri için bir yol haritası belirlemek amacıyla mevcut müşterilerin gelecekte şirkete sağlayacakları potansiyel değeri (Customer Lifetime Value - CLTV) tahmin etmeyi amaçlamaktadır. CLTV hesaplamalarında BG/NBD ve Gamma-Gamma modelleri kullanılmıştır.

## Veri Seti

Veri seti, FLO'dan OmniChannel (hem online hem offline) alışveriş yapan müşterilerin geçmiş alışveriş davranışlarını içermektedir.

- **Gözlem Sayısı:** 19,945
- **Değişken Sayısı:** 13
- **Veri Seti Boyutu:** 2.7 MB

### Değişkenler

- `master_id`: Eşsiz müşteri numarası
- `order_channel`: Alışveriş yapılan kanal (Android, iOS, Desktop, Mobile)
- `last_order_channel`: En son alışverişin yapıldığı kanal
- `first_order_date`: Müşterinin yaptığı ilk alışveriş tarihi
- `last_order_date`: Müşterinin yaptığı son alışveriş tarihi
- `last_order_date_online`: Müşterinin online platformda yaptığı son alışveriş tarihi
- `last_order_date_offline`: Müşterinin offline platformda yaptığı son alışveriş tarihi
- `order_num_total_ever_online`: Müşterinin online platformda yaptığı toplam alışveriş sayısı
- `order_num_total_ever_offline`: Müşterinin offline platformda yaptığı toplam alışveriş sayısı
- `customer_value_total_ever_offline`: Müşterinin offline alışverişlerinde ödediği toplam ücret
- `customer_value_total_ever_online`: Müşterinin online alışverişlerinde ödediği toplam ücret
- `interested_in_categories_12`: Müşterinin son 12 ayda alışveriş yaptığı kategorilerin listesi
- `store_type`: Alışveriş yapılan mağaza tipi

## Sonuç:
CLTV hesaplamalarında BG/NBD ve Gamma-Gamma modelleri kullanıldı. Müşterileri CLTV ve diğer etkenlere göre A,B,C,D olmak üzere 4 segmente ayırdık ve bu segmentlere göre önümüzdeki 6 aylık periyod için tahminlemelerde bulunduk.


## CLTV Segment Analizi

| CLTV Segment | exp_sales_6_month (sum) | exp_sales_6_month (mean) | exp_sales_6_month (count) | exp_average_value (sum) | exp_average_value (mean) | exp_average_value (count) | cltv (sum) | cltv (mean) | cltv (count) |
|--------------|-------------------------|--------------------------|---------------------------|-------------------------|--------------------------|---------------------------|------------|-------------|--------------|
| D            | 4078.2891                | 0.8178                   | 4987                      | 492172.3700             | 98.6911                  | 4987                      | 400651.7650| 80.3392     | 4987         |
| C            | 5239.6134                | 1.0509                   | 4986                      | 659445.3297             | 132.2594                 | 4986                      | 689628.6583| 138.3130    | 4986         |
| B            | 5994.6736                | 1.2023                   | 4986                      | 837605.9521             | 167.9916                 | 4986                      | 994895.2183| 199.5377    | 4986         |
| A            | 7709.7059                | 1.5463                   | 4986                      | 1186769.1892            | 238.0203                 | 4986                      | 1806628.3449| 362.3402   | 4986         |



## Proje Adımları

### 1. Veriyi Hazırlama

- **Adım 1:********şirket özel bilgisi****
- **Adım 2:** Aykırı değerleri baskılamak için gerekli fonksiyonları tanımlayın (`outlier_thresholds` ve `replace_with_thresholds`).
- **Adım 3:** Aykırı değerleri baskılayın.
- **Adım 4:** Müşterilerin toplam alışveriş sayısı ve harcaması için yeni değişkenler oluşturun.

### 2. CLTV Veri Yapısının Oluşturulması

- **Adım 1:** Veri setindeki en son alışveriş tarihinden 2 gün sonrasını analiz tarihi olarak alın.
- **Adım 2:** CLTV hesaplamaları için gerekli olan `customer_id`, `recency_cltv_weekly`, `T_weekly`, `frequency`, ve `monetary_cltv_avg` değişkenlerini içeren bir DataFrame oluşturun.

### 3. BG/NBD ve Gamma-Gamma Modellerinin Kurulması ve CLTV Hesaplanması

- **Adım 1:** BG/NBD modelini fit edin.
- **Adım 2:** Gamma-Gamma modelini fit edin ve müşterilerin ortalama bırakacakları değeri tahminleyip `exp_average_value` olarak CLTV DataFrame'ine ekleyin.
- **Adım 3:** 6 aylık CLTV hesaplayın .

### 4. CLTV'ye Dayalı Müşteri Segmentasyonu ve Aksiyon Önerileri

- **Adım 1:** 6 aylık CLTV'ye göre müşterileri 4 gruba ayırın. Bu gruplar üzerinde inceleme yapabilir ve aksiyonlar alabilirsiniz.


