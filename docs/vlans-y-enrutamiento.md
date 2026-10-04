# Tablas de VLANs y Enrutamiento — Surnet

## VLANs — Oficina

| VLAN | Nombre | Subred | Máscara | Gateway HSRP | SW1 (activo) | SW2 (standby) |
|:---|:---|:---|:---|:---|:---|:---|
| 10 | RRHH | 192.168.1.0/27 | /27 | 192.168.1.1 | 192.168.1.2 | 192.168.1.3 |
| 20 | Ventas | 192.168.1.32/27 | /27 | 192.168.1.33 | 192.168.1.34 | 192.168.1.35 |
| 30 | Administracion | 192.168.1.64/28 | /28 | 192.168.1.65 | 192.168.1.66 | 192.168.1.67 |
| 40 | Gerencia | 192.168.1.96/27 | /27 | 192.168.1.97 | 192.168.1.98 | 192.168.1.99 |
| 50 | Visitas | 192.168.1.128/27 | /27 | 192.168.1.129 | 192.168.1.130 | 192.168.1.131 |
| 99 | Servicios | 192.168.1.80/28 | /28 | 192.168.1.81 | 192.168.1.82 | 192.168.1.83 |

> Coincide GNS3 y PT: mismas VLANs, mismas subredes, mismos gateways HSRP.

---

## VLANs — Bodega

| VLAN | Nombre | Subred | Máscara | Gateway HSRP | SW5 (activo) | SW6 (standby) |
|:---|:---|:---|:---|:---|:---|:---|
| 60 | Operaciones | 192.168.2.0/28 | /28 | 192.168.2.1 | 192.168.2.2 | 192.168.2.3 |
| 30 | Administracion | 192.168.2.16/28 | /28 | 192.168.2.17 | 192.168.2.18 | 192.168.2.19 |
| 70 | Bodega | 192.168.2.32/28 | /28 | 192.168.2.33 | 192.168.2.34 | 192.168.2.35 |
| 99 | Servicios | 192.168.2.80/28 | /28 | 192.168.2.81 | 192.168.2.82 | 192.168.2.83 |

> Coincide GNS3 y PT: mismas VLANs, mismas subredes, mismos gateways HSRP.

---

## Tabla de Enrutamiento — Oficina (GNS3)

### SW1 / SW2 (OSPF área 0 + área 1)

| Tipo | Red | Máscara | Vía / Interfaz |
|:---|:---|:---|:---|
| C | 192.168.1.0/27 | /27 | Vlan10 |
| C | 192.168.1.32/27 | /27 | Vlan20 |
| C | 192.168.1.64/28 | /28 | Vlan30 |
| C | 192.168.1.80/28 | /28 | Vlan99 |
| C | 192.168.1.96/27 | /27 | Vlan40 |
| C | 192.168.1.128/27 | /27 | Vlan50 |
| C | 192.168.10.16/30 | /30 | GigabitEthernet0/1 (hacia R1) |
| C | 192.168.10.28/30 | /30 | GigabitEthernet0/0 (hacia R2) |
| C | 192.168.10.32/30 | /30 | Port-channel1 (inter-dist SW1-SW2) |
| O | 192.168.10.0/30 | /30 | via R1 (FW1-R1 link) |
| O | 192.168.10.4/30 | /30 | via R2 (FW1-R2 link) |
| O | 192.168.10.20/30 | /30 | via R1 (SW2-R1 link) |
| O | 192.168.10.24/30 | /30 | via R2 (SW2-R2 link) |
| O | 192.168.2.0/24 | /24 | via R1 o R2 (ruta estática redistribuida) |
| O | 192.168.20.0/24 | /24 | via R1 o R2 (ruta estática redistribuida) |
| O*E2 | 0.0.0.0/0 | — | via R1 o R2 (default redistribuida) |

### R1 / R2 (OSPF área 0)

| Tipo | Red | Vía / Interfaz |
|:---|:---|:---|
| C | 192.168.10.0/30 | GigabitEthernet0/2 (hacia FW1) |
| C | 192.168.10.16/30 | GigabitEthernet0/1 (hacia SW1) |
| C | 192.168.10.20/30 | GigabitEthernet0/0 (hacia SW2) |
| O | 192.168.1.x (todas) | via SW1 / SW2 (OSPF área 1) |
| S | 0.0.0.0/0 | 192.168.10.1 (FW1) |
| S | 192.168.2.0/24 | 192.168.10.1 (via VPN en FW1) |
| S | 192.168.20.0/24 | 192.168.10.1 (via VPN en FW1) |

---

## Tabla de Enrutamiento — Oficina (Packet Tracer)

### Core-Oficina (conectado a SW1, SW2 y ISP)

