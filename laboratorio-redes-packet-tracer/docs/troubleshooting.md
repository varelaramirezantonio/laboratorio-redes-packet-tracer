# Problemas encontrados y soluciones

## 1. EtherChannel en estado (SD)

**Síntoma:** `show etherchannel summary` mostraba `Po1(SD)` con puertos suspendidos. LACP indicaba "not enabled on remote port".

**Causa:** el canal se había creado sin `channel-protocol lacp` explícito y con dúplex distinto en los miembros.

**Solución:** en ambos switches, reconstruir el canal.

```
interface range fa0/23-24
 shutdown
 no channel-group 1
exit
no interface port-channel 1
interface range fa0/23-24
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30,50,99
 speed 100
 duplex full
 channel-protocol lacp
 channel-group 1 mode active
 no shutdown
exit
```

Resultado: `Po1(SU)` con Fa0/23(P) y Fa0/24(P).

## 2. Puerto del switch en err-disable por un cable mal conectado

**Síntoma:** un puerto Gi pasó a err-disable.

**Causa:** el cable entre switches se conectó a un puerto equivocado y BPDU guard lo apagó.

**Solución:** recablear al puerto correcto y reactivar con `shutdown` / `no shutdown`.

## 3. `crypto key generate rsa modulus 2048` rechazado

**Solución:** ejecutar `crypto key generate rsa` y responder `2048` cuando pregunte el tamaño.

## 4. Comandos rechazados en el router

**Causa:** la consola estaba en modo usuario.

**Solución:** ejecutar `enable` antes de pegar la configuración.

## 5. Primer ping perdido

Es normal: el primer paquete se pierde mientras se resuelve ARP.
