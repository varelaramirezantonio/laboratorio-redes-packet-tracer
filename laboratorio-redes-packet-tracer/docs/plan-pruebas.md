# Plan de pruebas

| ID | Prueba | Resultado esperado | Estado |
|---|---|---|---|
| TC-01 | DHCP en los 4 PCs | Reciben IP de su VLAN | OK |
| TC-02 | VENTAS → SRV-INTERNO (192.168.30.10) | Responde | OK (el primer paquete se pierde por ARP) |
| TC-03 | VENTAS → Internet (8.8.8.8) | Responde con NAT | OK |
| TC-04 | VENTAS → IT (192.168.20.1) | Bloqueado | OK, "Destination host unreachable" |
| TC-05 | INVITADO → SRV-INTERNO | Bloqueado | OK |
| TC-06 | INVITADO → Internet | Responde | OK, 4/4 |
| TC-07 | IT → SRV-INTERNO | Responde | OK, 4/4 |
| TC-08 | IT → VENTAS | Responde | OK, 4/4 |
| TC-09 | PC-VENTAS2 → 8.8.8.8 (atraviesa Po1) | Responde | OK, 4/4 |
| TC-10 | SSH desde IT a R1, SW1, SW2 | Banner y prompt | OK |
| TC-11 | Telnet a 192.168.99.1 | Rechazado | OK |
| TC-12 | SSH desde VENTAS a 192.168.10.1 | Rechazado | OK, "Connection refused" |
| TC-13 | Port security: conectar un equipo con otra MAC | Puerto en err-disable | OK: puerto caído y ping sin respuesta |
| TC-14 | BPDU guard: unir dos puertos de acceso entre switches | Puertos en err-disable | OK: puertos caídos en ambos extremos |
| TC-15 | EtherChannel: quitar un enlace del canal | Tráfico sigue por el otro enlace | OK: ping 4/4 con un solo enlace |

## Verificaciones de configuración

- `show etherchannel summary` → `Po1(SU)` con Fa0/23(P) y Fa0/24(P)
- `show vlan brief` → puertos sin uso en VLAN 999
- `show port-security` → MAC sticky aprendidas
- `show access-lists` → contadores incrementados en R1

Añade las capturas de cada prueba en `evidencias/` con el nombre `TC-XX.png`.
