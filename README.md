# VidalCasino 2.0 - Estadisticas Service

Este repositorio contiene el microservicio **estadisticas-service** del proyecto VidalCasino 2.0, desarrollado para la evaluación EP3 de Introducción a Herramientas DevOps. Este servicio entrega estadísticas de usuario y métricas generales relacionadas con el uso del sistema.

## Descripción general

`estadisticas-service` permite consultar estadísticas individuales y globales del sistema VidalCasino. El servicio se ejecuta dentro del clúster Kubernetes en Amazon EKS y se mantiene como componente interno mediante un Service de tipo `ClusterIP`.

El frontend consume este microservicio mediante rutas `/api/estadisticas`, evitando exponer el servicio directamente a Internet.

## Arquitectura del sistema

El sistema VidalCasino está compuesto por:

- **casino-frontend:** interfaz web expuesta públicamente mediante LoadBalancer.
- **casino-backend:** backend principal interno.
- **bonos-service:** microservicio de bonos.
- **apuestas-service:** microservicio de apuestas deportivas.
- **estadisticas-service:** microservicio de estadísticas.
- **postgres:** base de datos interna.

## Tecnologías utilizadas

- Python
- FastAPI
- PostgreSQL
- Docker
- Kubernetes
- Amazon EKS
- Amazon ECR
- GitHub Actions
- AWS Academy Learner Lab

## Endpoints de salud

El microservicio cuenta con rutas para sondas Kubernetes:

```txt
/livez
/readyz
/livez: permite validar que el proceso del servicio está vivo.
/readyz: permite validar que el servicio está listo para recibir tráfico y puede operar correctamente.
Despliegue en Kubernetes

Los manifiestos Kubernetes se encuentran en:

k8s/

Archivos principales:

k8s/deployment.yaml
k8s/service.yaml

El servicio se despliega como Deployment y se expone internamente mediante un Service tipo ClusterIP en el puerto 8006.

CI/CD

El despliegue automático se define en:

.github/workflows/deploy.yml

El workflow se ejecuta con push a la rama deploy.

El pipeline realiza:

Descarga del código.
Configuración de credenciales AWS Academy.
Inicio de sesión en Amazon ECR.
Build de imagen Docker.
Push de imagen a ECR con tags latest, v1.0.1 y SHA del commit.
Conexión de kubectl al clúster EKS.
Actualización del Deployment.
Verificación del rollout.
Comandos de verificación
kubectl get deployment estadisticas-service
kubectl get svc estadisticas-service
kubectl get pods -l app=estadisticas-service -o wide
kubectl describe deployment estadisticas-service
Estado esperado
Deployment disponible.
Service interno tipo ClusterIP.
Pod en estado Running.
Imagen publicada en Amazon ECR.
Pipeline CI/CD exitoso.