# Decisiones Técnicas — Surnet

## 1. ¿Por qué OSPF multi-área y no RIP o EIGRP?

RIP tiene límite de 15 saltos y convergencia lenta. EIGRP es propietario de Cisco. OSPF es estándar abierto (RFC 2328), escala bien y la división en áreas reduce el tráfico de LSA en redes grandes. El área 0 backbone centraliza el routing entre zonas.

## 2. ¿Por qué HSRP y no VRRP?

HSRP es el protocolo de gateway redundante nativo de Cisco y está soportado en vios_l2 sin configuración adicional. VRRP requeriría ajustes de compatibilidad. El resultado funcional es equivalente.

## 3. ¿Por qué un solo firewall por sede y no dos en HA?

En GNS3 se usaron routers IOS como firewalls (ZFW) porque las imágenes de Fortigate o ASA no estaban disponibles. Un solo firewall es suficiente para demostrar ZFW, NAT y VPN. Un segundo firewall en HA agregaría complejidad sin cambiar el concepto demostrado.

## 4. ¿Por qué los switches de distribución no se conectan directamente al firewall?

Los routers core (R1/R2, R3/R4) actúan como capa intermedia entre la distribución y el firewall. Esto permite que OSPF área 0 se mantenga en los routers y que el firewall solo maneje el perímetro. Conectar directamente los switches de distribución al firewall fusionaría la segmentación de red con la seguridad perimetral.

## 5. ¿Por qué IPsec site-to-site y no GRE tunnel o MPLS?

IPsec cifra el tráfico entre sedes sobre el ISP simulado, lo cual es el estándar en redes empresariales reales. GRE sin IPsec no cifra. MPLS requiere infraestructura de proveedor. IPsec IKEv1 con AES-256 demuestra seguridad real en un entorno de simulación.

## 6. ¿Por qué no se implementó IPv6?

El proyecto se enfocó en dominar los conceptos de red empresarial IPv4 (VLANs, OSPF, HSRP, VPN, ZFW) dentro del tiempo disponible. Agregar IPv4 e IPv6 simultáneamente habría requerido doble configuración en cada dispositivo y doble verificación, sin aportar valor adicional al aprendizaje en esta etapa. IPv6 se implementaría en una fase posterior con dual-stack.

## 7. ¿Por qué PVST+ y no RSTP o MSTP?

PVST+ permite asignar un root bridge distinto por VLAN, lo que facilita balancear tráfico entre SW1 y SW5 (root primary de sus respectivas sedes). RSTP es más rápido en convergencia pero no cambia el comportamiento observable en una demo. MSTP sería ideal en producción con muchas VLANs.

## 8. ACL BLOCK-VISITAS — justificación

La VLAN 50 (Visitas) es una red de acceso público. Sin ACL, los visitantes podrían alcanzar servidores internos (VLAN 99) o la red de Bodega. La ACL se aplica **inbound en la SVI Vlan50** de SW1 y SW2 para bloquear el tráfico en la capa más cercana al origen, antes de que sea enrutado.
