1. Error: Loki no encontraba el archivo de configuración

Mensaje detectado:

level=error ... "error getting ingester clients" err="empty ring"

Esto ocurría porque Loki intentaba cargar el archivo y no existía.

Cómo lo identifiqué:

docker-compose logs loki
grep -n "error" logs_completos.txt

Solución:

Crear el archivo loki-config.yml dentro de la carpeta contenedores/

Montarlo correctamente en el docker-compose

Verifiqué la solución cuando apareció:

msg="Loading configuration file" filename=/etc/loki/local-config.yaml

2. Problemas con Prometheus targets y su explicación

Al entrar a la página:

http://<IP_DE_MI_VM>:9090/targets

solo me aparecía el siguiente target:

Punto final: http://localhost:9090/metrics
Estado: UP
Trabajo: prometheus

Esto es correcto, porque en mi archivo prometheus.yml solo está configurado ese target, y NO está configurado Nginx ni ningún otro servicio.

Por ese motivo:

Prometheus aparece UP

No aparece Nginx, ya que nunca se agregó como job en prometheus.yml

3. Errores del Docker Compose original

Errores identificados:

Falta de archivo loki-config.yml

Servicios sin configuración consistente

Targets mal configurados

Dependencias entre contenedores sin orden correcto

Faltaba el volumen de Loki

Cómo se identificaron:

docker-compose config
docker-compose logs
docker-compose ps

Solución:

Crear el archivo de configuración

Ajustar rutas y volumes

Arreglar Prometheus targets

Levantar nuevamente con:

docker-compose down
docker-compose up -d
