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


## Check Bagian 2

Jangan Lupa ->
```
minikube start
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



