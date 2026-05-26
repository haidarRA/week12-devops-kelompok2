# week12-devops-kelompok2
Tugas Week 12 DevOps Kelompok 12

| Nama | NRP |
| -------- | -------- |
| Haidar Rafi Aqyla | 5027231029 | 
| Furqon Aryadana | 5027231024 |
| Abid Ubaidillah A. | 5027231089 |
| Muhammad Hildan Adiwena| 5027231077 |
| Muhammad Kenas Galeno Putra | 	5027231069 |
| Benjamin Khawarizmi Habibi | 5027231078 |
| Arsyad Rizantha Maulana Salim | 5027221049 |

## 📌 Pengenalan

Modul ini mengajarkan cara menjalankan aplikasi dalam Kubernetes cluster. Topik:
- Objek dasar Kubernetes (Pod, Deployment, Service, Namespace)
- Mengelola container di production
- Rolling update dan rollback
- Pengenalan microservices

## 🛠️ Tech Stack & Prerequisite

**Tools yang harus dipasang:**
- **kubectl** — command line untuk cluster Kubernetes
- **minikube** — Kubernetes mini di laptop (atau Docker Desktop dengan Kubernetes enabled)
- **Docker** — untuk build & push image (dari modul sebelumnya)

**Instalasi:**
```bash
# macOS
brew install minikube kubectl

# Windows
# Download dari: https://minikube.sigs.k8s.io/docs/start/
# Dan: https://kubernetes.io/docs/tasks/tools/

# Verifikasi
minikube version
kubectl version --client
```

## Bagian 1 — Why Kubernetes?
Masalah yang Diselesaikan
Saat aplikasi skala besar:

- Container crash → downtime manual
- Traffic naik → perlu scale up manual
- Deploy versi baru → semua user offline

Solusi Kubernetes

- ✅ Auto-restart — pod crash langsung dibuat ulang
- ✅ Auto-scaling — scale up/down dengan 1 perintah
- ✅ Zero-downtime deploy — rolling update
- ✅ Rollback cepat — kembali ke versi lama dalam detik

Cara Kerja
```
Control Plane (Otak)
    ↓
Worker Nodes (Server yang jalankan app)
    ↓
Pods (Container di dalam node)
```
Kubernetes memantau cluster dan membuat keputusan agar state sesuai dengan yang kamu definisikan di file YAML.

## Check Bagian 2

Jangan Lupa ->
```
minikube start --cpus=2 --memory=4096
```

Validate YAML → 
``` 
kubectl apply -f <file> --dry-run=client 
```
(preview tanpa apply)

Deploy → 
``` 
kubectl apply -f deployment-dev.yaml & kubectl apply -f service-dev.yaml 
```
Check Pod → 
```
kubectl get pods -n taskflow-dev 
```
(pastikan status Running)

Check Service → 
```
kubectl get svc -n taskflow-dev 
```
(pastikan punya IP & port)

Test akses → 
```
kubectl port-forward svc/taskflow-api 8080:80 -n taskflow-dev 
```
lalu akses http://localhost:8080

## Bagian 3
### Laporan Pengujian Insiden 1 — Self-Healing

### Analisis Masalah Lama (Insiden 1)
Pada arsitektur monolitik lama, aplikasi TaskFlow dijalankan secara manual di satu container server tunggal. Ketika container mengalami crash pada pukul 02.15 malam, sistem tidak memiliki kecerdasan untuk mendeteksi maupun memulihkan diri, sehingga mengakibatkan downtime fatal selama lebih dari 6 jam hingga tim operasional masuk kerja di pagi hari.

### Solusi Kubernetes: Self-Healing via ReplicaSet
Melalui objek **Deployment** yang dikonfigurasi dengan `replicas: 2` di namespace `taskflow-prod`, Kubernetes Control Plane secara konstan menjalankan siklus rekonsiliasi (*reconciliation loop*). Jika salah satu Pod terdeteksi mati atau dihapus, Kubernetes akan mendeteksi ketidaksesuaian jumlah *state* saat itu dengan manifes, lalu secara instan menjadwalkan pembuatan Pod baru untuk mempertahankan ketersediaan layanan.

### Hasil Pengujian Simulasi
<img width="772" height="108" alt="image" src="https://github.com/user-attachments/assets/27a37c5a-57e4-4cd3-a000-b47115fe5d69" />


### 1. Eksekusi Penghapusan Pod (Simulasi Container Crash)
```bash
kubectl delete pod taskflow-api-5c8ccc8c55-95rjv -n taskflow-prod
```
<img width="1026" height="137" alt="image" src="https://github.com/user-attachments/assets/6031468e-c505-4f73-9439-88b703c99507" />
<img width="797" height="307" alt="image" src="https://github.com/user-attachments/assets/f6942780-8b38-4ab5-a6bd-8d8a57250e6d" />

## **Dokumentasi**


**Validate YAML**
![alt text](<Screenshot/Screenshot 2026-05-26 165925.png>)

**Check Pod & Check Service**
![alt text](<Screenshot/Screenshot 2026-05-26 165708.png>)

**Test Akses**
![alt text](<Screenshot/Screenshot 2026-05-26 165755.png>)

**Deploy**
![alt text](<Screenshot/Screenshot 2026-05-26 165849.png>)



