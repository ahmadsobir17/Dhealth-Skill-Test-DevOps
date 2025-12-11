# 🏥 DHealth DevOps Skill Test Submission  

Repository ini berisi solusi lengkap untuk **Skill Test DevOps PT. Citraraya Nusatama (DHEALTH)**.  
Solusi ini mensimulasikan infrastruktur *Microservices Production-Ready* yang mencakup kontainerisasi aplikasi, manajemen database, reverse proxy, hingga sistem monitoring real-time.  

---

## 🚀 Live Demo (Zero Trust Deployment)

Aplikasi ini telah di-deploy ke Cloud VPS menggunakan **Cloudflare Tunnel (Zero Trust)** tanpa membuka port publik, menjamin keamanan akses.

| Service | URL Akses | Keterangan |
|----------|------------|------------|
| Web Application | [https://dhealth.agusahmad.my.id](https://dhealth.agusahmad.my.id) | Aplikasi Yii 2.0 via Nginx |
| Monitoring Dashboard | [https://monitor.agusahmad.my.id](https://monitor.agusahmad.my.id) | Grafana *(User: admin / Pass: admin)* |

---

## 🏗️ Arsitektur & Topologi

Sistem ini diorkestrasi menggunakan **Docker Compose** dan terbagi menjadi 3 Layer utama:

1. **Proxy Layer (Gateway)**  
   Menggunakan NGINX (Port 80) sebagai *Single Entry Point* untuk memanajemen trafik dan menyembunyikan IP internal container.

2. **Application Layer**  
   - Web App: Container berbasis Ubuntu + Yii 2.0  
   - Database: PostgreSQL 16 dengan konfigurasi *Data Persistence*

3. **Monitoring Layer (Observability)**  
   - Node Exporter  
   - Prometheus (scrape interval 5 detik)  
   - Grafana Dashboard

---

## ✅ Status Pengerjaan (Completed Phases)

| Fase | Deskripsi Teknis | Status |
|------|-------------------|--------|
| Fase 1 | Containers: Setup Custom Image Ubuntu + PHP & PostgreSQL Volume Mapping | ✅ DONE |
| Fase 2 | Web Server: Setup NGINX Reverse Proxy (Clean URL localhost) | ✅ DONE |
| Fase 3 | Monitoring: Setup Prometheus & Grafana Dashboard (CPU/RAM/Disk Usage) | ✅ DONE |

---

## 🛠️ Key Technical Decisions (Bedah Fitur)

### 1. Immutable Infrastructure (Web App)
Source code **Yii 2.0** ditanam langsung ke dalam Docker Image saat build untuk mencegah *code drift* di production.

### 2. Data Persistence (Database)
Menggunakan Volume Mapping `./db-data:/var/lib/postgresql/data` agar data tidak hilang saat container dihapus.

### 3. Monitoring 
Prometheus diset `scrape_interval: 5s` agar demo perubahan CPU/RAM terlihat instan.

---

## 💻 Cara Menjalankan (Localhost)

### Prasyarat
- Docker & Docker Compose terinstal  
- Port 80, 3000, 3001, 9090, 9100 tersedia  

### Langkah Instalasi

```bash
# 1. Clone Repository
git clone https://github.com/ahmadsobir17/Dhealth-Skill-Test-DevOps.git
cd dhealth-test

# 2. Jalankan Docker Compose
docker compose up -d --build
