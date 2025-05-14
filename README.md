<p align="center">
  <a href="" rel="noopener">
 <img width=325px height=325px src="https://accuknox.com/wp-content/uploads/kubernetes-hero-animation.gif" alt="Logo K8S"></a>
</p>

<h1 align="center">Kubernetes - K8S</h1>

<p align="center"> Este repositorio contiene todo lo necesario para levantar una página web dentro de un clúster de Kubernetes ejecutado con Kind y expuesto mediante Cloudflare y Cloudflare Tunnels.
    <br> 
</p>

---

## 📝 Contenido

- [Requisitos previos antes de iniciar](#requisitos)
  - [Instalación de Docker](#docker)
  - [Instalación de Kind](#kind)
  - [Instalación de kubectl](#kubectl)
  - [Instalación de Cloudflare CLI (Cloudflared)](#cloudflare)

---

## 📋 Requisitos

Antes de levantar este entorno necesitas tener las siguientes herramientas instaladas en tu equipo:

### 🐳 Docker <a name="docker"></a>

Docker es la plataforma que nos permite ejecutar contenedores de forma aislada en nuestro equipo.

🔗 [Guía oficial de instalación de Docker](https://docs.docker.com/get-docker/)

**Instalación rápida en Linux (Ubuntu):**

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
```

> ℹ️ Recuerda cerrar y volver a iniciar sesión para aplicar los permisos del grupo `docker`.

---

### 🌱 Kind (Kubernetes IN Docker) <a name="kind"></a>

Kind permite crear un clúster de Kubernetes dentro de contenedores Docker. Es útil para entornos de desarrollo y pruebas.

🔗 [Guía oficial de Kind](https://kind.sigs.k8s.io/)

**Instalación rápida:**

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

> Reemplaza `linux-amd64` por `darwin-arm64`, `windows-amd64`, etc. según tu sistema.

---

### 📦 kubectl <a name="kubectl"></a>

`kubectl` es la herramienta oficial para interactuar con clústeres Kubernetes desde la línea de comandos.

🔗 [Guía oficial de instalación de kubectl](https://kubernetes.io/docs/tasks/tools/)

**Instalación rápida:**

```bash
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

> Verifica la instalación con: `kubectl version --client`

---

### ☁️ Cloudflared CLI <a name="cloudflare"></a>

Cloudflared es el cliente de línea de comandos para interactuar con [Cloudflare Tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/), que permiten exponer servicios locales de manera segura al exterior sin necesidad de abrir puertos.

🔗 [Documentación de Cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install/)

**Instalación rápida en Linux:**

```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
```

> Verifica que funcione con: `cloudflared tunnel --help`

---

## ✅ Con todo esto instalado podrás levantar el entorno de pruebas local y exponerlo mediante Cloudflare 🚀
