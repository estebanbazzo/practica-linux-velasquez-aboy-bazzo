# Trabajo Práctico Final - Administración de Sistemas Linux

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Vagrant](https://img.shields.io/badge/Vagrant-1563FF?style=for-the-badge&logo=vagrant&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
![Shell Script](https://img.shields.io/badge/Shell_Script-121011?style=for-the-badge&logo=gnu-bash&logoColor=white)

![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)

![Grafana](https://img.shields.io/badge/grafana-%23F46800.svg?style=for-the-badge&logo=grafana&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=Prometheus&logoColor=white)
![Loki](https://img.shields.io/badge/Loki-F46800?style=for-the-badge&logo=grafana&logoColor=white)

![Apache](https://img.shields.io/badge/apache-%23D42029.svg?style=for-the-badge&logo=apache&logoColor=white)
![MySQL](https://img.shields.io/badge/mysql-%2300f.svg?style=for-the-badge&logo=mysql&logoColor=white)
![PHP](https://img.shields.io/badge/php-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)

**Universidad Tecnológica Nacional - Facultad Regional Avellaneda (UTN-FRA)** **Tecnicatura Universitaria en Programación** **Materia:** Arquitectura y Sistemas Operativos  
**Comisión:** 311  
**Grupo:** 5  
**Cuatrimestre:** 2° - 2025

## 👥 Integrantes del Equipo

* **Velasquez, Camila**
* **Aboy, Federico**
* **Bazzo, Esteban**

---

## 📝 Introducción y Objetivos

El objetivo principal de este trabajo práctico fue la implementación colaborativa de un entorno de administración de sistemas Linux utilizando virtualización con Vagrant. A través de la división de roles (Administrador, Desarrollador y Operador), el equipo simuló un escenario real de trabajo IT, abarcando desde la configuración básica del sistema hasta el despliegue de servicios en contenedores y monitoreo.

El proyecto se centró en los siguientes ejes temáticos:
1.  Control de versiones con **Git**.
2.  Gestión de **Usuarios y Permisos** en Linux.
3.  Administración de almacenamiento flexible con **LVM**.
4.  Automatización y gestión de archivos (Scripting básico).
5.  Orquestación de contenedores con **Docker Compose**.
6.  Implementación de un stack de monitoreo (**Prometheus, Loki, Grafana**).
7.  **Bonus:** Despliegue de un servidor **LAMP** completo.

---

## 🚀 Proceso de Resolución

### 1. Inicialización del Entorno y Colaboración
Cada integrante inicializó su máquina virtual utilizando el `Vagrantfile` provisto. Se configuró el repositorio remoto en GitHub y se estableció un flujo de trabajo colaborativo.
- Se documentaron las direcciones IP de cada nodo en `informacion/ip_vm.txt`.
- Se generó un reporte de información del sistema unificado mediante `fastfetch` en el archivo `informacion/system_info.txt`, consolidando la salida de los tres integrantes.

### 2. Gestión de Permisos y Usuarios
Se simularon entornos de trabajo multi-usuario para comprender la seguridad en Linux:
- Se crearon usuarios locales (`estudiante1`, `estudiante2`, `estudiante3`) y el grupo `equipotrabajo`.
- Se configuraron directorios con permisos específicos:
    - **Directorios Personales:** Acceso exclusivo (permisos `600` para archivos privados).
    - **Directorio Colaborativo:** Acceso compartido mediante SGID y permisos de grupo (`770` en `/tmp/colaborativo`), permitiendo que el grupo `equipotrabajo` colabore sin intervención de terceros.
- **Evidencia:** Verificada en `permisos/verificacion_permisos.txt` y `permisos/usuarios_[apellido].txt`.

### 3. Administración de Almacenamiento (LVM)
Para gestionar el disco adicional de 2GB de manera flexible, se utilizó LVM (Logical Volume Manager):
1.  Creación de Volúmenes Físicos (PV) sobre el disco adicional (`/dev/sdc`).
2.  Creación de Grupos de Volúmenes (VG) personalizados: `vg_datos_[apellido]`.
3.  Creación de Volúmenes Lógicos (LV) de 1.5GB: `lv_storage_[apellido]`.
4.  Formateo en `ext4` y montaje persistente en `/mnt/lvm_storage_[apellido]` mediante `/etc/fstab`.
- **Evidencia:** El estado del disco antes y después del montaje se detalla en la carpeta `lvm/`.

### 4. Gestión de Archivos y Directorios
Se realizó una estructura de directorios (`proyectos`, `activos`, `archivados`, etc.) dentro del volumen LVM. Se ejecutaron operaciones masivas de creación, copia y movimiento de archivos de texto para validar la manipulación del sistema de archivos.
- **Evidencia:** La estructura final se encuentra en `archivos/verificacion_archivos.txt`.

### 5. Contenedores y Monitoreo (Docker)
Se desplegó un stack de servicios interconectados utilizando Docker Compose.

**Servicios desplegados:**
* **Base de datos:** Postgres y Redis.
* **Servidor Web:** Nginx.
* **Monitoreo:** Prometheus (métricas), Loki (logs) y Grafana (visualización).

#### 🔧 Resolución de Problemas (Debugging)
Durante la implementación inicial del `docker-compose.yml` y `prometheus.yml`, nos encontramos con errores intencionales que fueron resueltos:

1.  **Configuración de Loki:** El contenedor de Loki fallaba al iniciar (`err="empty ring"`). Se detectó que faltaba el archivo de configuración.
    * *Solución:* Se creó el archivo `loki-config.yml` y se montó correctamente como volumen en el `docker-compose.yml`.
2.  **Targets de Prometheus:** Se verificó en la UI de Prometheus que el target de Nginx no se encontraba o daba error.
    * *Análisis:* Se corrigió la configuración en `prometheus.yml` para asegurar que Prometheus apunte a los puertos y servicios correctos que exponen métricas.
3.  **Redes y Volúmenes:** Se ajustaron las definiciones de redes (`monitoring`) para asegurar la visibilidad entre los contenedores.

El detalle completo de los errores y sus soluciones se encuentra en `contenedores/errores_encontrados.md`.

### 6. ⭐ Bonus: Servidor LAMP
El integrante **Esteban Bazzo** implementó exitosamente un servidor LAMP (Linux, Apache, MySQL, PHP) nativo sobre la VM.
* Se instaló y configuró Apache2, MySQL Server y PHP 8.1.
* Se creó una base de datos `tp_final_db` y un usuario `alumno` con privilegios.
* Se desarrolló una página web de prueba (`index.html`) y un script de conexión a base de datos (`test_db.php`).
* **Evidencia:** Las capturas de pantalla del sitio funcionando y la verificación de servicios se encuentran en la carpeta `proyecto/lamp/`.

---

## 🏁 Conclusiones

La realización de este trabajo práctico nos permitió consolidar los conocimientos teóricos vistos en la cursada mediante la práctica intensiva:

1.  **Virtualización y Entornos:** Comprendimos la importancia de `Vagrant` para garantizar que todos los desarrolladores trabajen sobre la misma infraestructura base, eliminando el problema de "en mi máquina funciona".
2.  **Seguridad:** La gestión manual de permisos (`chmod`, `chown`, ACLs básicas con grupos) nos demostró cómo Linux protege la información multi-usuario a bajo nivel.
3.  **Flexibilidad del Storage:** LVM resultó ser una herramienta poderosa. A diferencia de las particiones estáticas, pudimos ver cómo sería posible extender el espacio de almacenamiento "en caliente" si fuera necesario en un entorno de producción.
4.  **Containerización:** Docker Compose simplificó enormemente el despliegue de una arquitectura de microservicios compleja (Base de datos + Web + Monitoreo). Aprendimos que la correcta configuración de volúmenes y redes es crítica para la persistencia de datos y la comunicación entre servicios.
5.  **Trabajo en Equipo:** El uso de Git fue fundamental para integrar el trabajo de los tres miembros (LVM, scripts, Docker, LAMP) en un único entregable coherente.

---

## 📂 Estructura del Repositorio

```text
practica-linux-velasquez-aboy-bazzo/
├── proyecto/
│   ├── README.md                   # Documentación general (este archivo)
│   ├── Vagrantfile                 # Configuración de la VM
│   ├── informacion/
│   │   ├── ip_vm.txt               # IPs de los integrantes
│   │   └── system_info.txt         # Salida de fastfetch colaborativo
│   ├── permisos/
│   │   ├── usuarios_[apellido].txt # Info de usuarios y grupos
│   │   └── verificacion_permisos.txt
│   ├── lvm/
│   │   └── lvm-[apellido].txt      # Evidencia de particionamiento LVM
│   ├── archivos/
│   │   └── verificacion_archivos.txt
│   ├── contenedores/
│   │   ├── docker-compose.yml      # Archivo orquestador corregido
│   │   ├── prometheus.yml          # Configuración de métricas corregida
│   │   ├── loki-config.yml         # Configuración de logs (agregada)
│   │   ├── errores_encontrados.md  # Reporte de debugging
│   │   ├── logs_completos.txt      # Logs de ejecución
│   │   └── verificacion_contenedores.txt
│   │   └── capturas/               # Evidencia visual de Grafana/Docker
│   └── lamp/ (Bonus)
│       ├── verificacion_lamp.txt   # Estado de servicios Apache/MySQL
│       ├── comandos_ejecutados.txt # Historial de comandos
│       └── capturas/               # Screenshots del sitio web y PHP
└── Trabajo Práctico - Administración de Sistemas Linux con Vagrant.pdf