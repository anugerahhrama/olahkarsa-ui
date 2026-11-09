# Keputusan Fase 0 — Penyelarasan Brand Ekosistem Olahkarsa

> Status: **Disepakati 2026-11-10** (kecuali yang ditandai _Terbuka_).
> Cakupan: `ok-group-app` (Group), `compro-v4` (Consulting), `olahkarsa-blog`, `olahkarsa-event-v2`, `insight-report`.
> Implementasi token: [`src/tokens.css`](../src/tokens.css).

## Konteks singkat

Kelima app memakai logo dengan mark yang sama: biru `#009DFB` bergradasi ke `#004AFF`, plus titik kuning `#FECC00`. Masalahnya, UI tiap app memakai warna primary yang berbeda-beda:

| App | Primary yang dipakai sekarang |
|---|---|
| Group | `#009DFB` (token, terkonfirmasi Figma) |
| Consulting | `#0047CC`, `#005CFF`, `#1A5B8E` (ditulis langsung di ±159 tempat) |
| Blog / Event / Report | `#008BDE`, `#0262A3`, `#0073B8` (ditulis langsung) |

---

## D-01 · Arsitektur brand: _masterbrand_, satu primary

Semua entitas memakai **satu primary `#009DFB`**. Pembeda antar entitas hanya ada di _descriptor_ logo ("Group", "Consulting", dst.), bukan di warna UI.

**Alasan:** mark logo sudah identik di semua entitas. Kalau tiap app punya warna UI sendiri, kesan "satu grup" justru melemah.

## D-02 · Palet inti

| Token Tailwind | Hex | Peran |
|---|---|---|
| `brand` / `primary` | `#009DFB` | Warna utama: logo, ikon, elemen brand, tombol utama (lihat D-09) |
| `brand-hover` | `#0069A7` | Hover tombol/link. Juga dipakai untuk teks link di atas putih |
| `brand-pressed` | `#003454` | Pressed, background section gelap (CTA, footer gelap) |
| `brand-border` | `#CCEBFE` | Border card/badge biru muda |
| `brand-surface` | `#F5FBFF` | Background section terang kebiruan |
| `brand-deep` | `#004AFF` | Ujung gradasi logo. Hanya untuk gradasi dan aksen besar, **bukan** primary kedua |
| `brand-accent` | `#FECC00` | Kuning titik logo (aturan di D-03) |

Gradasi resmi: `bg-brand-gradient` = `#009DFB → #004AFF` (135°).

### Pemetaan warna lama → token baru

| Warna lama | App | Ganti dengan |
|---|---|---|
| `#0047CC`, `#005CFF`, `#008BDE`, `#0072B8`, `blue-600` | Consulting, Blog, Event, Report | `primary` |
| `#1A5B8E`, `#0262A3`, `#0073B8` | Consulting, Blog, Event, Report | `brand-hover` |
| `#021437`, `#01416B` | Consulting, Event, Report | `brand-pressed` |
| `#EEF8FF`, `#E8F4FD`, `#E8F3FF`, `#F8FCFF` | semua | `brand-surface` |
| `#BEDEFD` | Group | `brand-border` |
| `#FF2D20` (sisa warna starter Laravel) | Consulting, Blog, Event, Report | hapus, atau `destructive` kalau memang untuk error |

## D-03 · Kuning `#FECC00`: aksen terbatas

**Boleh dipakai untuk:**
- badge "Baru"/"Live"
- titik atau garis dekoratif
- garis bawah heading
- highlight angka atau metrik

**Tidak boleh dipakai untuk:**
- tombol
- teks di atas putih (kontras 1.4:1)
- background area luas

Kalau ada teks di atas kuning, pakai `ink-100` (`#101010`), yang kontrasnya 12.5:1.

## D-04 · Tipografi

Satu font untuk semua app: **Inter Tight**, weight 400 / 500 / 600 / 700. Dimuat dari Bunny Fonts lewat `@olahkarsa/ui/fonts.css`, atau satu `<link>` yang sama di tiap app.

Font berikut dihapus dari app yang masih memuatnya: Poppins, Figtree, dan Instrument Sans (fallback). Weight 200, 300, 800, dan 900 juga tidak dimuat lagi.

## D-05 · Skala neutral: namespace `ink-*`

Skala neutral dari Figma (10–100) **tidak** dipasang di atas `neutral-*` milik Tailwind. Alasannya, nama kelas yang sama sudah dipakai dengan arti berbeda:

| Kelas | Group (Figma) | Consulting / Blog (Tailwind) |
|---|---|---|
| `neutral-100` | `#101010` (hampir hitam, 50×) | `#F5F5F5` (hampir putih, 21×) |
| `neutral-50` | `#C2C2C2` | `#FAFAFA` |

Karena itu paket bersama memakai nama **`ink-10` … `ink-100`**. Group mempertahankan alias `neutral-10…100` lokalnya selama masa migrasi, lalu pindah ke `ink-*`.

