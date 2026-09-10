# Semantic Design Token Mimarisi

**Açıklama:** Marka renklerini doğrudan CSS'te kullanmak yerine, iki katmanlı bir token sistemi kuruldu: Core Tokens (ham renk değerleri) → Semantic Tokens (anlamsal kullanım).

**Neden bu desen:** Renk kodları (`#011b25`) anlamsız — nerede kullanılacağı belli değil. Semantic token (`--color-text-primary`) ise kullanım amacını kodluyor. Marka renkleri değiştiğinde tek noktadan güncelleme yeterli.

**Güvenli kod snippet'ı:**

```css
@theme {
  /* Katman 1: Core Tokens — Ham renk değerleri */
  --color-petrol-900: #011b25;
  --color-petrol-700: #01232f;
  --color-sage-700: #4a5a4f;
  --color-sage-100: #e8ece9;
  --color-gold-500: #f4c430;
  --color-neutral-50: #f8f9f7;

  /* Katman 2: Semantic Tokens — Anlamsal kullanım */
  --color-background: var(--color-neutral-50);
  --color-surface: #ffffff;
  --color-text-primary: var(--color-petrol-900);
  --color-text-secondary: var(--color-sage-700);
  --color-primary: var(--color-petrol-700);
  --color-accent: var(--color-gold-300);
  --color-border: var(--color-neutral-200);

  /* Tipografi */
  --font-sans: 'Manrope', sans-serif;
  --font-serif: 'Playfair Display', serif;
}
```

**Kullanım:**

```html
<!-- ❌ Kötü: Ham renk kodu — ne anlama geldiği belirsiz -->
<h1 class="text-[#011b25]">Başlık</h1>

<!-- ✅ İyi: Semantic token — amacı kodda okunuyor -->
<h1 class="text-[var(--color-text-primary)]">Başlık</h1>
```

**Erişilebilirlik notu:** `prefers-reduced-motion` medya sorgusu ile tüm CSS animasyonları devre dışı bırakılabiliyor. Dark mode desteği için semantic token katmanı ikinci bir değer seti alabilir — core token'lar değişmez, sadece semantic mapping değişir.
