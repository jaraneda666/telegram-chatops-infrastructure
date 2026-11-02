# Manual Operativo de ChatOps Móvil (Telegram + n8n)

## 1. Arquitectura y Enrutamiento

El sistema opera bajo el principio de webhook unificado y filtrado de identidad por lista blanca en el primer nodo de entrada:

```text
[App Telegram en Smartphone]
            │
            │  Envío de Link (https://...)  o  /menu / Botón Inline
            ▼
[n8n: Telegram Trigger (Webhook Único)]  (ID Workflow: rEShfn3jmYt83IVr)
            │
            ▼
[Filtro de Seguridad e Identidad]  (Valida User ID: 1307787776)
       │                 │
       ├─(Autorizado)    └─(No Autorizado)──► Mensaje de Rechazo ⛔
       ▼
[Router de Comandos / Switch]
  ├── [CLIPPER]    ──► Detección de URL ➔ ACK ➔ Scraping Jina/GH ➔ IA Gemini ➔ Obsidian API ➔ Confirmación Telegram + Gmail
  ├── [MENU]       ──► Menú táctil principal con botones Inline
  ├── [OPENWRT]    ──► Telemetría WAN, LAN, VLANs, clientes DHCP y Wi-Fi
  ├── [SYNOLOGY]   ──► Salud NAS, volumen Btrfs, SMART, Docker y Reverse Proxy
  ├── [UNIFI]      ──► Estado switch US-8-60W, PoE detallado por puerto y troncales
  ├── [AUDIT]      ──► Superficie de ataque, MiniUPnPd, filtrado DNS y perímetro
  ├── [HITL GATE]  ──► Confirmación de acciones de alto impacto
  └── [UNKNOWN]    ──► Mensaje guía con botón interactivo al menú principal
```

---

## 2. Guía Rápida de Uso desde el Móvil

### Iniciar el Control
1. Abrí el chat con tu bot de Telegram (*Multi RSS / ChatOps Bot*).
2. Enviá el comando `/start` o `/menu`.
3. El bot desplegará el **Menú Principal con botones táctiles**:
   - `[ 🌐 OpenWrt Router ]` `[ 🖧 Switch UniFi ]`
   - `[ 🗄️ Synology NAS   ]` `[ 🛡️ Auditoría Red ]`
   - `[ 🔄 Actualizar Menú ]`

### Navegación Jerárquica y Telemetría Técnica Detallada

Al pulsar cualquiera de los botones principales, el bot despliega su **Submenú específico**. Cada opción entrega métricas e inventarios con granularidad técnica pericial:

* **Submenú OpenWrt (`192.168.1.1`):**
  - `[ 📊 Estado & WAN/LAN ]`: Uptime exacto, carga promedio (`load average`), uso de memoria RAM (MB y %), interfaz WAN `pppoe-wan` con IP pública, MTU 1492, tráfico acumulado RX/TX, gestión de colas SQM CAKE (900/450 Mbps) y direccionamiento de VLANs Core (50, 60 y 70).
  - `[ 👥 Clientes DHCP ]`: Inventario completo con nombre de equipo, dirección IP estática/dinámica, dirección MAC física y red/VLAN conectada (VLAN 50 LAN Principal, VLAN 60 IoT y VLAN 70 Invitados bajo openNDS 10.3).
  - `[ 🛡️ Firewall & Reglas ]`: Políticas por zona de `fw4` / `nftables` (WAN DROP stealth, LAN, IoT y Guest), reenvío DNAT BitTorrent (58130), hardening de Dropbear SSH por llave Ed25519 y confinamiento estricto de MiniUPnPd a `/32`.
  - `[ 📶 SSIDs & Roaming ]`: Puntos de acceso UniFi AC Pro autónomos (AP1 Salón y AP2 Habitaciones), canales 5 GHz (VHT80) y 2.4 GHz (HT20), potencias de transmisión (dBm), enlaces PoE, itinerancia 802.11r/k/v con Mobility Domain 5001 y SSIDs mapeados.

