# Registro de Prompts - Pipeline de CI/CD (GitHub Actions & AWS EC2)

## 🎯 Fase 1: Trigger, Entorno Dinámico y Bloque de Tests
### Prompt Inicial:
> Actúa como un Ingeniero DevOps Senior. Necesito generar el esqueleto inicial de un pipeline en YAML para GitHub Actions para un monorepo donde el servidor está en la carpeta `/backend`. El pipeline debe activarse únicamente cuando haya un push en una rama con un Pull Request abierto apuntando a 'main'. Configura el primer job para correr en ubuntu-latest, instalar dependencias de Node 20 utilizando la caché del gestor de paquetes apuntando al package-lock de la subcarpeta, y ejecutar 'npm test'. Asegúrate de usar versiones estables de Actions actualizadas a 2026 (@v4).

---

## 📦 Fase 2: Secuenciación y Generación de Artifacts (Build)
### Prompt de Refinamiento:
> Añade un segundo job llamado 'build' que dependa explícitamente del éxito del job de 'test'. Este job debe realizar la instalación limpia con 'npm ci', compilar el proyecto TypeScript mediante 'npm run build' dentro del subdirectorio del backend, y empaquetar de forma segura la carpeta de producción resultante (`dist/`) usando 'actions/upload-artifact@v4' para su posterior consumo.

---

## 🚀 Fase 3: Despliegue Continuo Seguro (AWS EC2) con Control de Errores
### Prompt Final de Integración:
> Completa el pipeline añadiendo un tercer job llamado 'deploy' que dependa del build. Debe descargar el artefacto compilado y transferirlo a una instancia EC2 de AWS mediante SSH utilizando acciones oficiales de appleboy (`scp-action` y `ssh-action`). Los valores de conexión del servidor (Host, User y SSH Private Key) deben ser leídos de manera segura desde GitHub Secrets. Al finalizar la copia, el pipeline debe conectarse por SSH al servidor, instalar dependencias de producción y reiniciar el proceso de la API usando el gestor de procesos PM2.