# Packet Tracer — Surnet

La topología de Packet Tracer reproduce la misma segmentación de VLANs, subredes y gateways que GNS3, con una capa de borde simplificada.

## Diferencias respecto a GNS3

| Aspecto | GNS3 | Packet Tracer |
|:---|:---|:---|
| Borde Oficina | R1 + R2 + FW1 | Core-Oficina (un solo router) |
| Borde Bodega | R3 + R4 + FW2 | Core-Bodega (un solo router) |
| VPN IPsec | FW1 ↔ FW2 | Tránsito directo por ISP |
| ZFW y NAT | Configurados en FW1/FW2 | Configurados en Core routers |
| Wireless | No aplica | WLC-3504 + APs CAPWAP + laptops |

## Dispositivos en PT

### Oficina
- **SW1** — Cisco 3560 (distribución, HSRP activo)
- **SW2** — Cisco 3560 (distribución, HSRP standby)
- **SW3** — Cisco 2960 (acceso — también tiene el WLC en F0/2)
- **SW4** — Cisco 2960 (acceso)
- **Core-Oficina** — Router de borde con OSPF y NAT
- **DHCP Server** — Servidor con pools por VLAN (en VLAN 99)
- **WLC-3504** — Controlador inalámbrico CAPWAP
- **AP1, AP2** — Access Points gestionados por WLC

### Bodega
- **SW5** — Cisco 3560 (distribución, HSRP activo)
- **SW6** — Cisco 3560 (distribución, HSRP standby)
- **SW7** — Cisco 2960 (acceso)
- **SW8** — Cisco 2960 (acceso — también tiene el WLC en F0/2)
- **Core-Bodega** — Router de borde con OSPF y NAT
- **DHCP Server** — Servidor con pools por VLAN (en VLAN 99)
- **WLC-3504** — Controlador inalámbrico CAPWAP
- **AP Bodega** — Access Point gestionado por WLC

## Notas sobre wireless (WLC + CAPWAP)

- Los puertos de los APs deben ser **trunk con native VLAN 99**, no access.
- El WLC recibe IP por DHCP en VLAN 99 (pool VLAN99 en el servidor DHCP).
- Esperar 3-4 minutos tras abrir el archivo .pkt para que CAPWAP establezca la conexión.
- Si un AP no se registra, eliminarlo, guardar, cerrar y reabrir PT, y agregarlo nuevamente.

## Notas sobre DHCP en PT

- El servidor DHCP tiene un pool por VLAN (VLAN10, VLAN20, etc.) más un pool para VLAN99.
- **No configurar** un pool llamado `serverPool` para la misma subred de VLAN99 — genera conflicto y los dispositivos reciben IPs sin gateway.
- Los SVIs de SW1/SW2 tienen `ip helper-address` apuntando al servidor DHCP (192.168.1.81 en Oficina, 192.168.2.81 en Bodega).
