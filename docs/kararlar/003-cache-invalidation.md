# ADR: Cache Invalidation

## Bağlam
- İçerik editörü CMS'te bir seans veya etkinlik güncellediğinde, ziyaretçi eski içerik görmemeli
- Tam site rebuild uzun sürüyor ve gereksiz
- Sadece değişen sayfaların güncellenmesi lazım

## Değerlendirilen Seçenekler

| Seçenek | Avantajlar | Dezavantajlar |
| --- | --- | --- |
| **Tam rebuild (SSG)** | Basit, tutarlı | Dakikalarca sürer, CMS'te her değişiklikte tetiklenir |
| **ISR (zamanlayıcı)** | Kolay kurulum, otomatik | Gecikme var (60s-3600s), anlık değil |
| **Webhook + tag-based revalidation** | Anlık, cerrahi, sadece etkilenen sayfalar | Webhook altyapısı kurmak lazım, imza doğrulama gerekiyor |

## Karar
**Webhook + tag-based revalidation** uygulanmasına, ISR zamanlayıcısının güvenlik ağı olarak bırakılmasına karar verildi.

## Gerekçe
Tag stratejisi kullanılarak sadece etkilenen kısımlar revalidate edilecektir:
- Her içerik tipi kendi tag'ine sahip (örn: `seans`, `etkinlik`)
- Detay sayfaları ek granüler tag alıyor (örn: `seans-{slug}`)
- Cascade kuralları: Bir eğitmen güncellendiğinde, onu referans eden tüm içerik tipleri invalidate ediliyor
- Güvenlik ağı: Webhook gelmese bile periyodik revalidation devrede

## Sonuç
İçerik güncellemesi 2 saniyenin altında canlıya yansıyor. Tam rebuild yok. Sadece etkilenen sayfalar tazeleniyor.
