docker-compose.yaml

:

```yaml


version: '3.8'
```
- **version**: Especifica la versión del formato de archivo de Docker Compose que estás utilizando. La versión `3.8` es compatible con Docker Swarm.

```yaml
services:
  filebrowser:
```
- **services**: Define los servicios que se ejecutarán en el contenedor.
- **filebrowser**: Nombre del servicio.

```yaml
    image: hurlenko/filebrowser:latest
```
- **image**: Especifica la imagen de Docker que se utilizará para el servicio. En este caso, `hurlenko/filebrowser:latest` es la imagen más reciente de File Browser.

```yaml
    environment:
      - UID=1000
      - GID=1000
      - TZ=America/Santiago
```
- **environment**: Define las variables de entorno que se pasarán al contenedor.
  - `UID=1000`: Establece el ID de usuario dentro del contenedor.
  - `GID=1000`: Establece el ID de grupo dentro del contenedor.
  - `TZ=America/Santiago`: Establece la zona horaria dentro del contenedor.

```yaml
    ports:
      - 6005:8080
```
- **ports**: Mapea los puertos del contenedor a los puertos del host.
  - `6005:8080`: Mapea el puerto 8080 del contenedor al puerto 6005 del host.

```yaml
    volumes:
      - /media/ruta/disco:/data
      - ./fb-config:/config
```
- **volumes**: Monta volúmenes en el contenedor.
  - `/media/ruta/disco:/data`: Monta el directorio `/media/ruta/disco` del host en el directorio `/data` del contenedor.
  - `./fb-config:/config`: Monta el directorio `./fb-config` del host en el directorio `/config` del contenedor.

```yaml
    deploy:
      replicas: 3
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 120s
      update_config:
        parallelism: 2
        delay: 10s
        failure_action: rollback
        monitor: 60s
        max_failure_ratio: 0.1
        order: start-first
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
      endpoint_mode: vip
```
- **deploy**: Define las opciones de despliegue específicas para Docker Swarm.
  - **replicas: 3**: Ejecuta tres réplicas del servicio para mejorar la disponibilidad.
  - **restart_policy**: Define la política de reinicio del servicio.
    - `condition: on-failure`: Reinicia el servicio solo si falla.
    - `delay: 5s`: Espera 5 segundos antes de reiniciar.
    - `max_attempts: 3`: Intenta reiniciar un máximo de 3 veces.
    - `window: 120s`: Cuenta los intentos de reinicio en una ventana de 120 segundos.
  - **update_config**: Configuración de actualización del servicio.
    - `parallelism: 2`: Actualiza dos tareas en paralelo.
    - `delay: 10s`: Espera 10 segundos entre actualizaciones.
    - `failure_action: rollback`: Revierte la actualización si falla.
    - `monitor: 60s`: Monitorea cada actualización durante 60 segundos.
    - `max_failure_ratio: 0.1`: Permite un máximo de 10% de fallos.
    - `order: start-first`: Inicia las nuevas tareas antes de detener las antiguas.
  - **resources**: Límites de recursos.
    - `limits`: Límites de recursos.
      - `cpus: '0.50'`: Límite de 0.5 CPUs.
      - `memory: 512M`: Límite de 512 MB de memoria.
  - **endpoint_mode: vip**: Utiliza el modo de endpoint VIP (Virtual IP) para el servicio.

```yaml
networks: {}
```
- **networks**: Define las redes a las que se conectarán los servicios. En este caso, no se han definido redes adicionales, por lo que se utiliza la red predeterminada.

Estas configuraciones permiten definir cómo se ejecutará y gestionará el servicio `filebrowser` en un entorno de Docker Swarm, asegurando alta disponibilidad, políticas de reinicio, actualizaciones controladas y límites de recursos.
