# Direccionamiento y VLANs

## VLANs

| VLAN | Nombre | Red | Gateway (R1) | Uso |
|---|---|---|---|---|
| 10 | VENTAS | 192.168.10.0/24 | 192.168.10.1 | Equipos de ventas |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 | Administración |
| 30 | SERVIDORES | 192.168.30.0/24 | 192.168.30.1 | Servidor interno |
| 50 | INVITADOS | 192.168.50.0/24 | 192.168.50.1 | Red de invitados |
| 99 | GESTION | 192.168.99.0/24 | 192.168.99.1 | Gestión de equipos |
| 999 | NATIVA-SIN-USO | — | — | VLAN nativa y puertos apagados |

## Dispositivos

| Dispositivo | Interfaz | IP | Gateway |
|---|---|---|---|
| ISP | Gi0/0 | 203.0.113.1/30 | — |
| ISP | Gi0/1 | 8.8.8.1/24 | — |
| SRV-INTERNET | Fa0 | 8.8.8.8/24 | 8.8.8.1 |
| R1 | Gi0/1 | 203.0.113.2/30 | 203.0.113.1 (ruta por defecto) |
| R1 | Gi0/0.10/.20/.30/.50/.99 | 192.168.X.1/24 | — |
| SW1 | SVI VLAN 99 | 192.168.99.2/24 | 192.168.99.1 |
| SW2 | SVI VLAN 99 | 192.168.99.3/24 | 192.168.99.1 |
| SRV-INTERNO | Fa0 | 192.168.30.10/24 | 192.168.30.1 |
| PCs | Fa0 | DHCP (.21 en adelante) | VLAN correspondiente |

DHCP en R1 para VENTAS, IT e INVITADOS: excluidas .1–.20, DNS 8.8.8.8.

## Cableado

| Origen | Destino |
|---|---|
| SRV-INTERNET Fa0 | ISP Gi0/1 |
| ISP Gi0/0 | R1 Gi0/1 |
| R1 Gi0/0 | SW1 Gi0/1 (trunk) |
| SW1 Fa0/23, Fa0/24 | SW2 Fa0/23, Fa0/24 (Po1, LACP) |
| SW1 Fa0/1 | SRV-INTERNO (VLAN 30) |
| SW1 Fa0/2 | PC-VENTAS1 (VLAN 10) |
| SW2 Fa0/1 | PC-VENTAS2 (VLAN 10) |
| SW2 Fa0/2 | PC-IT (VLAN 20) |
| SW2 Fa0/3 | PC-INVITADO (VLAN 50) |

Trunks: VLAN nativa 999, permitidas 10,20,30,50,99.

## ACL

- **VENTAS-IN** (en Gi0/0.10): permite respuestas ICMP y tráfico TCP establecido hacia IT; bloquea VENTAS hacia IT, GESTION e INVITADOS; permite el resto.
- **INVITADOS-IN** (en Gi0/0.50): bloquea INVITADOS hacia cualquier red 192.168.0.0/16; permite Internet.
- **SSH-ADMIN** (en VTY): solo 192.168.20.0/24 y 192.168.99.0/24.
