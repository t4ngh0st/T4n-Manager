# T4n-Manager

> *Package manager ringan untuk Void-inspired workflows — dibuat dengan Zig.*

---

## Ringkasan singkat

T4n-Manager adalah package manager minimalis yang ditulis dengan **Zig**. Fokusnya pada:

- **Integrasi mulus dengan xbps & xbps-src** — gunakan alat yang sudah Anda kenal dengan perintah yang lebih ringan.
- **Kecepatan** — binary kecil, dependensi minimal, dan operasi I/O yang dioptimalkan.
- **Rollback** — snapshot atomik sehingga Anda bisa kembali ke keadaan sebelumnya dengan cepat.

Tujuan: jadi alat sehari-hari untuk devs yang ingin workflow cepat dan dapat diandalkan pada sistem berbasis XBPS.

---

## Fitur utama

### 1. Kemudahan penggunaan `xbps` dan `xbps-src`
- Wrapper intuitif yang menerjemahkan sub-perintah xbps/xbps-src ke antarmuka yang lebih ramah.
- Autocomplete untuk paket dan shortcuts (`t4n install`, `t4n build-src`).
- Mode *dry-run* dan output yang dioptimalkan agar mudah dibaca manusia.

**Contoh**

```bash
# Pasang paket (proxy ke xbps-install)
t4n install neovim

# Build paket dari xbps-src
t4n build --from-src neovim
```

### 2. Kecepatan
- Implementasi operasi inti di Zig: startup cepat dan overhead memori rendah.
- Paralelisasi build ringan (konfigurasi default ramah laptop) dan penggunaan caching pintar.
- Output ringkas untuk pipeline CI/CD.

**Design note:** binary tunggal, tanpa runtime besar — cocok untuk server dan mesin develop ringan.

### 3. Rollback
- Snapshots atomik setelah setiap operasi pemasangan/pembaruan.
- `t4n rollback <id|last>` mengembalikan sistem ke snapshot sebelumnya tanpa mengacak konfigurasi pengguna.
- Integrasi dengan *transaction log* untuk audit singkat perubahan paket.

**Contoh**

```bash
# Lihat daftar snapshot
t4n snapshot list

# Kembalikan ke snapshot terakhir
t4n rollback last
```

---

## Instalasi

*Contoh singkat (binaries / build dari source):*

```bash
# Jika tersedia binary (rekomendasi untuk pengguna biasa)
curl -sSL https://example.com/t4n-manager/latest | sudo sh

# Build dari source (butuh zig)
git clone https://github.com/gh0st4n/t4n-manager.git
cd t4n-manager
zig build -Drelease-safe
sudo cp zig-out/bin/t4n /usr/local/bin/
```

---

## Quickstart

1. Sinkronkan repositori paket

```bash
t4n update
```

2. Pasang paket

```bash
t4n install <paket>
```

3. Jika terjadi masalah, rollback

```bash
t4n rollback last
```

---

## Config singkat (`~/.config/t4n/config.toml`)

```toml
# Concurrency untuk build (default: 2)
concurrency = 2

# Lokasi xbps-src (jika custom)
xbps_src = "/usr/local/xbps-src"

# Simpan snapshot terakhir (true/false)
enable_snapshots = true

# Maks snapshot yang disimpan
max_snapshots = 10
```

---

## Filosofi desain
- Minimal: fungsionalitas inti yang jelas, tanpa fitur berlebih.
- Transparen: log yang sederhana dan dapat diaudit.
- Reproducible: build yang konsisten melalui xbps-src dengan cache yang deterministik.

---

## Lisensi
GPL-3.0 `LICENSE`.

---

*Jika ingin versi README dengan header bergaya (gambar/illustration), atau versi bahasa Inggris, bilang saja — saya sesuaikan.*
