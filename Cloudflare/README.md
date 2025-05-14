# 🌐 Configuración de Cloudflared Tunnels en Kubernetes

Este documento explica paso a paso cómo crear y desplegar un **Cloudflared Tunnel** en Kubernetes para exponer servicios internos de forma segura mediante Cloudflare.

---

## 📋 Requisitos previos

- Tener acceso a una cuenta de [Cloudflare](https://dash.cloudflare.com/)
- Tener el binario de `cloudflared` instalado ([guía oficial](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation/))
- Tener configurado `kubectl` para acceder a tu clúster de Kubernetes

---

## 🚀 Pasos para configurar el túnel

### 1️⃣ Iniciar sesión en Cloudflare

```bash
cloudflared tunnel login
```

🔐 Se abrirá una ventana del navegador para autenticarte en tu cuenta de Cloudflare y seleccionar el dominio a usar.

---

### 2️⃣ Crear el túnel

```bash
cloudflared tunnel create example-tunnel
```

🛠️ Esto generará un túnel con un ID único y guardará un archivo de credenciales (`.json`) en tu máquina. El nombre puede ser el del dominio o un identificador personalizado.

---

### 3️⃣ Crear un **Secret** en Kubernetes con las credenciales

```bash
kubectl create secret generic tunnel-credentials \
  --from-file=credentials.json=/ruta/a/.cloudflared/<TUNNEL-ID>.json \
  -n <NAMESPACE>
```

🔒 Este `Secret` será utilizado por el pod de `cloudflared` para autenticarse.

---

### 4️⃣ Crear el registro DNS tipo CNAME en Cloudflare

```bash
cloudflared tunnel route dns example-tunnel subdominio.tudominio.com
```

🌍 Esto enlaza el túnel a tu subdominio de forma automática mediante un registro CNAME.

---

### 5️⃣ Desplegar los recursos en Kubernetes

```bash
kubectl apply -f cloudflared.yaml -n <NAMESPACE>
```

📦 Esto aplicará la configuración de los pods y servicios necesarios para que `cloudflared` funcione dentro de tu clúster.

---

## 🧪 Verificación

Puedes verificar que el túnel esté activo desde la consola de Cloudflare:
[https://dash.cloudflare.com](https://dash.cloudflare.com)

También puedes verificar los logs del pod:

```bash
kubectl logs deployment/cloudflared -n <NAMESPACE>
```

---

## 🧼 Limpieza

Si deseas eliminar el túnel:

```bash
cloudflared tunnel delete example-tunnel
```

Y no olvides eliminar el secreto y los recursos de Kubernetes si ya no los necesitas.

---

## 📚 Recursos útiles

- [Documentación oficial de Cloudflared + Kubernetes](https://developers.cloudflare.com/cloudflare-one/tutorials/many-cfd-one-tunnel/)
- [Cloudflared Tunnel GitHub](https://github.com/cloudflare/cloudflared)

---

¿Listo para usar Cloudflare como tu entrada segura al clúster? 🌈🚪
