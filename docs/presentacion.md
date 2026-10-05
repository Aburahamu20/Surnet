# Presentación — PROYECTO SURNET

**Instituto Duoc UC · Administración de Redes y Telecomunicaciones**  
**Portafolio de Título 2026**

---

## Contenido de las Diapositivas

### Diapositiva 1 — Portada
Instituto Duoc UC · Administración de Redes y Telecomunicaciones · Portafolio de Título

### Diapositiva 2 — Problema y situación a evaluar
Empresa de logística que operaba todas sus actividades en una sola instalación. Al expandirse a dos sedes surge la necesidad de interconexión segura.

### Diapositiva 3 — Sedes de la empresa

| Sede | Ubicación | Función |
|---|---|---|
| **Oficina** | Huérfanos 869, Piso 5, Ofi. 502, Santiago Centro | Administración |
| **Bodega** | Av. Pedro Fontova 7455, Huechuraba | Logística / Operaciones |

### Diapositiva 4 — Topología anterior
Red desactualizada y simple: sin rutas alternas entre enlaces, sin segmentación de red, sin servicios de servidor.

### Diapositiva 5 — (Imagen: topología antigua)

### Diapositiva 6 — Objetivos

**Objetivos Generales:**
- Implementar una red moderna y segura
- Conectar enlace entre bodega y administración
- Integrar servicios en la nube

**Objetivos Específicos:**
- Diseño de topología de red en ambas sucursales
- Implementación de tecnología IoT y de gestión de inventario
- Aplicar parámetros de seguridad
- Configuración de servicios: DHCP, DNS y WEB

### Diapositiva 7 — Metodologías
Metodología híbrida Cascada + Kanban (ver [informe-proyecto.md](informe-proyecto.md) sección 3).

### Diapositiva 8 — Planos de bodega y oficina
*(Imagen: planos físicos de ambas sedes)*

### Diapositiva 9 — Mapa de Calor — Cobertura WiFi en Bodega
*(Imagen: heatmap de señal Wi-Fi, validando cobertura para terminales handheld)*

### Diapositiva 10 — (Imagen complementaria)

### Diapositiva 11 — Tecnologías a utilizar

| Equipo | Modelo | Rol |
|---|---|---|
| Switch L3 PoE | Cisco Catalyst C1300-48P-4G | Distribución |
| Switch PoE acceso | Cisco Catalyst C1000-24P-4X-L | Acceso PoE |
| Switch acceso | Cisco Catalyst C1300-24T-4G | Acceso |
| Servidor | Ubuntu Linux | Servicios (DHCP/DNS/WEB) |
| AP | Huawei eKit AP362E | Cobertura Wi-Fi |
| WLC | Huawei AC650-128AP | Gestión centralizada APs |

### Diapositiva 12 — Tecnologías IoT

| Equipo | Modelo | Función |
|---|---|---|
| WLC | Huawei AC650-128AP | Controlador inalámbrico |
| AP | Huawei eKit AP362E | Puntos de acceso |
| Cámara IP | Hikvision DS-2CD2047G3-LI2UY | Vigilancia |
| Control de acceso | Anviz VF30 Pro | Gestión de acceso biométrico |

### Diapositiva 13 — Diagrama lógico y segmentación
*(Imágenes: diagramas lógicos ANTES y DESPUÉS de la implementación)*

### Diapositiva 14 — Decisiones de diseño

> **Alta disponibilidad por sede:**  
> Oficina = sede crítica (ventas, gerencia) → requiere alta disponibilidad (HSRP, LACP, redundancia).  
> Bodega tolera mayor downtime → costo/beneficio favorece firewall único.

> **OSPF independiente por sede:**  
> OSPF por sede + rutas estáticas inter-sede hacia la VPN. Evita fuga de rutas internas entre sedes; simplifica troubleshooting; cada sede mantiene su propio dominio de routing.

### Diapositiva 15 — Subneteo VLSM por sede
*(Ver tabla completa en [vlans-y-enrutamiento.md](vlans-y-enrutamiento.md))*

### Diapositiva 16 — Topología ANTES
*(Imagen: red antigua, plana, sin redundancia)*

### Diapositiva 17 — Topología DESPUÉS
*(Imagen: nueva topología con HSRP, LACP, OSPF multi-área, VPN IPsec, ZFW, segmentación por VLANs)*

### Diapositiva 18 — Análisis de Factibilidad Financiera
*(Ver detalles completos en [flujo-de-caja.md](flujo-de-caja.md))*

- Financiamiento: $10M capital propio + $50M banco
- Cobro al cliente: $48M en Mes 3 + $48M en Mes 6
- Período de recuperación: Mes 6

### Diapositiva 19 — Inversión en Infraestructura por Categoría
*(Gráfico de inversión por categoría — ver tabla en [flujo-de-caja.md](flujo-de-caja.md))*

### Diapositiva 20 — Rentabilidad del Proyecto: VAN y TIR
*(Gráfico — VAN positivo, TIR > tasa exigida 0,8% mensual)*

### Diapositiva 21 — Ejemplo práctico — Herramientas
- **GNS3:** Simulación de dispositivos Cisco (vios / vios_l2) — configuraciones completas en `/gns3/`
- **Packet Tracer:** Validación alternativa con switches 3560/2960 + WLC-3504 — ver `/packet-tracer/`

### Diapositiva 22 — (Imagen/demo)

### Diapositiva 23 — Cierre
> ¿Tienen alguna pregunta?  
> **Gracias** — Presentation 2026
