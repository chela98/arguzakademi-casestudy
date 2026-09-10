# Schema.org JSON-LD Builder Deseni

**Açıklama:** Google Rich Results için Schema.org structured data üretimi. Her builder saf bir fonksiyon — CMS verisini alır, JSON-LD objesi döner.

**Neden bu desen:** Framework'e bağımlı olmayan saf fonksiyonlar. DOM yok, fetch yok, side-effect yok. Girdi ver → çıktı al. Bu sayede birim test trivial.

**Güvenli kod snippet'ı** (gerçek projeden esinlenilmiş ama genelleştirilmiş):

```typescript
// Saf builder fonksiyonu — yan etkisi yok, birim test edilebilir
type EventInput = {
  title: string;
  description?: string | null;
  slug: string;
  startDate: string;
  format?: 'online' | 'yuz-yuze' | 'hibrit' | null;
};

export function buildEventSchema(input: EventInput) {
  const attendanceMode =
    input.format === 'online'
      ? 'https://schema.org/OnlineEventAttendanceMode'
      : input.format === 'hibrit'
      ? 'https://schema.org/MixedEventAttendanceMode'
      : 'https://schema.org/OfflineEventAttendanceMode';

  return {
    '@context': 'https://schema.org',
    '@type': 'Event',
    name: input.title,
    description: input.description || undefined,
    startDate: input.startDate,
    eventAttendanceMode: attendanceMode,
    eventStatus: 'https://schema.org/EventScheduled',
  };
}
```

**Test örneği:**

```typescript
import { describe, it, expect } from 'vitest';
import { buildEventSchema } from './structuredData';

describe('buildEventSchema', () => {
  it('online etkinlik için doğru attendance mode üretmeli', () => {
    const result = buildEventSchema({
      title: 'Test Etkinliği',
      slug: 'test',
      startDate: '2026-10-01',
      format: 'online',
    });

    expect(result.eventAttendanceMode).toBe(
      'https://schema.org/OnlineEventAttendanceMode'
    );
  });

  it('açıklama yoksa undefined olmalı (JSON.stringify otomatik atar)', () => {
    const result = buildEventSchema({
      title: 'Test',
      slug: 'test',
      startDate: '2026-10-01',
    });

    expect(result.description).toBeUndefined();
  });
});
```

**Öğrenilen ders:** `null` yerine `undefined` döndürmek önemli — `JSON.stringify` `undefined` değerli alanları otomatik atlıyor, `null` ise çıktıda kalıyor ve Schema.org validator'da hata veriyor.
