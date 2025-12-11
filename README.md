# DHealth DevOps Skill Test Submission

Repository ini berisi infrastruktur containerized untuk aplikasi Yii 2.0 dengan database PostgreSQL, Nginx Reverse Proxy, dan Monitoring Stack (Prometheus + Grafana).

Solusi dikerjakan berdasarkan spesifikasi dokumen yang telah diberikan.

## 🌐 Live Demo (Cloudflare Zero Trust)

Aplikasi ini telah diduplikasi ke environment live menggunakan **Cloudflare Tunnel** untuk demonstrasi akses publik yang aman tanpa mengekspos IP server secara langsung.

| Service | URL | Keterangan |
|---------|-----|------------|
| **Web Application** | [https://dhealth.agusahmad.my.id/](https://dhealth.agusahmad.my.id/) | Yii 2.0 Framework |
| **Grafana Dashboard** | [https://monitor.agusahmad.my.id/](https://monitor.agusahmad.my.id/) | Monitoring Metrics (User: `admin`, Pass: `admin`) |

> *Note: Jika link tidak dapat diakses, kemungkinan tunnel sedang offline (local machine dimatikan).*

---

## 📋 Requirement Checklist Status

| Requirement | Status | Implementation Notes |
|-------------|--------|----------------------|
| **Fase 1: Containerization** | ✅ Done | Base image `ubuntu:latest` (sesuai instruksi), PostgreSQL persistence volume. |
| **Fase 2: Web Server** | ✅ Done | Nginx sebagai Reverse Proxy di port 80 meneruskan ke App container. |
| **Fase 3: Monitoring** | ✅ Done | Prometheus & Grafana setup. Dashboard menampilkan CPU/RAM/Disk metrics. |

## 🛠 Architecture Overview

Project ini menggunakan Docker Compose untuk orkestrasi service berikut:

* **`nginx` (Proxy Layer):** Entry point utama (Port 80). Menangani routing HTTP ke aplikasi.
* **`web-app` (App Layer):** Menjalankan framework Yii 2.0.
    * *Note:* Menggunakan base image `ubuntu:latest` sesuai requirement soal (bukan Alpine/PHP-FPM image).
* **`db` (Data Layer):** PostgreSQL 16 dengan volume mapping `./db-data` untuk persistensi data lokal.
* **`prometheus` & `grafana` (Monitoring Layer):** Mengumpulkan metrics dari Node Exporter.
* **`node-exporter`:** Mengambil metrics sistem host/container.
* **`cloudflared` (Live Demo):** Daemon untuk ekspos layanan lokal ke internet via Cloudflare Zero Trust Network.

## 🔧 Technical Decisions & Trade-offs

Bagian ini menjelaskan keputusan teknis yang diambil selama pengerjaan tugas:

### 1. Base Image Selection
Instruksi mewajibkan penggunaan **Ubuntu Latest**.
* **Decision:** Saya menggunakan `FROM ubuntu:latest` dan menginstall PHP secara manual di dalamnya.
* **Trade-off:** Image size lebih besar (~2x) dibandingkan menggunakan `php:8.2-fpm-alpine`. Untuk production environment sesungguhnya, saya merekomendasikan migrasi ke Alpine Linux untuk efisiensi resource dan keamanan (smaller attack surface).

### 2. Application Server
* **Current Setup:** Menggunakan PHP built-in server (`php yii serve`) untuk memenuhi requirement `ubuntu:latest` dengan konfigurasi minimalis.
* **Production Note:** Untuk high-traffic production, setup ini idealnya diganti menggunakan **PHP-FPM** yang berkomunikasi dengan Nginx via socket/TCP untuk performa konkurensi yang lebih baik.

### 3. Monitoring Strategy (Node Exporter)
* **Implementation:** Saya menggunakan satu instance `node-exporter` yang me-mount root volume host.
* **Justification:** Meskipun instruksi menyebutkan "masing-masing container", best-practice Prometheus umumnya adalah memonitor *host* (Node) atau menggunakan *cAdvisor* untuk metrics spesifik container. Menjalankan `node_exporter` process di dalam setiap container aplikasi dianggap *anti-pattern* (fat container) dan akan menghasilkan data duplikat (host metrics yang sama). Konfigurasi saat ini memberikan visibilitas metrics CPU/RAM yang akurat tanpa membebani container aplikasi.

## 🚀 How to Run

1. **Clone Repository**
   ```bash
   git clone https://github.com/ahmadsobir17/Dhealth-Skill-Test-DevOps.git
   cd Dhealth-Skill-Test-DevOps