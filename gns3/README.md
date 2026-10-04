# GNS3 — Configuraciones por Dispositivo

Cada archivo contiene los comandos de configuración completos para aplicar en el dispositivo correspondiente desde el modo `configure terminal`.

## Sede Oficina

| Archivo | Dispositivo | Rol |
|:---|:---|:---|
| [oficina/R1.txt](oficina/R1.txt) | R1 | Router core (Cisco IOSv) |
| [oficina/R2.txt](oficina/R2.txt) | R2 | Router core (Cisco IOSv) |
| [oficina/SW1.txt](oficina/SW1.txt) | SW1 | Switch distribución L3 — HSRP activo (vios_l2) |
| [oficina/SW2.txt](oficina/SW2.txt) | SW2 | Switch distribución L3 — HSRP standby (vios_l2) |
| [oficina/SW3.txt](oficina/SW3.txt) | SW3 | Switch acceso L2 (vios_l2) |
| [oficina/SW4.txt](oficina/SW4.txt) | SW4 | Switch acceso L2 (vios_l2) |
| [oficina/FW1.txt](oficina/FW1.txt) | FW1 | Firewall perimetral — ZFW + NAT + IPsec (Cisco IOSv) |

## Sede Bodega

| Archivo | Dispositivo | Rol |
|:---|:---|:---|
| [bodega/R3.txt](bodega/R3.txt) | R3 | Router core (Cisco IOSv) |
| [bodega/R4.txt](bodega/R4.txt) | R4 | Router core (Cisco IOSv) |
| [bodega/SW5.txt](bodega/SW5.txt) | SW5 | Switch distribución L3 — HSRP activo (vios_l2) |
| [bodega/SW6.txt](bodega/SW6.txt) | SW6 | Switch distribución L3 — HSRP standby (vios_l2) |
| [bodega/SW7.txt](bodega/SW7.txt) | SW7 | Switch acceso L2 (vios_l2) |
| [bodega/SW8.txt](bodega/SW8.txt) | SW8 | Switch acceso L2 (vios_l2) |
| [bodega/FW2.txt](bodega/FW2.txt) | FW2 | Firewall perimetral — ZFW + NAT + IPsec (Cisco IOSv) |

## ISP

| Archivo | Dispositivo | Rol |
|:---|:---|:---|
| [isp/ISP.txt](isp/ISP.txt) | ISP | Router ISP simulado (Cisco IOSv) |

## Cómo usar

1. Abre GNS3 y conecta al dispositivo mediante SolarPuTTY.
2. Ingresa a modo de configuración: `enable` → `configure terminal`.
3. Copia y pega el contenido del archivo correspondiente.
4. Al terminar: `end` → `write memory`.
