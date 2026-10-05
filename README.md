<div align="center">

# Surnet — Red Corporativa Dual-Sede

### *Dos sedes, una red. Segura, redundante y escalable.*

![Badge](https://img.shields.io/badge/Institución-Duoc%20UC-003087?style=for-the-badge)
![Badge](https://img.shields.io/badge/Simulador-GNS3%20%2B%20Packet%20Tracer-28a745?style=for-the-badge)
![Badge](https://img.shields.io/badge/Routing-OSPF%20Multi--Área-0078D4?style=for-the-badge)
![Badge](https://img.shields.io/badge/VPN-IPsec%20IKEv1-f0ad4e?style=for-the-badge)
![Badge](https://img.shields.io/badge/Redundancia-HSRP%20%2B%20LACP-6C63FF?style=for-the-badge)
![Badge](https://img.shields.io/badge/Seguridad-ZFW%20%2B%20ACL-DC143C?style=for-the-badge)

</div>

---

## Tabla de Contenidos

- [1. Descripción del Proyecto](#1-descripción-del-proyecto)
- [2. Equipo de Trabajo](#2-equipo-de-trabajo)
- [3. La Empresa](#3-la-empresa)
- [4. Topología de Red](#4-topología-de-red)
- [5. VLANs y Direccionamiento](#5-vlans-y-direccionamiento)
- [6. Hardware y Tecnologías](#6-hardware-y-tecnologías)
- [7. Protocolos y Arquitectura](#7-protocolos-y-arquitectura)
- [8. Seguridad](#8-seguridad)
- [9. Factibilidad Financiera](#9-factibilidad-financiera)
- [10. Metodología](#10-metodología)
- [11. GNS3 vs Packet Tracer](#11-gns3-vs-packet-tracer)
- [12. Estructura del Repositorio](#12-estructura-del-repositorio)
- [13. Verificación Rápida](#13-verificación-rápida)

---

## 1. Descripción del Proyecto

**Surnet** es el proyecto de titulación del equipo para el Portafolio de Título de *Técnico en Redes y Telecomunicaciones* en **Duoc UC**. Consiste en el diseño e implementación de una infraestructura de red corporativa para una empresa con dos sedes físicamente separadas en la Región Metropolitana.

La solución interconecta ambas sedes mediante una **VPN IPsec sitio a sitio**, provee **alta disponibilidad** con HSRP + LACP, segmenta el tráfico con **VLANs y OSPF multi-área**, y protege el perímetro con **Zone-Based Firewall y ACLs**. La simulación se realizó en GNS3 (IOS real) y en Cisco Packet Tracer (topología equivalente con WLC inalámbrico).

| Campo | Detalle |
|:---|:---|
| **Institución** | Instituto Profesional Duoc UC — Conectividad y Redes |
| **Carrera** | Técnico en Redes y Telecomunicaciones |
| **Proyecto** | Surnet — Red Corporativa Dual-Sede |
| **Sede Oficina** | Huérfanos 869, Piso 5, Dpto. 502, Santiago |
| **Sede Bodega** | Av. Pedro Fontova 7455, Huechuraba |
| **Simuladores** | GNS3 (vios / vios_l2) · Cisco Packet Tracer |
| **Protocolos** | OSPF Multi-área · HSRP · LACP · IPsec IKEv1 · NAT/PAT · PVST+ |
| **Seguridad** | Zone-Based Firewall · ACL BLOCK-VISITAS · SSH v2 |
| **Fecha** | Octubre 2026 |

---

## 2. Equipo de Trabajo

| Integrante | Rol | Horas |
|:---|:---|:---|
| Abraham Castro Romero | Técnico de Redes | 186,7 hrs |
| Sebastián Fuentes | Técnico de Redes | 186,7 hrs |
| Juan Silva | Técnico de Redes | 186,7 hrs |
| **Docente** | Juan Pierattini | — |

---

## 3. La Empresa

Empresa dedicada a la **comercialización y distribución de productos y útiles de aseo**. Ante el crecimiento de su demanda, decidió separar físicamente sus operaciones en dos sedes.

### Problemática

La separación física sumada al bajo nivel de digitalización generó:

- Proceso logístico 100% manual (órdenes en papel)
- Errores frecuentes en el proceso de picking
- Sin trazabilidad de pedidos en tiempo real
- Sin coordinación efectiva entre bodega y administración
- Sin conectividad segura entre sedes

### Solución Implementada

- **Red dual-sede** interconectada por VPN IPsec
- **Digitalización del picking** con terminales handheld y lectores de código de barras vía Wi-Fi
- **Segmentación VLAN** por área funcional en cada sede
- **Alta disponibilidad** con HSRP + LACP en distribución
- **Seguridad perimetral** con Fortinet FortiGate + ZFW

---

## 4. Topología de Red

```
                   [ISP — 200.1.1.x / 30]
                  /                      \
               [FW1]                   [FW2]
           (200.1.1.6)             (200.1.1.2)
              /      \               /      \
           [R1]      [R2]         [R3]      [R4]
            |    \  /   |          |    \  /   |
        [SW1 ——— Po1 ——— SW2]  [SW5 ——— Po1 ——— SW6]
        (HSRP activo/standby)  (HSRP activo/standby)
            |              |       |              |
         [SW3]          [SW4]  [SW7]          [SW8]
         (acc)          (acc)  (acc)          (acc)
```

**Sede Oficina:** `FW1` → `R1`/`R2` → `SW1`/`SW2` (distribución L3, HSRP, LACP) → `SW3`/`SW4` (acceso L2)

**Sede Bodega:** `FW2` → `R3`/`R4` → `SW5`/`SW6` (distribución L3, HSRP, LACP) → `SW7`/`SW8` (acceso L2)

**Interconexión:** Túnel VPN IPsec entre `FW1` (200.1.1.6) y `FW2` (200.1.1.2) a través del ISP simulado.

---

## 5. VLANs y Direccionamiento

### Sede Oficina — `192.168.1.0/24`

| VLAN | Nombre | Subred | Gateway HSRP | SW1 (activo) | SW2 (standby) |
|:---|:---|:---|:---|:---|:---|
| 10 | RRHH | 192.168.1.0/27 | 192.168.1.1 | .2 | .3 |
| 20 | Ventas | 192.168.1.32/27 | 192.168.1.33 | .34 | .35 |
| 30 | Administracion | 192.168.1.64/28 | 192.168.1.65 | .66 | .67 |
| 40 | Gerencia | 192.168.1.96/27 | 192.168.1.97 | .98 | .99 |
| 50 | Visitas | 192.168.1.128/27 | 192.168.1.129 | .130 | .131 |
| 99 | Servicios | 192.168.1.80/28 | 192.168.1.81 | .82 | .83 |

### Sede Bodega — `192.168.2.0/24`

| VLAN | Nombre | Subred | Gateway HSRP | SW5 (activo) | SW6 (standby) |
|:---|:---|:---|:---|:---|:---|
| 60 | Operaciones | 192.168.2.0/28 | 192.168.2.1 | .2 | .3 |
| 30 | Administracion | 192.168.2.16/28 | 192.168.2.17 | .18 | .19 |
| 70 | Bodega | 192.168.2.32/28 | 192.168.2.33 | .34 | .35 |
| 99 | Servicios | 192.168.2.80/28 | 192.168.2.81 | .82 | .83 |

Tablas de enrutamiento completas por dispositivo → [docs/vlans-y-enrutamiento.md](docs/vlans-y-enrutamiento.md)

---

## 6. Hardware y Tecnologías

### Equipos de Red

| Equipo | Modelo | Cantidad | Rol |
|:---|:---|:---|:---|
| Switch L3 PoE | Cisco CBS350 Managed 48-Port L3 | 6 | Distribución |
| Switch PoE | Cisco Catalyst C1000-24P-4X-L | 3 | Acceso PoE |
| Switch acceso | Cisco Catalyst C1300-24T-4G | 1 | Acceso |
| Firewall/Router | Fortinet FortiGate 60F (lic. 5 años) | 2 | Perímetro + VPN |
| WLC | Huawei AC650-128AP | 2 | Controlador Wi-Fi |
| Access Point | Huawei eKit AP362E | 16 | Cobertura inalámbrica |

### Tecnologías IoT y Seguridad Física

| Equipo | Modelo | Cantidad | Función |
|:---|:---|:---|:---|
| Cámara IP | Hikvision DS-2CD2047G3-LI2UY | 8 | Vigilancia |
| Control de acceso | Anviz VF30 Pro | 3 | Biometría |
| UPS | 1500VA | 2 | Respaldo energético |
| PDU | 19" | 3 | Distribución eléctrica |
| Rack | 22U-27U | 2 | Instalación de equipos |

### Conectividad ISP

| Proveedor | Plan |
|:---|:---|
| Movistar Empresas | Enlace Oficina |
| Entel Empresas | Enlace Bodega |

---

## 7. Protocolos y Arquitectura

### OSPF Multi-Área

| Proceso | Sede | Áreas |
|:---|:---|:---|
| Proceso 1 | Oficina | Área 0 (R1, R2, SW1, SW2) + Área 1 (SVIs LAN) |
| Proceso 2 | Bodega | Área 0 (R3, R4, SW5, SW6) + Área 2 (SVIs LAN) |

Rutas estáticas hacia el túnel VPN redistribuidas en OSPF. `default-information originate` en routers core para ruta por defecto. OSPF independiente por sede — evita fuga de rutas internas y simplifica el troubleshooting.

### HSRP

| Sede | Activo (priority 110) | Standby (priority 100) | VLANs |
|:---|:---|:---|:---|
| Oficina | SW1 | SW2 | 10, 20, 30, 40, 50, 99 |
| Bodega | SW5 | SW6 | 30, 60, 70, 99 |

### LACP / EtherChannel

| Port-channel | Dispositivos | Tipo | Función |
|:---|:---|:---|:---|
| Po1 SW1↔SW2 | SW1 + SW2 | L3 | Inter-distribución Oficina |
| Po1 SW5↔SW6 | SW5 + SW6 | L3 | Inter-distribución Bodega |
| Po1 SW3↔SW4 | SW3 + SW4 | L2 trunk | Cross-link acceso Oficina |
| Po1 SW7↔SW8 | SW7 + SW8 | L2 trunk | Cross-link acceso Bodega |

### Spanning-Tree PVST+

- `SW1 root primary` / `SW2 root secondary` — VLANs 10, 20, 30, 40, 50, 99
- `SW5 root primary` / `SW6 root secondary` — VLANs 30, 60, 70, 99
- PortFast + BPDUGuard habilitados en todos los puertos de acceso

### VPN IPsec (FW1 ↔ FW2)

```
Phase 1:  AES-256 · SHA-1 · DH group 14 · pre-share key: Surnet@Ipsec24
Phase 2:  esp-aes 256 · esp-sha-hmac · modo tunnel
Peer FW1: 200.1.1.6   ←→   Peer FW2: 200.1.1.2
Tráfico:  192.168.1.0/24  ↔  192.168.2.0/24
```

### NAT/PAT

Configurado en FW1 y FW2. ACL `NAT-LAN` excluye el tráfico inter-sede (evita nattear la VPN). El tráfico de salida a internet se traduce con la IP de la interfaz WAN.

Detalles completos → [docs/arquitectura.md](docs/arquitectura.md)

---

## 8. Seguridad

### Zone-Based Firewall (FW1 y FW2)

Dos zonas en cada firewall: `LAN` y `WAN`

| Dirección | Acción | Protocolo |
|:---|:---|:---|
| LAN → WAN | `inspect` | TCP, UDP, ICMP |
| WAN → LAN (VPN) | `pass` | ACL 101 (tráfico cifrado) |
| Resto | `drop` | — |

### ACL BLOCK-VISITAS

Aplicada **inbound en Vlan50** de SW1 y SW2. Bloquea que la red de visitas (`192.168.1.128/27`) acceda a redes internas:

```cisco
ip access-list extended BLOCK-VISITAS
 deny ip 192.168.1.128 0.0.0.31 192.168.1.0 0.0.0.255
 deny ip 192.168.1.128 0.0.0.31 192.168.2.0 0.0.0.255
 permit ip any any
```

### Hardening (todos los dispositivos)

- `ip ssh version 2` + RSA 2048 bits
- `service password-encryption` + `enable secret`
- `exec-timeout 5 0` en console y VTY
- `no ip domain-lookup`
- Banner MOTD de acceso restringido

---

## 9. Factibilidad Financiera

### Presupuesto por Categoría

| Categoría | Total (CLP) |
|:---|:---|
| Equipos activos (switches, APs, cámaras, control acceso) | ~$10.080.000 |
| Infraestructura pasiva (cableado, racks, UPS, PDU) | ~$5.743.990 |
| Seguridad / Firewall (FortiGate 60F × 2, lic. 5 años) | $4.400.000 |
| Conectividad ISP (Movistar + Entel) | $354.000 |
| Mano de obra (3 técnicos × 186,7 hrs) | $3.150.395 |
| Obras civiles (instalación infraestructura × 2) | $8.000.000 |
| **TOTAL ESTIMADO** | **~$31.728.385** |

### Flujo de Caja — Resumen 6 Meses

| Concepto | Valor |
|:---|:---|
| Capital propio aportado | $10.000.000 |
| Préstamo bancario (+ $8M interés) | $50.000.000 |
| Cobro al cliente (Mes 3 + Mes 6) | $48.000.000 × 2 |
| Inversión inicial (equipos + obras) | -$50.000.000 |
| Tasa de descuento mensual | 0,8% |
| Período de recuperación | Mes 6 |

> ✔ **VAN positivo** — El proyecto genera valor sobre el costo del capital.  
> ✔ **TIR > tasa exigida** — El proyecto es rentable.

Presupuesto detallado y flujo mes a mes → [docs/flujo-de-caja.md](docs/flujo-de-caja.md)

---

## 10. Metodología

Se adoptó una **metodología híbrida Cascada + Kanban**:

| Metodología | Aplicación |
|:---|:---|
| **Cascada** | Fases secuenciales: Análisis → Diseño → Ejecución → Pruebas → Entrega |
| **Kanban** | Gestión visual de tareas durante la fase de Ejecución (WIP limitado) |

### Fases del Proyecto

1. **Análisis** — Levantamiento de requerimientos, diagnóstico de infraestructura existente
2. **Diseño** — Topología, VLSM, VLANs, VPN, red inalámbrica, selección de equipos
3. **Ejecución** — Instalación, configuración e integración (gestionada con Kanban)
4. **Pruebas** — Conectividad, VPN, VLANs, cobertura Wi-Fi, terminales handheld
5. **Entrega** — Documentación completa, respaldo de configs, presentación formal

Informe completo → [docs/informe-proyecto.md](docs/informe-proyecto.md)  
Contenido de la presentación → [docs/presentacion.md](docs/presentacion.md)

---

## 11. GNS3 vs Packet Tracer

| Aspecto | GNS3 | Packet Tracer |
|:---|:---|:---|
| VLANs Oficina | 10, 20, 30, 40, 50, 99 | 10, 20, 30, 40, 50, 99 ✓ |
| VLANs Bodega | 30, 60, 70, 99 | 30, 60, 70, 99 ✓ |
| Subredes y gateways | Idénticos | Idénticos ✓ |
| OSPF multi-área | ✅ | ✅ |
| HSRP | ✅ | ✅ |
| LACP EtherChannel | ✅ | ✅ |
| Spanning-Tree PVST+ | ✅ | ✅ |
| Borde Oficina | R1 + R2 + FW1 (ZFW, VPN, NAT) | Core-Oficina (router simple) |
| Borde Bodega | R3 + R4 + FW2 (ZFW, VPN, NAT) | Core-Bodega (router simple) |
| VPN IPsec | FW1 ↔ FW2 | Tránsito directo ISP |
| Wireless | — | WLC-3504 + APs + laptops |
| ACL BLOCK-VISITAS | SW1 + SW2 | SW1 + SW2 ✓ |

Las VLANs, subredes y gateways son idénticos en ambas simulaciones. La diferencia está en la capa de borde: GNS3 representa la infraestructura real con firewalls y routers core redundantes; PT simplifica con un solo router de borde y agrega la capa inalámbrica.

---

## 12. Estructura del Repositorio

```
Surnet/
├── README.md                         ← Este archivo
├── CONTRIBUTING.md                   ← Guía para contribuir
├── .gitignore
│
├── docs/                             ← Documentación técnica y de proyecto
│   ├── README.md                     ← Índice de docs
│   ├── vlans-y-enrutamiento.md       ← Tablas VLAN, VLSM, rutas por dispositivo
│   ├── arquitectura.md               ← OSPF, VPN, ZFW, NAT, tablas P2P
│   ├── decisiones-tecnicas.md        ← 8 decisiones de diseño justificadas
│   ├── verificacion.md               ← Comandos de verificación
│   ├── informe-proyecto.md           ← Informe formal completo (Duoc UC)
│   ├── flujo-de-caja.md              ← Presupuesto, flujo de caja, VAN/TIR
│   └── presentacion.md              ← Contenido de las 23 diapositivas
│
├── gns3/                             ← Configuraciones GNS3 (IOS real)
│   ├── README.md
│   ├── oficina/
│   │   ├── R1.txt · R2.txt
│   │   ├── SW1.txt · SW2.txt · SW3.txt · SW4.txt
│   │   └── FW1.txt
│   ├── bodega/
│   │   ├── R3.txt · R4.txt
│   │   ├── SW5.txt · SW6.txt · SW7.txt · SW8.txt
│   │   └── FW2.txt
│   └── isp/
│       └── ISP.txt
│
└── packet-tracer/                    ← Notas sobre la topología PT
    └── README.md
```

---

## 13. Verificación Rápida

```cisco
show ip interface brief          ! estado de todas las interfaces
show ip route                    ! tabla de rutas completa
show ip ospf neighbor            ! vecinos OSPF (FULL = correcto)
show standby brief               ! HSRP activo/standby por VLAN
show crypto isakmp sa            ! VPN Phase 1 (QM_IDLE = activo)
show crypto ipsec sa             ! VPN Phase 2 + paquetes cifrados
show ip nat translations         ! traducciones NAT activas
show spanning-tree summary       ! SW1/SW5 deben aparecer como root
show ip access-lists BLOCK-VISITAS
show vlan brief                  ! VLANs activas por switch
show etherchannel summary        ! estado del port-channel (P = bundled)
```

Comandos completos con pings de prueba → [docs/verificacion.md](docs/verificacion.md)

---

<div align="center">

**Duoc UC · Conectividad y Redes · Portafolio de Título · Octubre 2026**

*Castro · Fuentes · Silva*

</div>
