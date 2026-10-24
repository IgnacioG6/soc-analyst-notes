# Comandos de red en Ubuntu

Práctica de diagnóstico de red desde la terminal de Ubuntu.

## ip a
- **Para qué sirve:** muestra las interfaces de red del sistema con su estado, direcciones IP, máscara de subred y dirección MAC. Reemplaza al antiguo `ifconfig`.
- **Qué mostró en mi equipo:** la interfaz wifi `wlo1` en estado UP, con IP privada `192.168.100.6/24` asignada dinámicamente (DHCP). También aparecieron `lo` (loopback, 127.0.0.1), interfaces virtuales de Docker (`docker0` y puentes `br-...`, en estado DOWN) y una interfaz de VPN (`ham0`).
- **Capa relacionada:** capa 3 (red, dirección IP) y capa 2 (enlace de datos, dirección MAC).
- **Utilidad en un SOC:** identificar qué interfaces y redes tiene un equipo, incluidas interfaces virtuales o de VPN que podrían ser inesperadas al investigar un incidente.

## ip route
- **Para qué sirve:** muestra la tabla de enrutamiento: la puerta de enlace predeterminada y por dónde sale el tráfico hacia cada red (siguiente salto).
- **Qué mostró en mi equipo:** ruta predeterminada `default via 192.168.100.1 dev wlo1 proto dhcp` (mi router, asignado por DHCP) y la red local `192.168.100.0/24` por la interfaz wifi. También aparecieron rutas de la interfaz de VPN (`25.0.0.0/8` por `ham0`) y de las redes virtuales de Docker (`172.17.0.0/16` a `172.21.0.0/16`, en estado `linkdown`).
- **Capa relacionada:** capa 3 (red, enrutamiento).
- **Utilidad en un SOC:** detectar rutas inesperadas que puedan indicar redirección de tráfico o un túnel/VPN no autorizado.

## ping
- **Para qué sirve:** comprueba la conectividad con un destino usando ICMP (Echo Request y Echo Reply) y mide la latencia y la pérdida de paquetes.
- **Qué mostró en mi equipo:** `ping -c 4 google.com` resolvió el dominio a `142.251.128.46`. Se enviaron 4 paquetes y se recibieron 4 (0 % de pérdida), con tiempos de entre 43 ms y 123 ms (promedio de unos 83 ms) y TTL de 116.
- **Capa relacionada:** capa 3 (red, ICMP).
- **Utilidad en un SOC:** comprobar si un host está activo o alcanzable durante una investigación. El TTL también da una pista sobre el sistema operativo del destino y la distancia en saltos.

## tracepath
- **Para qué sirve:** muestra la ruta que siguen los paquetes hasta un destino, con los routers intermedios y la MTU del trayecto (alternativa a `traceroute` en Ubuntu).
- **Qué mostró en mi equipo:** `tracepath google.com` con MTU 1500. El primer salto fue mi router; el segundo fue una dirección del rango `100.64.0.0/10`, usado por proveedores que comparten IPs (CGNAT); luego una dirección privada de la red del proveedor, routers de operadores argentinos y, por último, equipos de Google, donde la latencia subió de unos 8 ms a unos 124 ms. Los últimos saltos no respondieron (`no reply`), algo normal cuando un router filtra este tráfico.
- **Capa relacionada:** capa 3 (red) y capa 4 (transporte), porque envía paquetes UDP.
- **Utilidad en un SOC:** ver por qué redes pasa el tráfico hacia una IP sospechosa y detectar dónde aumenta la latencia.

## nslookup
- **Para qué sirve:** consulta DNS para traducir un nombre de dominio a su dirección IP (y al revés).
- **Qué mostró en mi equipo:** `nslookup google.com` fue atendido por el resolvedor local `127.0.0.53` (`systemd-resolved`) y devolvió una respuesta no autoritativa con la IPv4 `142.251.128.46` y una dirección IPv6.
- **Capa relacionada:** capa 7 (aplicación, DNS). Normalmente usa UDP en el puerto 53.
- **Utilidad en un SOC:** verificar a qué IP apunta un dominio sospechoso.
