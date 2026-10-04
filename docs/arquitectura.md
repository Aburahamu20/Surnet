# Arquitectura — Surnet

## Capas de la red

```
Capa 7 — Aplicación
  └── Servidores en VLAN 99 (Servicios): DHCP, DNS, WEB

Capa 3 — Red (distribución)
  ├── SW1 / SW2 — Oficina  (HSRP activo/standby, OSPF, SVIs)
  └── SW5 / SW6 — Bodega   (HSRP activo/standby, OSPF, SVIs)

Capa 3 — Red (core)
  ├── R1 / R2  — routers core Oficina (OSPF área 0)
  └── R3 / R4  — routers core Bodega  (OSPF área 0)

Capa 3 — Borde / Seguridad
  ├── FW1 — Firewall Oficina (ZFW, NAT/PAT, IPsec)
  └── FW2 — Firewall Bodega  (ZFW, NAT/PAT, IPsec)

Capa WAN
  └── ISP simulado  (200.1.1.x)
       ├── FW1  200.1.1.6/30
       └── FW2  200.1.1.2/30
```

---

## OSPF Multi-Área

| Área | Dispositivos | Redes |
|:---|:---|:---|
| Área 0 (backbone Oficina) | R1, R2, SW1, SW2 | 192.168.10.0/24 (enlaces P2P) |
| Área 1 (LAN Oficina) | SW1, SW2 | 192.168.1.0/24 (todas las VLANs) |
| Área 0 (backbone Bodega) | R3, R4, SW5, SW6 | 192.168.20.0/24 (enlaces P2P) |
| Área 2 (LAN Bodega) | SW5, SW6 | 192.168.2.0/24 (todas las VLANs) |

Las redes de ambas sedes **no forman un área 0 común** — cada sede tiene su propio proceso OSPF (proceso 1 y proceso 2). La comunicación entre sedes ocurre a través del túnel IPsec, con rutas estáticas redistribuidas en OSPF.

---

## HSRP

| Sede | HSRP Activo | HSRP Standby | Priority activo | Priority standby |
|:---|:---|:---|:---|:---|
| Oficina | SW1 | SW2 | 110 | 100 |
| Bodega | SW5 | SW6 | 110 | 100 |

SW1 y SW5 tienen `standby X preempt` — recuperan el rol activo automáticamente al volver en línea.

---

## VPN IPsec Site-to-Site

```
[LAN Oficina] → [FW1 200.1.1.6] ═══IPsec═══ [FW2 200.1.1.2] → [LAN Bodega]
```

| Parámetro | Valor |
|:---|:---|
| IKE Policy | 10 |
| Encriptación Phase 1 | AES-256 |
| Autenticación | Pre-shared key |
| DH Group | 14 |
| Pre-shared key | `Surnet@Ipsec24` |
| Transform-set | `esp-aes 256 esp-sha-hmac` |
| Modo | Tunnel |
| Crypto map FW1 | `VPN-BODEGA` |
| Crypto map FW2 | `VPN-OFICINA` |

El tráfico interesante (ACL 100) cubre `192.168.1.0/24 ↔ 192.168.2.0/24` y las redes de infraestructura (`192.168.10.0/24` y `192.168.20.0/24`).

---

## Zone-Based Firewall

Configurado en FW1 y FW2. Dos zonas por dispositivo:

- **LAN** — interfaces hacia R1/R2 (o R3/R4)
- **WAN** — interfaz hacia ISP

| Zone-pair | Política |
|:---|:---|
| LAN → WAN | inspect TCP, UDP, ICMP |
| WAN → LAN | pass solo tráfico VPN (ACL 101); drop el resto |

---

## NAT/PAT

ACL `NAT-LAN` en cada firewall:
- **Deny** el tráfico entre sedes (no natea tráfico VPN)
- **Permit** el resto hacia internet
- `ip nat inside source list NAT-LAN interface <WAN> overload`

---

## Direccionamiento de infraestructura (enlaces P2P)

### Oficina

| Enlace | Red | FW1/R1 | R1/R2/SW1/SW2 |
|:---|:---|:---|:---|
| FW1 ↔ R1 | 192.168.10.0/30 | .1 | .2 |
| FW1 ↔ R2 | 192.168.10.4/30 | .5 | .6 |
| R1 ↔ SW1 | 192.168.10.16/30 | .17 | .18 |
| R1 ↔ SW2 | 192.168.10.20/30 | .21 | .22 |
| R2 ↔ SW1 | 192.168.10.28/30 | .29 | .30 |
| R2 ↔ SW2 | 192.168.10.24/30 | .25 | .26 |
| SW1 ↔ SW2 (Po1) | 192.168.10.32/30 | .33 | .34 |

### Bodega

| Enlace | Red | FW2/R3 | R3/R4/SW5/SW6 |
|:---|:---|:---|:---|
| FW2 ↔ R3 | 192.168.20.0/30 | .1 | .2 |
| FW2 ↔ R4 | 192.168.20.4/30 | .5 | .6 |
| R3 ↔ SW5 | 192.168.20.8/30 | .9 | .10 |
| R3 ↔ SW6 | 192.168.20.12/30 | .13 | .14 |
| R4 ↔ SW5 | 192.168.20.16/30 | .17 | .18 |
| R4 ↔ SW6 | 192.168.20.20/30 | .21 | .22 |
| SW5 ↔ SW6 (Po1) | 192.168.20.24/30 | .25 | .26 |
