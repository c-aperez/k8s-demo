# 🐳 Dockerfile para una landing page con Nginx

Este proyecto contiene un `Dockerfile` simple que construye una imagen de contenedor basada en Nginx para servir archivos estáticos como HTML, CSS y JavaScript.

---

## 📦 ¿Qué hace este Dockerfile?

Este archivo crea una imagen basada en [**nginx:alpine**](https://hub.docker.com/_/nginx) y copia tu landing page directamente al directorio raíz de Nginx.

```dockerfile
FROM nginx:alpine
```

🔹 Usa una imagen ligera de Nginx (basada en Alpine Linux).

---

```dockerfile
COPY . /usr/share/nginx/html
```

📁 Copia el contenido del proyecto (HTML, CSS, JS, etc.) al directorio por defecto de Nginx.

---

```dockerfile
EXPOSE 80
```

🌐 Expone el puerto 80 para servir tu aplicación.

---

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

🚀 Inicia Nginx en primer plano. Esto es importante en Docker para mantener el proceso en ejecución.

---

## 🛠️ ¿Cómo construir y ejecutar esta imagen?

1. Construye la imagen:

   ```bash
   docker build -t mi-landing-page .
   ```

2. Ejecuta el contenedor:

   ```bash
   docker run -d -p 8080:80 mi-landing-page
   ```

3. Abre en tu navegador:

   ```
   http://localhost:8080
   ```

## 🛠️ ¿Cómo construir y subir esta imagen a mi cluster de Kind?

1. Construye la imagen:

   ```bash
   docker build -t mi-landing-page .
   ```

2. Cargar la imagen nueva al Cluster:

   ```bash
   kind load docker-image <mi-landing-page> --name <nombre-de-cluster>
   ```


---

## 📚 Recursos útiles

- 📘 [Documentación oficial de Nginx](https://nginx.org/en/docs/)
- 🐳 [Documentación oficial de Docker](https://docs.docker.com/)
- 🧱 [Base de imágenes oficiales en Docker Hub](https://hub.docker.com/_/nginx)

---