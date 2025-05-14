# 🐳 Configuración de clúster local con Kind (Kubernetes IN Docker)

Este archivo YAML permite crear un clúster de Kubernetes local utilizando [**Kind**](https://kind.sigs.k8s.io/), ideal para pruebas, desarrollo y aprendizaje.

---

## 🛠️ ¿Qué es Kind?

[Kind (Kubernetes IN Docker)](https://kind.sigs.k8s.io/docs/user/quick-start) es una herramienta que permite ejecutar clústeres de Kubernetes **dentro de contenedores Docker**. Es perfecta para:

- Desarrollo local
- Integración continua (CI)
- Pruebas automatizadas

📦 Cada nodo del clúster se ejecuta como un contenedor Docker con una imagen oficial de Kubernetes.

---

## ⚙️ Estructura del clúster

Este clúster está compuesto por:

- 🧠 **1 nodo de control (control-plane)**: gestiona el estado del clúster (scheduler, controller manager, API server, etcd).
- 🧱 **3 nodos de trabajo (workers)**: ejecutan las aplicaciones (pods, servicios, etc.).

```text
[Control Plane]
     │
 ┌───┴─────────┐───────────┐
 │             │           │
[Worker 1] [Worker 2] [Worker 3]
```

---

## 📄 Contenido del archivo

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  image: kindest/node:v1.32.0
- role: worker
  image: kindest/node:v1.32.0
- role: worker
  image: kindest/node:v1.32.0
- role: worker
  image: kindest/node:v1.32.0
```

---

## 🚀 ¿Cómo crear el clúster?

1. Asegúrate de tener Docker y Kind instalados:

   ```bash
   docker --version
   kind version
   ```

2. Guarda este archivo como `kind-cluster.yaml`.

3. Crea el clúster:

   ```bash
   kind create cluster --name ''Nombre-de-tu-cluster'' --config clusterConfig.yaml
   ```

4. Verifica el estado:

   ```bash
   kubectl cluster-info --context kind-Nombre-de-tu-cluster
   kubectl get nodes
   ```

---
### 📊 Instalación de Metric Server

Metrics Server es un agregador de métricas ligeras de recursos como CPU y memoria. Recoge esta información directamente del kubelet en cada nodo y la expone a través de la API de Kubernetes.

📉 En español... te brinda metricas de consumo de CPU/RAM de tus PODS, este está listo para utilizarse con escaladores automaticos mejor conocidos como HPA.

1. Instalación de Metric Server con Helm

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

2. Edita el deployment para que funcione en Kind
```
kubectl -n kube-system edit deployment metrics-server
```

- Y en la sección containers.args, agrega esta línea:
```
    - --kubelet-insecure-tls
```

---
## 🧹 ¿Cómo eliminar el clúster?

```bash
kind delete cluster --name ''Nombre-de-tu-cluster''
```

---

## 📚 Recursos útiles

- 📘 [Documentación oficial de Kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
- 📦 [Lista de imágenes oficiales de nodos](https://hub.docker.com/r/kindest/node/tags)
- 🎓 [Conceptos básicos de Kubernetes](https://kubernetes.io/es/docs/concepts/)

---
