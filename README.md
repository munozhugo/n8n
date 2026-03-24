# n8n-local

Repositorio local para ejecutar n8n junto con Loki y Promtail usando Docker Compose.

Servicios incluidos

- n8n: interfaz de automatización (puerto 5678). Datos persistentes en el volumen `n8n_data`.
- loki: almacenamiento de logs (puerto 3100).
- promtail: recolector de logs, usa `promtail-config.yml` y monta el volumen `n8n_data` en modo lectura.

Notas importantes

- La red `monitoring_network` está marcada como `external: true` en `docker-compose.yaml`. Cree la red si no existe:

```bash
docker network create monitoring_network
```

- Variables de entorno añadidas (para evitar deprecaciones):
  - `DB_SQLITE_POOL_SIZE=5` — pool de conexiones para SQLite (debe ser > 0).
  - `N8N_RUNNERS_ENABLED=true` — habilita los task runners.
  - `N8N_BLOCK_ENV_ACCESS_IN_NODE=true` — evita acceso a variables de entorno desde Code Node. Poner `false` si necesita acceso desde nodos de código o expresiones.
  - `N8N_GIT_NODE_DISABLE_BARE_REPOS=true` — deshabilita repositorios "bare" en el Git Node.

Cargar / reiniciar el stack

En el directorio del proyecto:

```bash
docker-compose down
docker-compose up -d --force-recreate
```

Si necesita reconstruir imágenes (no necesario para este setup básico):

```bash
docker-compose up -d --build --force-recreate
```

Enlaces útiles

- n8n — Configuración de SQLite: https://docs.n8n.io/hosting/configuration/environment-variables/database/#sqlite
- n8n — Task runners: https://docs.n8n.io/hosting/configuration/task-runners/
- n8n — Seguridad: https://docs.n8n.io/hosting/configuration/environment-variables/security/

Si desea que permita acceso a variables de entorno desde Code Node, dígamelo y cambio `N8N_BLOCK_ENV_ACCESS_IN_NODE=false` en `docker-compose.yaml`.
