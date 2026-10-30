# Daftar Perbaikan E-LOCK

Tampilan dan isi web tidak diubah. Yang diperbaiki hanya error dan perilaku yang tidak sesuai.

## Bug fungsional
1. **Filter kategori tidak berfungsi.** Pill kategori (Precision Measurement, dll.) hanya meredupkan bay kalau kolom search juga terisi. Sekarang langsung berfungsi. (`Locker3DGrid.tsx`)
2. **Kartu filter status (Blue/Green/Red) tidak berpengaruh.** Label "Filtered: BLUE" berubah, tapi locker grid tidak difilter. Sekarang bay yang tidak cocok diredupkan. (`App.tsx`, `Locker3DGrid.tsx`)
3. **Status "Overdue" bergantung pada jam asli pengunjung.** Data demo memakai `Date.now()`, sedangkan jam simulasi tetap 10:15. Kalau web dibuka sore hari, alat Hex Key tidak overdue dan tombol "1 Overdue" tidak muncul. Sekarang data demo dan jam simulasi memakai satu titik awal yang sama. (`mockData.ts`, `App.tsx`)
4. **Popup Overdue menghitung keterlambatan dengan jam asli**, bukan jam simulasi, sehingga "+Xm Late" salah. (`OverdueModal.tsx`)
5. **Teks "1 Overdue" di header di-hardcode.** Sekarang mengikuti jumlah alat yang benar-benar overdue. (`Header.tsx`)
6. **Bay yang terkunci maintenance bisa dibuka pintunya.** Sekarang pintu tetap tertutup dan yang muncul adalah detail "Safety Lockout". (`Locker3DGrid.tsx`)
7. **ID log bisa kembar** (memakai 4 digit terakhir timestamp), yang memicu warning key React. Sekarang ID selalu unik. (`App.tsx`)
8. **Timer toast dan simulasi load cell saling bertabrakan** kalau diklik cepat. Sekarang timer lama dibatalkan. (`App.tsx`, `LoadCellModal.tsx`)

## Angka dan teks yang salah
9. **"Top 5 tools account for 68%"**: dari data sebenarnya hasilnya 54%. Sekarang dihitung otomatis dari data. (`UsageErgonomicsChart.tsx`)
10. **Annual ROI selisih karena pembulatan bertingkat** ($77,640 vs rumus di modal Logic = $77,648). Sekarang sesuai rumus. (`lockerEngine.ts`)
11. Filter "Needs Re-tier" menampilkan area kosong. Sekarang ada pesan bahwa semua alat sudah ergonomis.

## Konfigurasi
12. **`npm run lint` gagal** karena `tsconfig.json` hanya memuat tipe `vite/client` (tidak ada `node`). Sekarang `["node", "vite/client"]`.
13. **Animasi `animate-fadeIn` / `animate-slideDown` dan `scrollbar-none` dipakai tapi tidak pernah didefinisikan.** Sekarang didefinisikan di `index.css`.
14. **Script `npm test` ditambahkan** (file test sudah ada, tapi tidak bisa dijalankan). Test ROI ditambah, hasil 7/7 lulus.
15. **Dependensi yang tidak dipakai dihapus** (`express`, `dotenv`, `@google/genai`, `@types/express`). Web ini tidak memanggil Gemini, jadi **tidak perlu API key**.
16. Folder `.aistudio/` (3,5 MB gambar sementara AI Studio) tidak dibawa karena tidak dipakai web.

## Catatan
- `src/data/toolImages.ts` banyak berisi kode yang tidak terpakai. Dibiarkan supaya isi tidak berubah.
- Tombol "Open All Bays", "Apple Crop", dan "Upload" ada di kode tapi tidak ditampilkan karena handler-nya tidak pernah dipasang di `App.tsx`. Dibiarkan supaya tampilan tidak berubah.

## Cara deploy (Vercel)
1. Upload folder ini ke repo GitHub baru.
2. Buka vercel.com, login dengan GitHub, lalu **Add New, Project**, pilih repo-nya.
3. Biarkan pengaturan default (Vite terdeteksi otomatis), lalu klik **Deploy**.
