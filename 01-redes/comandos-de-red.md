# Comandos de red en Ubuntu

## ip a
- **Para qué sirve:** Muestra todas las interfaces de red del sistema junto con sus direcciones IP, direcciones físicas MAC, estado del enlace y la máscara de subred. Es el comando nativo moderno en Linux (que reemplaza al antiguo `ifconfig`).
- **Qué mostró en mi equipo:** Listó las interfaces activas (como `lo` para loopback y `eth0`/`wlan0` para red física/Wi-Fi), especificando el estado de conexión (`state UP`), la dirección IP asignada (`inet 192.168.x.x/24`) y la MAC física (`link/ether xx:xx:xx:xx:xx:xx`).
- **Capa relacionada:** Capa 3 (Red - Dirección IP) y Capa 2 (Enlace de datos - Dirección MAC).

---

## ip route
- **Para qué sirve:** Muestra la tabla de enrutamiento del sistema operativo. Permite identificar la puerta de enlace predeterminada, las rutas hacia subredes locales y los saltos que utilizará el tráfico saliente.
- **Qué mostró en mi equipo:** La ruta predeterminada (`default via 192.168.x.1 dev eth0`) que redirige todo el tráfico hacia el router, así como el rango de red local asignado a la interfaz.
- **Capa relacionada:** Capa 3 (Red - Enrutamiento e IP).

---

## ping
- **Para qué sirve:** Comprueba la conectividad de red de extremo a extremo utilizando el protocolo ICMP (mensajes *Echo Request* y *Echo Reply*). Mide el tiempo de respuesta en milisegundos (latencia) y la pérdida de paquetes.
- **Qué mostró en mi equipo:** El intercambio de paquetes enviando peticiones a un destino (ej. `ping 8.8.8.8`), confirmando si la dirección de destino está alcanzable, la cantidad de bytes devueltos, el TTL (*Time To Live*) y el RTT (*Round-Trip Time*).
- **Capa relacionada:** Capa 3 (Red - Protocolo ICMP).

---

## tracepath
- **Para qué sirve:** Rastrea la ruta completa que sigue un paquete desde el equipo local hasta un destino, mostrando cada uno de los routers o saltos (*hops*) intermedios en el camino, midiendo la latencia en cada punto y descubriendo la MTU (*Maximum Transmission Unit*) del enlace. Es la alternativa nativa en Ubuntu a `traceroute` o `tracert`.
- **Qué mostró en mi equipo:** Una lista numerada con las direcciones IP de los routers intermedios atravesados hasta llegar al host destino, indicando los milisegundos de respuesta de cada salto.
- **Capa relacionada:** Capa 3 (Red - Enrutamiento y control de ICMP/UDP).

---

## nslookup
- **Para qué sirve:** Consulta a los servidores DNS (*Domain Name System*) para resolver un nombre de dominio a su dirección IP correspondiente (*registro A/AAAA*) o realizar la resolución inversa (*de IP a dominio*). Es fundamental para diagnosticar fallas de resolución de nombres.
- **Qué mostró en mi equipo:** La dirección IP del servidor DNS que procesó la consulta localmente y la respuesta con la dirección IP pública asociada al dominio consultado (ej. resolviendo `google.com` a su IP).
- **Capa relacionada:** Capa 7 (Aplicación - Servicio DNS).