| Tipo | Red | Vía / Interfaz |
|:---|:---|:---|
| C | enlace P2P SW1 | GigabitEthernet0/0 |
| C | enlace P2P SW2 | GigabitEthernet0/1 |
| C | enlace hacia ISP | Serial0/0/0 |
| O | 192.168.1.x (todas) | via SW1 / SW2 (OSPF) |
| S | 0.0.0.0/0 | ISP Serial |

> Arquitectura más simple — un solo router de borde en lugar de R1+R2+FW1.

---

## Tabla de Enrutamiento — Bodega (GNS3)

### SW5 / SW6 (OSPF área 0 + área 2)

| Tipo | Red | Máscara | Vía / Interfaz |
|:---|:---|:---|:---|
| C | 192.168.2.0/28 | /28 | Vlan60 |
| C | 192.168.2.16/28 | /28 | Vlan30 |
| C | 192.168.2.32/28 | /28 | Vlan70 |
| C | 192.168.2.80/28 | /28 | Vlan99 |
| C | 192.168.20.8/30 | /30 | GigabitEthernet0/0 (hacia R3) |
| C | 192.168.20.16/30 | /30 | GigabitEthernet0/1 (hacia R4) |
| C | 192.168.20.24/30 | /30 | Port-channel1 (inter-dist SW5-SW6) |
| O | 192.168.20.0/30 | /30 | via R3 (FW2-R3 link) |
| O | 192.168.20.4/30 | /30 | via R4 (FW2-R4 link) |
| O | 192.168.20.12/30 | /30 | via R3 (SW6-R3 link) |
| O | 192.168.20.20/30 | /30 | via R4 (SW6-R4 link) |
| O | 192.168.1.0/24 | /24 | via R3 o R4 (ruta estática redistribuida) |
| O | 192.168.10.0/24 | /24 | via R3 o R4 (ruta estática redistribuida) |
| O*E2 | 0.0.0.0/0 | — | via R3 o R4 (default redistribuida) |

### R3 / R4 (OSPF área 0)

| Tipo | Red | Vía / Interfaz |
|:---|:---|:---|
| C | 192.168.20.0/30 | GigabitEthernet0/2 (hacia FW2) |
| C | 192.168.20.8/30 | GigabitEthernet0/0 (hacia SW5) |
| C | 192.168.20.12/30 | GigabitEthernet0/1 (hacia SW6) |
| O | 192.168.2.x (todas) | via SW5 / SW6 (OSPF área 2) |
| S | 0.0.0.0/0 | 192.168.20.1 (FW2) |
| S | 192.168.1.0/24 | 192.168.20.1 (via VPN en FW2) |
| S | 192.168.10.0/24 | 192.168.20.1 (via VPN en FW2) |

---

## Tabla de Enrutamiento — Bodega (Packet Tracer)

### Core-Bodega (conectado a SW5, SW6 y ISP)

| Tipo | Red | Vía / Interfaz |
|:---|:---|:---|
| C | enlace P2P SW5 | GigabitEthernet0/0 |
| C | enlace P2P SW6 | GigabitEthernet0/1 |
| C | enlace hacia ISP | Serial0/0/1 |
| O | 192.168.2.x (todas) | via SW5 / SW6 (OSPF) |
| S | 0.0.0.0/0 | ISP Serial |

> Arquitectura más simple — un solo router de borde en lugar de R3+R4+FW2.

---

## Comparación GNS3 vs Packet Tracer

| Aspecto | GNS3 | Packet Tracer |
|:---|:---|:---|
| VLANs Oficina | 10,20,30,40,50,99 | 10,20,30,40,50,99 ✓ |
| VLANs Bodega | 30,60,70,99 | 30,60,70,99 ✓ |
| Subredes | 192.168.1.x / 192.168.2.x | 192.168.1.x / 192.168.2.x ✓ |
| Gateways HSRP | Mismas IPs virtuales | Mismas IPs virtuales ✓ |
| Protocolo routing | OSPF multi-área | OSPF multi-área ✓ |
| Borde Oficina | R1 + R2 + FW1 (ZFW+VPN) | Core-Oficina (router simple) |
| Borde Bodega | R3 + R4 + FW2 (ZFW+VPN) | Core-Bodega (router simple) |
| Salida internet | NAT/PAT en FW1 / FW2 | NAT en Core routers |
| VPN entre sedes | IPsec FW1 ↔ FW2 | Tránsito directo ISP |
| Spanning-tree | PVST+ con root SW1/SW5 | PVST+ con root SW1/SW5 ✓ |
| ACL Visitas | BLOCK-VISITAS en SW1/SW2 | BLOCK-VISITAS en SW1/SW2 ✓ |
