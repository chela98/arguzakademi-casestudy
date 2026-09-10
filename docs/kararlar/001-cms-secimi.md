# ADR: CMS Seçimi

## Bağlam
- Müşterinin teknik bilgisi yok, içerik güncellemesini kendisi yapabilmeli
- SEO kritik (Google sıralaması)
- Geliştirici olarak TypeScript tip güvenliği istiyorum
- İçerik değiştiğinde site anlık güncellensin
- Kod tabanında hardcoded metin bırakılmadan tüm buton, etiket ve mikro kopyaların (microcopy) tek panelden yönetilebilmesi

## Değerlendirilen Seçenekler

| Seçenek | Avantajlar | Dezavantajlar |
| --- | --- | --- |
| **WordPress** | Tanıdık, eklenti ekosistemi | Yavaş, güvenlik açıkları, monolitik, PHP |
| **Strapi** | Açık kaynak, self-host | Ek sunucu maliyeti, ayrı bakım, daha az olgun |
| **Contentful** | Enterprise-grade, CDN | Pahalı, Türkçe arayüz yok, vendor lock-in güçlü |
| **Sanity** | Gömülü Studio, GROQ, TypeGen, webhook | Vendor bağımlılığı, ücretsiz katman sınırı |

## Karar
**Sanity** kullanılmasına karar verildi.

## Gerekçe
Gömülü Studio ile ayrı deploy gerektirmiyor, GROQ ile esnek sorgulama, TypeGen ile derleme zamanı tip güvenliği, webhook entegrasyonu ile anlık cache invalidation sağlıyor.

## Sonuç
İçerik güncelleme süresi günlerden saniyelere düştü. Müşteri tek panelden yönetiyor. Singleton doküman deseni ile kritik sayfaların (site ayarları, hakkımızda) yanlışlıkla silinmesi engellendi.
