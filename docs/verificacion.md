# Comandos de Verificación Rápida — Surnet (GNS3)

## Conectividad general (routers y firewalls)

```cisco
show ip interface brief       ! estado de todas las interfaces
show ip route                 ! tabla de rutas completa
```

## OSPF (R1, R2, R3, R4, SW1, SW2, SW5, SW6)

```cisco
show ip ospf neighbor         ! vecinos activos (estado FULL = OK)
```

## HSRP (SW1, SW2, SW5, SW6)

```cisco
show standby brief            ! Active/Standby por VLAN y IP virtual
```

## VPN IPsec (FW1 y FW2)

```cisco
show crypto isakmp sa         ! Phase 1 (QM_IDLE = activo)
show crypto ipsec sa          ! Phase 2 + paquetes encriptados
```

## NAT (FW1 y FW2)

```cisco
show ip nat translations      ! traducciones activas hacia internet
show ip nat statistics        ! hits y misses del NAT
```

## Spanning-Tree (SW1, SW2, SW5, SW6)

```cisco
show spanning-tree summary    ! SW1/SW5 deben ser root; puertos BLK = normal
```

## ACL Visitas (SW1 o SW2)

```cisco
show ip access-lists BLOCK-VISITAS   ! contadores de deny suben si hay tráfico bloqueado
```

## VLANs (SW3, SW4, SW7, SW8)

```cisco
show vlan brief               ! VLANs creadas y puertos asignados
```

## EtherChannel / LACP (SW1, SW2, SW5, SW6)

```cisco
show etherchannel summary     ! estado del port-channel (SU = activo)
```

## Pings de prueba (desde las PCs)

```
ping 192.168.1.1              ! gateway Oficina
ping 192.168.2.1              ! gateway Bodega
ping 8.8.8.8                  ! internet simulado
ping 192.168.2.82             ! ping entre sedes (Oficina -> Bodega)

! Desde VLAN 50 (Visitas):
ping 192.168.1.82             ! debe FALLAR — bloqueado por BLOCK-VISITAS
ping 8.8.8.8                  ! debe PASAR  — internet permitido
```
