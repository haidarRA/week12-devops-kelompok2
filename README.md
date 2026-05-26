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



