FROM nginx:alpine
# 🐳 Usa la imagen oficial de Nginx basada en Alpine Linux.
# Alpine es una distribución mínima y muy ligera, ideal para contenedores.

RUN apk add --no-cache stress-ng
# 🛠️ Instala stress-ng, una herramienta para generar carga en el sistema.

COPY . /usr/share/nginx/html
# 📁 Copia todos los archivos del directorio actual al directorio raíz de Nginx.
# Este es el lugar donde Nginx sirve archivos estáticos como HTML, CSS o JS.

EXPOSE 80
# 🌐 Expone el puerto 80, que es el puerto estándar para HTTP.

CMD ["nginx", "-g", "daemon off;"]
# 🚀 Inicia Nginx en primer plano. Esto es necesario para que el contenedor no termine inmediatamente.
