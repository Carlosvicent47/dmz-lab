# Informe de configuración de DMZ con Cisco Packet Tracer


### 1. Objetivo del laboratorio

Se pretende realizar un laboratorio con 3 subredes:
    * Red Interna
    * Zona demilitalizada
    * Red externa (Simula la conexión a internet)

En este entorno se podrá simular un escenario real de una organización que necesita un servidor web accesible desde internet sin comprometer la seguridad de la red interna. Para ello se realizará una zona desmilitarizada segura utilizando un router Cisco y aplicando reglas NAT y ACLs, para controlar el tráfico de las redes.

La zona desmilitarizada permite conexión externa, a través de un enmascaramiento de IP, pero no permite ningún tipo de comunicación desde el servidor a la red interna. Pero sí que permite conexión de la red interna al servidor.


### 2. Topologí­a implementada

- Cantidad de redes: 3
- Dispositivos usados: 7 nodos, 4 díspositivos de red y 3 dispositivos finales.
- Breve descripción de la función de cada zona (LAN, DMZ, Externa).



### 3. Plan de direccionamiento IP

| Dispositivo             | IP               | MÃ¡scara          | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             | 192.168.1.10     | 255.255.255.0     | 192.168.1.1       |
| Server_DMZ              | 192.168.2.10     | 255.255.255.0     | 192.168.2.1       |
| PC_External             | 192.168.3.10     | 255.255.255.0     | 192.168.3.1       |
| Router_FW Gi0/0 (LAN)   | 192.168.1.1      | 255.255.255.0     | -----------       |
| Router_FW Gi0/1 (DMZ)   | 192.168.2.1      | 255.255.255.0     | -----------       |
| Router_FW Gi0/2 (Ext)   | 192.168.3.1      | 255.255.255.0     | -----------       |


### 4. Configuración aplicada (resumen)

no access-list 101
ip access-list extended 101
10 permit icmp any any
20 permit tcp any host 192.168.3.1 eq 80
30 permit tcp any host 192.168.3.1 eq 443
exit

ip access-list extended 100
10 permit icmp any 192.168.1.0 0.0.0.255 echo-reply
20 permit tcp any 192.168.1.0 0.0.0.255 established
30 permit tcp any 192.168.3.0 0.0.0.255 established
40 permit icmp any 192.168.3.0 0.0.0.255 echo-reply
50 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
60 permit ip any any
exit

interface GigabitEthernet0/1
ip access-group 100 in
exit

interface GigabitEthernet0/2
ip access-group 101 in
exit



### 5. Verificaciones realizadas

* Desde PC_External (Web Browser):
    - Accede al servidor web DMZ (192.168.3.1). Resultado esperado: La página web debe cargar.
* Desde PC_External (Command Prompt):
    - ping 192.168.3.1 (Ping a la interfaz WAN/IP pública del servidor). Resultado esperado: Request timed out (Debe FALLAR si tu ACL externa bloquea ICMP).
* Desde PC_Internal (Web Browser):
    - Accede al servidor web DMZ (192.168.2.10). Resultado esperado: La página web debe cargar.
* Desde Server-PT Web_DMZ (Command Prompt):
    - ping 192.168.1.10 (Ping a PC_Internal). Resultado esperado: Request timed out (Debe FALLAR - ¡Esto es seguridad crucial!).


### 6. Conclusiones y recomendaciones

Aprendi a aplicar NAT y ACLs en un entorno simulado. Recomiendo verificar conectividad básica antes de aplicar reglas de firewall, ya que un error en la IP puede bloquear todo.