| `ink-10` | `ink-20` | `ink-30` | `ink-40` | `ink-50` | `ink-60` | `ink-70` | `ink-80` | `ink-90` | `ink-100` |
|---|---|---|---|---|---|---|---|---|---|
| `#FFFFFF` | `#F5F5F5` | `#EDEDED` | `#D6D6D6` | `#C2C2C2` | `#878787` | `#606060` | `#383838` | `#403A3A` | `#101010` |

## D-06 · Radius

- Radius dasar `--radius: 0.5rem` (8px), mengikuti Group. Varian `rounded-sm/md/lg` diturunkan dari nilai ini.
- Tombol `rounded-btn` = 12px.
- Consulting saat ini memakai 0.75rem dan akan ikut disesuaikan.

## D-07 · Distribusi: repo paket `olahkarsa-ui`

Token dan (nanti) komponen bersama tinggal di repo **`anugerahhrama/olahkarsa-ui`** (public; dipindah ke akun kantor `olahkarsa` bila perlu). Tiap app memasangnya sebagai git dependency yang dikunci ke tag versi:

```jsonc
// package.json tiap app
"@olahkarsa/ui": "github:anugerahhrama/olahkarsa-ui#v0.1.0"
```

Rencana versi:

- **v0.1** — token CSS saja (`tokens.css`, `fonts.css`).
- **v0.2** — komponen Vue _shell_: `EcosystemBar`, `SiteFooter`, `OlahkarsaLogo`, `ContentCard`.

Event dan Report masih Blade/Livewire, jadi baru bisa memakai komponen Vue setelah ditulis ulang di Fase 3. Sebelum itu, keduanya cukup memakai `tokens.css`, yang bisa langsung dipakai setelah upgrade ke Tailwind 4.

## D-08 · Domain: _Terbuka_, semua lewat env

Domain Group dan Consulting belum diputuskan. Untuk sementara, **tidak boleh ada URL antar-app yang ditulis langsung di kode.** Semua app memakai kunci env yang sama:

```dotenv
OLAHKARSA_GROUP_URL=
OLAHKARSA_CONSULTING_URL=
OLAHKARSA_BLOG_URL=https://blog.olahkarsa.com
OLAHKARSA_EVENT_URL=https://event.olahkarsa.com
OLAHKARSA_REPORT_URL=https://report.olahkarsa.com
```

Kunci-kunci ini dibaca lewat `config/olahkarsa.php` (dibuat di Fase 1). Kunci lama yang nanti diganti:
- Consulting: `LINK_BLOG`, `LINK_EVENT`, `LINK_REPORT`. Nilai default-nya saat ini salah, tanpa `.com`.
- Group: `INSIGHT_*_SITE_URL`.
- Event: `env('API_URL')` yang dipanggil langsung di Blade.

## D-09 · Kontras warna: _Terbuka_, perlu keputusan desainer

Hasil cek kontras WCAG 2.1 AA (minimal 4.5:1 untuk teks normal, 3:1 untuk teks besar):

| Kombinasi | Rasio | Status |
|---|---|---|
| Teks putih di atas `#009DFB` (tombol primary sekarang) | 2.91 | ❌ gagal, bahkan untuk teks besar |
| Teks `#009DFB` di atas putih (link) | 2.91 | ❌ |
| Teks putih di atas `#0069A7` (`brand-hover`) | 5.87 | ✅ |
| Teks `#101010` di atas `#009DFB` | 6.54 | ✅ |
| Body text `#878787` (`ink-60`) di atas putih | 3.59 | ❌ untuk paragraf |
| Body text `#606060` (`ink-70`) di atas putih | 6.29 | ✅ |

Opsi untuk tombol dan link:

1. **Tetap `#009DFB`** dan terima kontras di bawah standar. Ini tampilan Group saat ini.
2. **Tombol dan link memakai `#0069A7`**, sementara `#009DFB` hanya untuk elemen brand dan dekorasi. Ini lulus AA, tapi tampilan jadi lebih gelap.
3. **Tambah satu langkah baru `#0077C2`** (4.75:1) sebagai warna aksi. Lulus AA dan paling dekat dengan brand, tapi warna ini belum ada di Figma.

Sampai diputuskan, `tokens.css` sudah menyiapkan `--primary-strong` (`#0069A7`) yang bisa dipakai untuk teks link. Body text direkomendasikan memakai `ink-70`, bukan `ink-60`.

## Isu lain yang dicatat

- Halaman "Colors" di Figma masih berisi warna template (Viridian `#099699`, footer "Flow Forge Studio"). Tim desain perlu membersihkannya supaya Figma dan token tidak saling bertentangan.
- Favicon Consulting dan Blog masih logo merah Laravel (`#FF2D20`). Ganti dengan favicon Olahkarsa dari Group.

## Langkah berikutnya (Fase 1)

1. Jadikan `olahkarsa-ui` repo di GitHub org `olahkarsa`, lalu buat tag `v0.1.0`.
2. Pilot di **olahkarsa-blog**:
   - pasang paket, ganti blok token di `app.css` dengan `@import '@olahkarsa/ui/tokens.css'`
   - ganti warna yang ditulis langsung sesuai tabel D-02
   - pisahkan navbar & footer dari `Welcome.vue`
   - buat `config/olahkarsa.php`
3. Evaluasi hasilnya, lalu lanjut ke komponen shell (v0.2).
