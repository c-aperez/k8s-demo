# 🚀 Despliegue de `Pagina Web Demo` en Kubernetes

Este proyecto contiene los archivos necesarios para desplegar una landing page básica en un clúster de Kubernetes. A continuación se explican los conceptos fundamentales involucrados, acompañados de enlaces útiles a la documentación oficial. 🧠📘

---

## 🌐 ¿Qué es Kubernetes?

[Kubernetes](https://kubernetes.io/) es una plataforma de orquestación de contenedores que automatiza el despliegue, escalado y administración de aplicaciones en contenedores (como Docker).

Organiza las aplicaciones en **pods**, que se ejecutan en un clúster distribuido de **nodos**.

📚 [Ver documentación oficial →](https://kubernetes.io/docs/concepts/)

---

## 🗂️ Namespace: `dcsarapp`

Un [**Namespace**](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) en Kubernetes permite aislar recursos dentro de un mismo clúster.

🔹 En este caso usamos:

```yaml
namespace: dcsarapp
```

✅ Esto permite organizar los recursos de esta aplicación de forma separada de otros entornos o servicios dentro del mismo clúster.

---

## 📦 Deployment: `dcsar-basic-landing-page`

Un [**Deployment**](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) controla cómo se crean y gestionan los pods de tu aplicación. Es ideal para mantener alta disponibilidad y facilitar actualizaciones sin tiempo de inactividad.

🔧 Este Deployment:

- 🔁 Mantiene **1 réplicas** activas de la app.
- 🐳 Usa la imagen `dcsarapp:v1`.
- 🔄 Aplica actualizaciones con estrategia `RollingUpdate`.
- ⚙️ Define **límites de CPU y memoria** para estabilidad.

```text
[Deployment]
   └── [1 Pods]
         └── [Contenedor: dcsarapp:v1]
```

📚 [Más sobre Deployments →](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

---

## 🌐 Service: `svc-dcsar-basic-landing-page`

Un [**Service**](https://kubernetes.io/docs/concepts/services-networking/service/) permite exponer tus pods para ser accedidos desde otras aplicaciones o desde fuera del clúster.

💡 Aquí usamos un `NodePort`:

- 🌍 Expone el puerto **30222** en todos los nodos del clúster.
- 🎯 Redirige el tráfico al **puerto 80** del contenedor.
- 🔗 Utiliza un **selector de etiquetas** para identificar los pods gestionados por el Deployment.

```text
[Cliente externo]
     ↓ (HTTP 30222)
[NodePort Service]
     ↓
[Pod: Puerto 80]
```

📚 [Más sobre NodePort →](https://kubernetes.io/docs/concepts/services-networking/service/#nodeport)

---

## 📈 HorizontalPodAutoscaler (HPA)

El recurso [**HorizontalPodAutoscaler**](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) permite escalar automáticamente el número de pods de una aplicación en función de métricas como uso de CPU o memoria.

🔧 En este caso:

- 📊 Se escala según el **uso de memoria**.
- 📉 El número mínimo de réplicas es `1`.
- 📈 El número máximo de réplicas es `3`.
- 🧠 Escala cuando el uso promedio de memoria supera el **70%**.

```yaml
metrics:
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 70
```

Esto permite que tu aplicación se adapte automáticamente a la demanda de tráfico sin intervención manual. 🧩

📚 [Más sobre HPA →](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)

---

## 🧭 Flujo de Funcionamiento

```text
┌────────────────────┐
│ Usuario Externo    │
└────────┬───────────┘
         ↓ Puerto 30222
┌────────┴───────────┐
│ Nodo de Kubernetes │
└────────┬───────────┘
         ↓
┌────────┴──────────────┐
│ Service: NodePort     │
└────────┬──────────────┘
         ↓
┌────────┴───────────-───┐
│ Pod con contenedor     │
│ dcsarapp:v5 (Puerto 80)│
└────────┬─────────────-─┘
         ↓
       (HPA)
  Evalúa uso de memoria
  y escala según demanda
```

---

## ⚙️ ¿Cómo desplegar?

1. 🧭 Asegúrate de tener acceso a un clúster Kubernetes.
2. Crea el namespace si no existe:

   ```bash
   kubectl create namespace dcsarapp
   ```

3. Aplica los manifiestos:

   ```bash
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   kubectl apply -f hpa.yaml
   ```

4. Verifica los recursos:

   ```bash
   kubectl get all -n dcsarapp
   kubectl describe hpa hpa-dcsar-basic-landing-page -n dcsarapp
   ```

---

## 📚 Recursos útiles

- 📖 [Documentación oficial de Kubernetes](https://kubernetes.io/es/docs/home/)
- 🎓 [Conceptos clave: Pods, Deployments, Services, Namespaces, HPA](https://kubernetes.io/es/docs/concepts/)
- 🧪 [Probar servicios con curl](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/#accessing-services-running-on-the-cluster)

---
