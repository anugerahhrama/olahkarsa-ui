# @olahkarsa/ui

Design tokens dan (mulai v0.2) komponen bersama untuk semua app Olahkarsa: Group, Consulting, Blog, Event, dan Report.

Keputusan brand dan alasannya ada di [docs/DECISIONS.md](docs/DECISIONS.md).

## Pasang

```jsonc
// package.json app
"dependencies": {
    "@olahkarsa/ui": "github:olahkarsa/olahkarsa-ui#v0.1.0"
}
```

```css
/* resources/css/app.css */
@import '@olahkarsa/ui/fonts.css';
@import 'tailwindcss';
@import 'tw-animate-css';
@import '@olahkarsa/ui/tokens.css';

/* blok @theme, :root, dan .dark lokal dihapus. Isinya sudah ada di tokens.css */
```

Butuh Tailwind CSS v4.

## Isi token

| Kelompok | Contoh utility |
|---|---|
| Brand | `bg-brand`, `text-brand-hover`, `bg-brand-surface`, `border-brand-border`, `bg-brand-gradient` |
| Aksen kuning (hemat) | `bg-brand-accent`, `decoration-brand-accent` |
| Neutral Figma | `text-ink-100`, `text-ink-70`, `border-ink-30` |
| Semantik shadcn | `bg-primary`, `text-primary-strong`, `bg-muted`, `text-muted-foreground`, `border-border` |
| Status | `bg-success-surface text-success-pressed`, `bg-error`, `bg-warning-surface`, `bg-info` |
| Radius | `rounded-lg` (8px), `rounded-btn` (12px) |

## Rilis

1. Ubah token dan catat alasannya di `docs/DECISIONS.md`.
2. Naikkan `version` di `package.json`, lalu buat tag `vX.Y.Z`.
3. Di tiap app, ganti ref tag di `package.json`, jalankan `npm install` dan `npm run build`.
