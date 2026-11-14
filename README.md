[README.md](https://github.com/user-attachments/files/34863208/README.md)
# k8s-grafana-deployment

Práctica individual de Procesos de Software (UDLA): despliegue de **Grafana** en Kubernetes usando **Deployment** y **ReplicaSet**, y exposición con un **Service**.

## Contenido

- `k8s/deployment.yaml`: Deployment de Grafana (3 réplicas, imagen `grafana/grafana:10.4.2`).
- `k8s/service.yaml`: Service tipo NodePort que expone Grafana en el puerto 3000.
- `capturas/`: evidencia de cada paso.

## Requisitos

- Docker Desktop
- Minikube
- kubectl

## Cómo reproducirlo

```bash
# 1. Levantar el clúster
minikube start --driver=docker
kubectl get nodes

# 2. Crear el namespace y desplegar
kubectl create namespace grafana-lab
kubectl apply -f k8s/deployment.yaml
kubectl get deployments,replicasets,pods -n grafana-lab

# 3. Probar el ReplicaSet (autocuración y escalado)
kubectl delete pod <nombre-del-pod> -n grafana-lab
kubectl scale deployment grafana --replicas=5 -n grafana-lab
kubectl scale deployment grafana --replicas=2 -n grafana-lab
kubectl scale deployment grafana --replicas=3 -n grafana-lab

# 4. Probar el Deployment (rolling update y rollback)
kubectl set image deployment/grafana grafana=grafana/grafana:11.0.0 -n grafana-lab
kubectl rollout status deployment/grafana -n grafana-lab
kubectl rollout history deployment/grafana -n grafana-lab
kubectl rollout undo deployment/grafana -n grafana-lab

# 5. Exponer y abrir Grafana
kubectl apply -f k8s/service.yaml
kubectl get svc,endpoints -n grafana-lab
minikube service grafana -n grafana-lab
```

Usuario y contraseña por defecto de Grafana: `admin` / `admin`.

## Autor

Sebastián Vallejo
