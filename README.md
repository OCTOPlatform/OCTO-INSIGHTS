# OCTO Insights

> **One Platform. Many Possibilities.**  
> **Product Principle:** *Proof Over Hype*  
> **Product Tagline:** *Understand Your Customers. Make Smarter Decisions.*

---

## 1. Ikhtisar Produk

**OCTO Insights** adalah platform *Customer Intelligence*, analitik ulasan, dan analisis sentimen berbasis AI yang dirancang untuk bisnis Indonesia. Menggabungkan komputasi analitik deterministik dengan pemrosesan bahasa alami (NLP) berbasis aspek untuk Bahasa Indonesia.

Nilai utama produk mengikuti alur kerja terpercaya:
$$\text{Collect} \longrightarrow \text{Validate} \longrightarrow \text{Analyze} \longrightarrow \text{Understand} \longrightarrow \text{Verify} \longrightarrow \text{Act} \longrightarrow \text{Measure}$$

### Prinsip Inti "Proof Over Hype"
1. **Tidak Ada Halusinasi Angka:** Semua KPI, rata-rata bintang, persentase sentimen, dan tren dihitung secara deterministik (100% kode / SQL), bukan perkiraan LLM.
2. **Keterlacakan Bukti Sumber:** Setiap klaim temuan AI melampirkan ID ulasan pendukung dan kutipan teks asli.
3. **Preservasi Teks Asli:** Teks asli ulasan konsumen tidak pernah dimodifikasi atau ditimpa oleh ringkasan AI.
4. **Sentimen Berbasis Aspek (ABSA):** Mendeteksi multi-aspek (Kualitas Produk, Pelayanan, Waktu Tunggu, Harga, Kebersihan, Kemudahan, dll.) secara mandiri dalam satu ulasan yang sama.
5. **Human Review & Audit Trail:** Mendukung koreksi manual sentimen oleh staf yang tercatat rapi di log audit.
6. **Integrasi Ekosistem OCTO:** Terhubung melalui kontrak API eksplisit ke perangkat cerdas **OCTO-TAP** (NFC/QR) dan **OCTO-CRM** (penugasan follow-up tugas).

---

## 2. Arsitektur Teknis

```
Delivery (Gin HTTP Router, Middlewares, Handlers, DTOs)
   │
   ▼
Application (Auth, Workspace, Feedback, Ingestion, Analysis, Intelligence, Reports, Integrations)
   │
   ▼
Domain (Entities, Value Objects, Invariants, Repository Ports) ◄─── Infrastructure (PostgreSQL, Memory, Indonesian NLP Engine)
```

- **Backend:** Go 1.26 (Clean Architecture + DDD, TDD, Gin Framework)
- **Database:** PostgreSQL 15+ / Thread-Safe In-Memory Store (Zero-dependency development)
- **Frontend:** Next.js 16 + React 19 + TypeScript + Tailwind CSS v4
- **Palet Warna Ekosistem OCTO:**
  - Deep Navy: `#142740`
  - Deep Navy Light: `#1e395e`
  - Cyan: `#00AFC4`
  - Soft Cyan: `#E8F8FA`
  - Warm White: `#FAFBFC`
  - Soft Gray: `#F2F5F7`

---

## 3. Fitur Utama

| Modul | Deskripsi |
|---|---|
| **Overview Dashboard** | KPI Total Ulasan, Rata-rata Rating Bintang, Net Sentiment Score, Bar Distribusi Sentimen, Top Keunggulan & Keluhan Berulang. |
| **Feedback Explorer** | Pencarian teks, filter sentimen, filter cabang, detail ulasan per aspek, dan modal koreksi manual (Human Review). |
| **Topics & Aspects** | Taksonomi 8 aspek default bisnis (Kualitas, Pelayanan, Waktu Tunggu, Harga, Kebersihan, Kemudahan, Pengiriman, Komplain). |
| **Evidence Insights** | Temuan AI berpola teramati atau hipotesis, ukuran sampel, peringatan ketidakpastian/batasan, tombol verifikasi kutipan asli, dan 1-click dispatch ke OCTO-CRM. |
| **Ask OCTO (Conversational Analytics)** | Tanya jawab dalam Bahasa Indonesia santai (contoh: "Apa keluhan paling sering?") yang dijawab berdasarkan data terukur dengan kutipan review pendukung. |
| **Trends & Alerts** | Deteksi anomali lonjakan keluhan terhadap baseline dengan syarat sampel minimum untuk menekan noise. |
| **Laporan & Ekspor** | Pembuatan laporan eksekutif dengan rincian metodologi, snapshot KPI, temuan utama, dan tombol cetak/PDF siap pakai. |
| **Data Ingestion** | Impor file CSV fleksibel, input ulasan manual, dan tombol 1-Click untuk memuat 25+ ulasan sintetis Indonesia. |
| **Integrasi Ekosistem** | Sinkronisasi pemindaian NFC/QR dari OCTO-TAP dan pengiriman tugas follow-up ke OCTO-CRM. |
| **Pengaturan & Kuota** | Pengukur konsumsi kuota analitik per bulan dan matriks hak akses Role (Owner, Admin, Analyst, Viewer). |

