# Cómo colaborar en Surnet

Gracias por contribuir. Esta guía explica cómo proponer cambios y mantener el repositorio ordenado.

## Ramas

- `main` — configuraciones verificadas y funcionales
- `feature/<dispositivo>` — cambios en un dispositivo específico (ej. `feature/fw1-acl`)
- `fix/<descripcion>` — corrección de errores de configuración

## Proceso

1. Crea una rama desde `main`.
2. Realiza los cambios en los archivos correspondientes dentro de `gns3/` o `docs/`.
3. Prueba los comandos en GNS3 o Packet Tracer antes de subir.
4. Abre un Pull Request describiendo qué cambiaste y por qué.
5. Espera revisión antes de hacer merge.

## Convenciones de archivos

- Los archivos de configuración van en `gns3/oficina/` o `gns3/bodega/` según la sede.
- El nombre del archivo debe coincidir con el hostname del dispositivo (ej. `SW1.txt`).
- Los comandos deben incluir comentarios `!` explicando cada bloque.
- La documentación va en `docs/` en formato Markdown.

## Qué no subir

- Contraseñas reales o claves de producción.
- Archivos `.gns3` con rutas absolutas de tu equipo.
- Configuraciones sin probar.

## Estilo de commits

```
tipo(alcance): descripción breve

Ejemplos:
feat(sw1): agrega ACL BLOCK-VISITAS en Vlan50
fix(fw1): corrige peer address de VPN hacia FW2
docs(arquitectura): actualiza diagrama de topología
```
