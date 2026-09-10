# ADR: Deployment Stratejisi

## Bağlam
- Canlıda çalışacak ticari bir site
- Otomatik TLS sertifikası şart (müşteri teknik bakım yapamaz)
- Aylık maliyet öngörülebilir olmalı (usage-based fatura riski istemiyorum)
- Full infrastructure kontrolü istiyorum (custom header, reverse proxy kuralları)

## Değerlendirilen Seçenekler

| Seçenek | Avantajlar | Dezavantajlar |
| --- | --- | --- |
| **Vercel** | Sıfır konfigürasyon, otomatik preview | Vendor lock-in, fiyat öngörülemez, Next.js dışı kontrol kısıtlı |
| **Netlify** | Kolay deploy, form handling | SSR desteği sınırlı, cold start sorunu |
| **VPS + Container + Reverse Proxy** | Tam kontrol, sabit maliyet, custom güvenlik | Bakım yükü, manuel ölçekleme |

## Karar
**VPS + multi-stage container build + reverse proxy** kullanılmasına karar verildi. CI/CD pipeline ile push-to-deploy otomasyonu yapılacak.

## Gerekçe
Bu yöntemle tam kontrol sağlanır, maliyetler sabit tutulur.

## Sonuç
Sabit aylık maliyet, otomatik TLS yenileme, güvenlik başlıkları üzerinde tam kontrol, container sayesinde ortam tutarlılığı. Trade-off: Yatay ölçekleme manuel, ama proje ölçeğinde gereksiz.
