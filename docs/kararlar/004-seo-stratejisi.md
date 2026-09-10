# ADR: SEO Stratejisi

## Bağlam
- Organik arama trafiği müşterinin ana müşteri edinme kanalı
- Google Rich Results (zengin snippet) desteği isteniyor
- Her sayfanın kendine özel meta tag'leri olmalı
- Sitemap otomatik üretilmeli, elle güncellenmemeli
- Reklam bütçesi olmadan organik danışan çekmek için hem Inbound Blog hem de zengin snippet (Rich Results) gereksinimi

## Değerlendirilen Seçenekler

| Seçenek | Avantajlar | Dezavantajlar |
| --- | --- | --- |
| **Manuel meta tag'ler** | Basit | Ölçeklenmiyor, CMS ile senkronizasyon yok |
| **SEO eklentisi (Yoast tarzı)** | Tanıdık | Headless CMS'te çalışmıyor |
| **Programatik builder fonksiyonları** | Tam kontrol, test edilebilir, CMS verisiyle otomatik | Geliştirme süresi daha fazla |

## Karar
**Programatik yaklaşım** uygulanmasına karar verildi.

## Gerekçe
Uygulama detayları:
- 7 ayrı Schema.org JSON-LD builder fonksiyonu (Organization, Service, Event, BlogPosting, FAQPage, BreadcrumbList, Person)
- Hepsi saf (pure) fonksiyon — framework'e bağımlı değil, birim test edilebilir
- CMS'teki her içerik tipi için dinamik `generateMetadata()` ile OpenGraph desteği
- XML Sitemap CMS verilerinden otomatik üretiliyor (slug + son güncelleme tarihi)

## Sonuç
Google Rich Results'ta etkinlik ve SSS snippet'ları görünüyor. Her sayfanın meta description, OG image ve canonical URL'si CMS'ten otomatik geliyor.
