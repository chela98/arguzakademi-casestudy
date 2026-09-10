# ADR: Component Mimarisi

## Bağlam
- 35+ bileşen var, organizasyon şart
- Server Component ve Client Component ayrımı bilinçli yapılmalı (performans)
- İş mantığı bileşenlerden bağımsız test edilebilmeli

## Değerlendirilen Seçenekler

| Seçenek | Avantajlar | Dezavantajlar |
| --- | --- | --- |
| **Düz yapı (components/)** | Basit | 35+ dosyada kaos, sorumluluk karmaşası |
| **Feature-based (features/auth/)** | Domain odaklı | Bu projede feature'lar net ayrışmıyor |
| **Atomic Design** | Net hiyerarşi, ölçeklenebilir | Öğrenme eğrisi, bazı bileşenler hangi katmana gider tartışması |

## Karar
**Atomic Design + Server Component varsayılanı** mimarisinin kullanılmasına karar verildi.

## Gerekçe
Katman tanımları:
- **Atoms**: Tekrar kullanılabilir en küçük birimler (Logo)
- **UI**: Shadcn/Base UI tabanlı stil bileşenleri (Button, Dialog, Calendar, Select)
- **Features**: Client Component sınırı burada — formlar, animasyonlar, consent yönetimi
- **Organisms**: Server Component — veri çeken, sayfa bölümlerini oluşturan büyük birimler (Hero, Navbar, Footer)

Client boundary kuralı: Bir bileşende `useState`, `useEffect`, `onClick` veya `localStorage` gerekiyorsa → Features katmanına gider ve `use client` alır. Gerekmiyorsa Server Component kalır.

## Sonuç
Net sorumluluk ayrımı sağlandı. İstemciye gönderilen JavaScript minimumda. İş mantığı framework'ten bağımsız saf fonksiyonlarda.