* **Submenú Synology (`192.168.1.100`):**
  - `[ 🐳 Contenedores Docker ]`: Telemetría de Container Manager / Docker Engine 24.0.2 con estado de ejecución, imágenes, puertos TCP/UDP y consumo de RAM/CPU para `n8n`, `n8n-DB` (PostgreSQL), `Obsidian` (CouchDB), `Pi-hole v6`, `sshwifty_web` y `portainer`, junto con la protección de la cadena `DOCKER-USER`.
  - `[ 💾 Salud de Discos ]`: CPU, memoria RAM disponible, Storage Pool 1 en RAID 1 Btrfs (capacidad usada/libre), y telemetría SMART por disco Seagate IronWolf 8TB (temperaturas, horas de uso, 0 sectores reasignados y estado de salud).
  - `[ 🔒 Proxy Inverso & ACLs ]`: Servidor Nginx 1.20, certificado Let's Encrypt Wildcard `*.admin.synology.me`, tabla de mapeo de subdominios (`n8n`, `obsidian`, `pihole`, `dsm`, `lab-dash`), perfiles de acceso ACL `LAN_y_VPN` y cabeceras de seguridad HSTS.
  - `[ 🛡️ Firewall DSM ]`: Perfil activo `Hardening_LAN_y_VPN` con política por defecto `INPUT DROP`, reglas para LAN, OpenVPN, DNS para VLAN 70 y BitTorrent WAN, además de hardening en `sshd_config` (Ed25519, sin passwords, sin root directo).

* **Submenú UniFi (US-8-60W):**
  - `[ 🔌 Puertos & Consumo PoE ]`: Presupuesto PoE total (60W), consumo activo en watts y porcentaje, temperatura de chasis y estado detallado puerto a puerto (1 al 8) indicando estado de enlace (Up/Down), velocidad (1000M Full / 100M Full), dispositivo conectado y consumo PoE individual (AP1 y AP2).
  - `[ 🌐 Troncales VLAN ]`: Base de datos 802.1Q en UniFi Controller (VLAN 1, 50, 60, 70), enlace FastPath Gateway (`192.168.1.1`), asignación troncal etiquetada en Puerto 3 (Uplink), Puerto 6 (AP2), Puerto 8 (AP1) y acceso untagged en Puerto 4.
  - `[ 👥 Clientes Conectados ]`: Tabla de conmutación FDB (Forwarding Database) con asignación de direcciones MAC por puerto, estadísticas de tráfico, 0 paquetes descartados y 0 colisiones de red.

* **Submenú Auditoría:**
  - `[ 🚪 Puertos Expuestos ]`: Radiografía de superficie exterior WAN (0 puertos administrativos expuestos, DNAT 58130, puertos 80/443 con ACL) vs superficie LAN interna protegida.
  - `[ 🛑 Verificación UPnP ]`: Inspección de MiniUPnPd con denegación implícita en la subred `/24`, exclusividad para el host de trabajo `127.0.0.1/32` y 0 mapeos autónomos no autorizados.
  - `[ 🌐 Filtrado DNS ]`: Arquitectura de resolutores primario (AdGuard Home en router con ~340.000 reglas y DoH Quad9/Cloudflare) y secundario (Pi-hole en NAS), con verificación de split-horizon y cero fugas DNS (DNS Leaks).
  - `[ 🔍 Escaneo Perimetral ]`: Diagnóstico holístico de postura de seguridad, modo stealth en WAN, segmentación estricta inter-VLAN, aislamiento Docker y portal openNDS 10.3 con veredicto de perímetro 100% bastionado.

### Controles de Navegación Rápida
Cada reporte de acción cuenta con botones táctiles al pie:
- `[ 🔄 Refrescar ]`: Ejecuta de nuevo la consulta en tiempo real sin salir de la pantalla actual.
- `[ 🔙 Volver al Submenú ]`: Regresa al submenú del subsistema consultado.
- `[ 🏠 Menú Principal ]`: Vuelve directamente a la raíz del panel de control.

---

## 3. Seguridad y Control de Acceso
* **Filtro de Lista Blanca:** Solo los identificadores numéricos autorizados (`1307787776`, `8266932918`) o el usuario `@admin` son procesados.
* **Resiliencia ante Errores:** En caso de caída de un servicio o excepción de ejecución, el nodo `Manejador Global de Errores` captura el evento y te envía una notificación de alerta a tu chat privado.