---

## 4. Memulai (Quickstart)

### Prasyarat
- Go 1.22+ (atau Go 1.26)
- Node.js 20+ & npm

### Menjalankan Backend
```bash
cd backend
go run ./cmd/api
```
Backend akan aktif di `http://localhost:8082` dan secara otomatis memuat:
- Akun demo: `demo@octo-insights.id` / `password123`
- Workspace default: **Warung Kopi Nusantara**
- 25+ sample review realistis berbahasa Indonesia beserta aspek dan insight terverifikasi.

### Menjalankan Frontend
```bash
cd frontend
npm run dev
```
Buka browser di `http://localhost:3003`.

### Menjalankan Seluruh Tes
```bash
make test
```
Menjalankan domain unit tests, validasi evaluasi NLP Bahasa Indonesia (slang, negasi, multi-aspek), application service tests, dan End-to-End HTTP API tests.

---

## 5. Ringkasan Endpoint API (`/api/v1`)

### Otentikasi & Workspace
- `POST /api/v1/auth/register` — Pendaftaran akun & workspace awal
- `POST /api/v1/auth/login` — Masuk pengguna & penerbitan token JWT
- `GET /api/v1/auth/me` — Info profil pengguna terotentikasi
- `GET /api/v1/workspaces` — Daftar workspace milik pengguna
- `POST /api/v1/workspaces` — Membuat workspace baru

### Feedback & Ingestion
- `GET /api/v1/workspaces/:id/feedback` — Daftar & filter ulasan
- `POST /api/v1/workspaces/:id/feedback` — Input review manual
- `POST /api/v1/workspaces/:id/feedback/:feedbackId/corrections` — Koreksi manual sentimen
- `POST /api/v1/workspaces/:id/imports/csv` — Unggah file ulasan CSV
- `POST /api/v1/workspaces/:id/imports/synthetic` — 1-Click muat sample data

### Analitik & Insights
- `GET /api/v1/workspaces/:id/analytics/overview` — Agregasi deterministik (KPIs & sentimen)
- `GET /api/v1/workspaces/:id/insights` — Daftar insight terverifikasi bukti
- `POST /api/v1/workspaces/:id/insights/generate` — Generate insight baru
- `POST /api/v1/workspaces/:id/insights/query` — Conversational analytics Q&A
- `PATCH /api/v1/workspaces/:id/insights/:insightId/review` — Review status insight (useful / dismissed)
- `POST /api/v1/workspaces/:id/insights/:insightId/actions` — Tindak lanjut & dispatch ke CRM

### Ekosistem & Laporan
- `POST /api/v1/insights/events` — Ingestion stream event tap dari OCTO-TAP
- `GET /api/v1/workspaces/:id/integrations/tap/metrics` — Metrik konversi tap ke ulasan
- `GET /api/v1/workspaces/:id/reports` — Riwayat laporan eksekutif
- `POST /api/v1/workspaces/:id/reports` — Generate laporan eksekutif baru
- `GET /api/v1/workspaces/:id/usage` — Pemantauan kuota penggunaan bulanan

---

## 6. Lisensi & Tata Kelola
Sesuai spesifikasi `OCTO_Insights_Ultimate_PRD_v2.md`, produk ini mematuhi standar privasi data dan perlindungan informasi konsumen Indonesia. Hak Cipta © 2026 OCTO Platform.
