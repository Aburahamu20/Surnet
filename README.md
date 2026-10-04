<div align="center">

# Surnet — Red Corporativa Dual-Sede

## Diseño e implementación de infraestructura de red empresarial

### *Dos sedes, una red. Segura, redundante y escalable.*

![Badge](https://img.shields.io/badge/Simulador-GNS3%20%2B%20Packet%20Tracer-28a745?style=for-the-badge)
![Badge](https://img.shields.io/badge/Routing-OSPF%20Multi--Área-0078D4?style=for-the-badge)
![Badge](https://img.shields.io/badge/VPN-IPsec%20IKEv1-f0ad4e?style=for-the-badge)
![Badge](https://img.shields.io/badge/Redundancia-HSRP%20%2B%20LACP-6C63FF?style=for-the-badge)
![Badge](https://img.shields.io/badge/Seguridad-ZFW%20%2B%20ACL-DC143C?style=for-the-badge)

| Campo | Detalle |
|:---|:---|
| **Institución** | Instituto Profesional Duoc UC |
| **Proyecto** | Surnet — Red Corporativa Dual-Sede |
| **Sedes** | Oficina (`192.168.1.0/24`) y Bodega (`192.168.2.0/24`) |
| **Simuladores** | GNS3 (vios / vios_l2) y Cisco Packet Tracer |
| **Protocolos** | OSPF multi-área, HSRP, LACP/EtherChannel, IPsec, NAT/PAT, PVST+ |
| **Seguridad** | Zone-Based Firewall, ACL BLOCK-VISITAS, SSH v2, banner MOTD |
| **Integrante** | Abraham Castro Romero |
| **Fecha** | Octubre 2026 |

</div>

---

## 📋 Contenido

- [1. Descripción general](#1-descripción-general)
- [2. Topología](#2-topología)
- [3. VLANs y direccionamiento](#3-vlans-y-direccionamiento)
- [4. Protocolos y decisiones clave](#4-protocolos-y-decisiones-clave)
- [5. Seguridad](#5-seguridad)
- [6. GNS3 vs Packet Tracer](#6-gns3-vs-packet-tracer)
- [7. Estructura del repositorio](#7-estructura-del-repositorio)
- [8. Verificación rápida](#8-verificación-rápida)

---

## 1. Descripción general

Surnet es una red corporativa diseñada para dos sucursales interconectadas mediante VPN IPsec. Cada sede cuenta con redundancia en capa de distribución (HSRP + LACP), enrutamiento dinámico OSPF multi-área y segmentación por VLANs. La simulación completa se realizó en GNS3 con imágenes IOS reales (`vios` y `vios_l2`), y en Cisco Packet Tracer con una topología equivalente simplificada que agrega conectividad inalámbrica mediante WLC-3504.

---

## 2. Topología

```
                    [ISP — 200.1.1.x]
                   /                 \
                [FW1]               [FW2]
               /     \             /     \
            [R1]     [R2]       [R3]     [R4]
             |   \  /   |       |   \  /   |
            [SW1 — Po1 — SW2] [SW5 — Po1 — SW6]
             |          |       |          |
          [SW3]       [SW4]  [SW7]       [SW8]
```

**Sede Oficina:** `FW1` → `R1`/`R2` → `SW1`/`SW2` (distribución L3, HSRP) → `SW3`/`SW4` (acceso L2)

**Sede Bodega:** `FW2` → `R3`/`R4` → `SW5`/`SW6` (distribución L3, HSRP) → `SW7`/`SW8` (acceso L2)

**Interconexión:** VPN IPsec entre `FW1` (200.1.1.6) y `FW2` (200.1.1.2) pasando por el ISP simulado.

---

## 3. VLANs y direccionamiento

### Sede Oficina — 192.168.1.0/24

| VLAN | Nombre | Subred | Gateway HSRP | SW1 (activo) | SW2 (standby) |
|:---|:---|:---|:---|:---|:---|
| 10 | RRHH | 192.168.1.0/27 | 192.168.1.1 | .2 | .3 |
| 20 | Ventas | 192.168.1.32/27 | 192.168.1.33 | .34 | .35 |
| 30 | Administracion | 192.168.1.64/28 | 192.168.1.65 | .66 | .67 |
| 40 | Gerencia | 192.168.1.96/27 | 192.168.1.97 | .98 | .99 |
| 50 | Visitas | 192.168.1.128/27 | 192.168.1.129 | .130 | .131 |
| 99 | Servicios | 192.168.1.80/28 | 192.168.1.81 | .82 | .83 |

### Sede Bodega — 192.168.2.0/24

| VLAN | Nombre | Subred | Gateway HSRP | SW5 (activo) | SW6 (standby) |
|:---|:---|:---|:---|:---|:---|
| 60 | Operaciones | 192.168.2.0/28 | 192.168.2.1 | .2 | .3 |
| 30 | Administracion | 192.168.2.16/28 | 192.168.2.17 | .18 | .19 |
| 70 | Bodega | 192.168.2.32/28 | 192.168.2.33 | .34 | .35 |
| 99 | Servicios | 192.168.2.80/28 | 192.168.2.81 | .82 | .83 |

Tablas completas de rutas: [docs/vlans-y-enrutamiento.md](docs/vlans-y-enrutamiento.md)

---

## 4. Protocolos y decisiones clave

### OSPF Multi-Área

- **Proceso 1 — Oficina:** Área 0 (R1, R2, SW1, SW2) + Área 1 (SVIs de LAN Oficina)
- **Proceso 2 — Bodega:** Área 0 (R3, R4, SW5, SW6) + Área 2 (SVIs de LAN Bodega)
- Rutas estáticas hacia el túnel VPN redistribuidas en OSPF con `redistribute static subnets`
- Ruta default redistribuida con `default-information originate` desde los routers core

### HSRP

- **SW1** activo (priority 110) para VLANs 10, 20, 30, 40, 50, 99
- **SW2** standby (priority 100) — toma el control automáticamente si SW1 cae
- Mismo esquema en Bodega con SW5/SW6

### LACP / EtherChannel

| Port-channel | Dispositivos | Tipo | Uso |
|:---|:---|:---|:---|
| Po1 SW1↔SW2 | SW1 + SW2 | L3 | Inter-distribución Oficina |
| Po1 SW5↔SW6 | SW5 + SW6 | L3 | Inter-distribución Bodega |
| Po1 SW3↔SW4 | SW3 + SW4 | L2 trunk | Cross-link acceso Oficina |
| Po1 SW7↔SW8 | SW7 + SW8 | L2 trunk | Cross-link acceso Bodega |

### Spanning-Tree PVST+

- `SW1 root primary` / `SW2 root secondary` — VLANs 10,20,30,40,50,99
- `SW5 root primary` / `SW6 root secondary` — VLANs 30,60,70,99
- PortFast + BPDUGuard en todos los puertos de acceso

### VPN IPsec (FW1 ↔ FW2)

- **Phase 1:** AES-256, pre-shared key `Surnet@Ipsec24`, DH group 14
- **Phase 2:** `esp-aes 256 esp-sha-hmac`, modo tunnel
- Tráfico interesante: `192.168.1.0/24 ↔ 192.168.2.0/24` y redes de infraestructura
- Peer FW1: `200.1.1.6` | Peer FW2: `200.1.1.2`

Ver detalles completos: [docs/arquitectura.md](docs/arquitectura.md)

---

## 5. Seguridad

### Zone-Based Firewall (FW1 y FW2)

Dos zonas definidas en cada firewall — `LAN` y `WAN`:

- **LAN→WAN:** `inspect` TCP/UDP/ICMP (tráfico saliente permitido)
- **WAN→LAN:** solo tráfico VPN decifrado (controlado por ACL 101)
- Resto: `drop` por defecto

### ACL BLOCK-VISITAS

Aplicada **inbound en Vlan50** de SW1 y SW2. Impide que la VLAN de Visitas (`192.168.1.128/27`) alcance redes internas, pero permite salida a internet:

```cisco
ip access-list extended BLOCK-VISITAS
 deny ip 192.168.1.128 0.0.0.31 192.168.1.0 0.0.0.255
 deny ip 192.168.1.128 0.0.0.31 192.168.2.0 0.0.0.255
 permit ip any any
```

### Hardening aplicado a todos los dispositivos

- `ip ssh version 2` con RSA 2048 bits
- `service password-encryption` + `enable secret`
- `exec-timeout 5 0` en console y VTY
- `no ip domain-lookup`
- Banner MOTD de acceso restringido

---

## 6. GNS3 vs Packet Tracer

| Aspecto | GNS3 | Packet Tracer |
|:---|:---|:---|
| VLANs Oficina | 10,20,30,40,50,99 | 10,20,30,40,50,99 ✓ |
| VLANs Bodega | 30,60,70,99 | 30,60,70,99 ✓ |
| Subredes y gateways | Idénticos | Idénticos ✓ |
| OSPF multi-área | ✅ | ✅ |
| HSRP | ✅ | ✅ |
| LACP Port-channel | ✅ | ✅ |
| Spanning-Tree PVST+ | ✅ | ✅ |
| Borde Oficina | R1 + R2 + FW1 (ZFW+VPN+NAT) | Core-Oficina (router simple) |
| Borde Bodega | R3 + R4 + FW2 (ZFW+VPN+NAT) | Core-Bodega (router simple) |
| VPN IPsec | FW1 ↔ FW2 | Tránsito directo ISP |
| Wireless | No aplica | WLC-3504 + APs + laptops |
| ACL BLOCK-VISITAS | SW1 + SW2 | SW1 + SW2 ✓ |

**Conclusión:** Las VLANs, subredes y gateways son idénticos en ambas simulaciones. La diferencia está en la capa de borde: GNS3 representa una infraestructura real con firewalls y routers core redundantes; PT simplifica con un solo router de borde.

---

## 7. Estructura del repositorio

```
Surnet/
├── README.md
├── CONTRIBUTING.md
├── .gitignore
├── docs/
│   ├── README.md
│   ├── vlans-y-enrutamiento.md
│   ├── arquitectura.md
│   ├── decisiones-tecnicas.md
│   └── verificacion.md
├── gns3/
│   ├── README.md
│   ├── oficina/
│   │   ├── R1.txt
│   │   ├── R2.txt
│   │   ├── SW1.txt
│   │   ├── SW2.txt
│   │   ├── SW3.txt
│   │   ├── SW4.txt
│   │   └── FW1.txt
│   ├── bodega/
│   │   ├── R3.txt
│   │   ├── R4.txt
│   │   ├── SW5.txt
│   │   ├── SW6.txt
│   │   ├── SW7.txt
│   │   ├── SW8.txt
│   │   └── FW2.txt
│   └── isp/
│       └── ISP.txt
└── packet-tracer/
    └── README.md
```

---

## 8. Verificación rápida

Comandos esenciales para demostrar el funcionamiento en GNS3:

```cisco
show ip interface brief       ! estado de todas las interfaces
show ip route                 ! tabla de rutas completa
show ip ospf neighbor         ! vecinos OSPF (FULL = correcto)
show standby brief            ! HSRP activo/standby por VLAN
show crypto isakmp sa         ! VPN Phase 1 (QM_IDLE = activo)
show crypto ipsec sa          ! VPN Phase 2 + paquetes cifrados
show ip nat translations      ! traducciones NAT activas
show spanning-tree summary    ! SW1/SW5 deben aparecer como root
show ip access-lists BLOCK-VISITAS
show etherchannel summary     ! estado del port-channel
```

Lista completa con pings de prueba: [docs/verificacion.md](docs/verificacion.md)

---

<div align="center">

**DUOC UC · Redes · Surnet · Octubre 2026**

</div>
