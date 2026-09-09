# 🖥️ Gestión de Infraestructura, Redes y Data Center

Documentación técnica sobre la administración, segmentación de red y acceso Out-of-Band (OOB) en entorno hospitalario/público.

---

## 🛠️ Tecnologías y Hardware Administrado
* **Networking:** Switches Managed Cisco SF300 (VLANs, Access/Trunk ports).
* **Gestión de Servidores:** KVM sobre IP Tripp-Lite (B020-U16-19-IP), PDU Tripp-Lite.
* **Sistemas Operativos & Web Server:** Linux (Ubuntu/Debian), Windows Server, Apache/Nginx.
* **Bases de Datos:** PostgreSQL, Oracle, MySQL.

---

## 🔒 1. Segmentación de Red (Cisco Switches & VLANs)
* Configuración de VLANs para aislamiento de tráfico crítico (p. ej. **VLAN 20** para sistemas de diagnóstico/servidores).
* Asignación de puertos `Access` para nodos finales y `Trunk` para interconexión de tráfico taggeado entre switches.

## 🖥️ 2. Administración Out-of-Band (KVM sobre IP Tripp-Lite)
* Configuración de interfaz de administración remota fuera de banda (`172.16.0.249`).
* Despliegue de cliente remoto `Java Client` (JNLP) para control total a nivel de hardware/BIOS del servidor.
* Resolución de incidencias de arranque, configuración de discos virtuales y contingencias de red sin dependencia del sistema operativo base.

## 🌐 3. Acceso Remoto de Emergencia & Zero-Trust Mesh VPN (Tailscale)

* **Problema Operativo:** El servidor físico requiere intervención manual en el arranque (press key to boot) tras cortes de energía imprevistos fuera del horario laboral.
* **Solución de Arquitectura:**
  * Despliegue de nodo **Ubuntu Server** configurado con **Tailscale (Subnet Router)**.
  * Enrutamiento de subredes locales mediante `--advertise-routes` para exponer la red local del Data Center (`172.16.0.0/24`) de forma segura.
  * Enlace Zero-Trust VPN que permite acceso fuera de banda (OOB) a la interfaz del KVM Tripp-Lite desde cualquier ubicación externa 24/7.
  * **Resultado:** Eliminación del tiempo de inactividad (Downtime) y resolución de contingencias críticas de hardware de forma remota en minutos.
