# Práctica 5 Capa de Transporte - Parte I

## 1. ¿Cuál es la función de la capa de transporte?
Proveer comunicacion logica entre procesos de aplicacion entre diferentes hosts dentro de la red.

## 2. Describa la estructura del segmento TCP y UDP.

_TCP_:
- Puerto origen
- Puerto destino
- Numero de secuencia (SEQ)
- Numero de reconocimiento (ACK)
- Flags (TCP tiene flags porque es orientado a la conexion, mientras que UDP no tiene flags por ser sin conexion):
    - URG
    - ACK: Si el flag ACK está activo, el campo de número de reconocimiento es válido y contiene el valor del siguiente número de secuencia que el emisor espera recibir.
    - PSH
    - RST: Si el flag RST está activo, indica que se debe restablecer la conexión.
    - SYN: Si el flag SYN está activo, indica que se está iniciando una conexión y el campo de número de secuencia contiene el número de secuencia inicial (ISN) del emisor.
    - FIN: Si el flag FIN está activo, indica que el emisor ha terminado de enviar datos y desea cerrar la conexión.
- Tamaño de ventana (Window size)
- Checksum
- Puntero urgente (Urgent pointer)
- Opciones (Options)
- Datos (Data)

_UDP_:
- Puerto origen
- Puerto destino
- Longitud (Length)
- Checksum
- Datos (Data)


## 3. ¿Cuál es el objetivo del uso de puertos en el modelo TCP/IP?
El objetivo del uso de puertos es permitir la *multiplexación* de servicios en un mismo host, de manera que múltiples aplicaciones puedan comunicarse simultáneamente a través de la red utilizando el mismo protocolo de transporte (TCP o UDP) pero diferenciándose por los números de puerto asignados a cada servicio. Esto permite que el sistema operativo pueda dirigir correctamente los datos entrantes al proceso adecuado según el puerto destino.

## 4. Compare TCP y UDP en cuanto a:
### a.​ Confiabilidad
TCP es confiable porque garantiza la entrega de datos en orden y sin errores, mientras que UDP no garantiza la entrega ni el orden de los datos.

### b.​ Multiplexación.
TCP y UDP permiten la multiplexación de servicios mediante el uso de puertos

### c.​ Orientado a la conexión.
TCP es orientado a la conexión, lo que significa que establece una conexión antes de enviar datos y mantiene el estado de la comunicación durante toda la sesión. UDP es sin conexión, lo que significa que no establece una conexión antes de enviar datos y no mantiene el estado de la comunicación.

### d.​ Controles de congestión.
TCP implementa mecanismos de control de congestión para evitar saturar la red y garantizar un rendimiento óptimo, mientras que UDP no tiene control de congestión, lo que puede llevar a la pérdida de paquetes en redes congestionadas.

### e.​ Utilización de puertos.
TCP y UDP utilizan puertos para identificar los procesos de aplicación que se comunican, pero TCP requiere un puerto de origen y un puerto de destino para establecer una conexión, mientras que UDP solo necesita un puerto de destino para enviar datagramas.

## 5. La PDU de la capa de transporte es el segmento. Sin embargo, en algunos contextos suele utilizarse el término datagrama. Indique cuando.
Se utiliza el termino de datagrama cuando se hace referencia a la PDU de la capa de transporte en el contexto del protocolo UDP, ya que este protocolo es sin conexión y no garantiza la entrega de los datos, a diferencia de TCP que utiliza segmentos para garantizar la entrega confiable.

## 6. Describa el saludo de tres vías de TCP. ¿UDP tiene esta característica?
1. El cliente envía un segmento SYN al servidor para iniciar la conexión.
2. El servidor responde con un segmento SYN-ACK, confirmando la recepción del SYN y enviando su propio número de secuencia inicial (ISN).
3. El cliente envía un segmento ACK al servidor, confirmando la recepción del SYN-ACK y estableciendo la conexión.
UDP no tiene esta característica, ya que es un protocolo sin conexión y no establece una conexión antes de enviar datos, por lo que no realiza un saludo de tres vías.

## 7. Investigue qué es el ISN (Initial Sequence Number). Relaciónelo con el saludo de tres vías.
El ISN (Initial Sequence Number) es un número de secuencia inicial que se utiliza en el protocolo TCP para identificar de manera única cada conexión y garantizar la entrega ordenada de los datos. 
Durante el saludo de tres vías, el ISN es generado por ambos extremos de la conexión (cliente y servidor) y se intercambia en los segmentos SYN y SYN-ACK. El cliente envía su ISN en el primer segmento SYN, y el servidor responde con su propio ISN en el segmento SYN-ACK. Esto permite que ambos extremos acuerden un punto de partida para la numeración de los segmentos que se enviarán durante la comunicación, asegurando que los datos se reciban en el orden correcto y sin duplicaciones.

## 8. Investigue qué es el MSS. ¿Cuándo y cómo se negocia?
El MSS (Maximum Segment Size) es el tamaño máximo de segmento que un host puede recibir en una conexión TCP. Se negocia durante el saludo de tres vías, específicamente en los segmentos SYN y SYN-ACK, donde cada extremo de la conexión indica su MSS preferido. Esto permite que ambos extremos acuerden un tamaño de segmento que sea compatible y eficiente para la transmisión de datos, evitando fragmentación y optimizando el rendimiento de la comunicación.

## 9. Utilice el comando ss (reemplazo de netstat) para obtener la siguiente información de su PC:

### a.​ Para listar las comunicaciones TCP establecidas.

``` bash
ss -t -a
```

### b.​ Para listar las comunicaciones UDP establecidas.
``` bash
ss -u -a
```
### c.​ Obtener sólo los servicios TCP que están esperando comunicaciones
``` bash
ss -t -l
```
### d.​ Obtener sólo los servicios UDP que están esperando comunicaciones.
``` bash
ss -u -l
```
### e.​ Repetir los anteriores para visualizar el proceso del sistema asociado a la conexión.
``` bash
ss -t -a -p
```
### f.​ Obtenga la misma información planteada en los items anteriores usando el comando netstat.
``` bash
netstat -t -a
netstat -u -a
netstat -t -l
netstat -u -l
netstat -t -a -p
```

## 10. ¿Qué sucede si llega un segmento TCP con el flag SYN activo a un host que no tiene ningún proceso esperando en el puerto destino de dicho segmento (es decir, el puerto destino no está en estado LISTEN)?
En TCP , si llega un segmento con el flag SYN activo a un host que no tiene ningún proceso escuchando en el puerto destino, el host responderá con un segmento TCP con el flag RST (Reset) activo. Esto indica al remitente que la conexión no puede ser establecida porque no hay un servicio disponible en ese puerto.

### a.​ Utilice hping3 para enviar paquetes TCP al puerto destino 22 de la máquina virtual con el flag SYN activado.
```bash
hping3 -S -p 22 127.0.0.1
redes@debian:/$ sudo /usr/sbin/hping3 -S -p 22 127.0.0.1
HPING 127.0.0.1 (lo 127.0.0.1): S set, 40 headers + 0 data bytes
len=44 ip=127.0.0.1 ttl=64 DF id=0 sport=22 flags=SA seq=0 win=65495 rtt=7.8 ms
len=44 ip=127.0.0.1 ttl=64 DF id=0 sport=22 flags=SA seq=1 win=65495 rtt=7.8 ms
^C
--- 127.0.0.1 hping statistic ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 7.8/7.8/7.8 ms

```
### b.​ Utilice hping3 para enviar paquetes TCP al puerto destino 40 de la máquina virtual con el flag SYN activado.
```bash
hping3 -S -p 40 127.0.0.1
redes@debian:/$ sudo /usr/sbin/hping3 -S -p 40 127.0.0.1
HPING 127.0.0.1 (lo 127.0.0.1): S set, 40 headers + 0 data bytes
len=40 ip=127.0.0.1 ttl=64 DF id=0 sport=40 flags=RA seq=0 win=0 rtt=7.9 ms
len=40 ip=127.0.0.1 ttl=64 DF id=0 sport=40 flags=RA seq=1 win=0 rtt=7.0 ms
^C
--- 127.0.0.1 hping statistic ---
3 packets transmitted, 2 packets received, 34% packet loss
round-trip min/avg/max = 7.0/7.4/7.9 ms

```
### c.​ ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores?  ¿Puede explicar a qué se debe? (Ayuda: utilice el comando ss visto anteriormente).
En el primer caso, el puerto 22 estaba en estado LISTEN, por lo que el host respondio con las FLAGS SA (SYN-ACK), indicando que el puerto estaba abierto y listo para establecer una conexión. En el segundo caso, el puerto 40 no estaba en estado LISTEN, por lo que el host respondió con las FLAGS RA (RST-ACK), indicando que no había ningún proceso escuchando en ese puerto y que la conexión no podía ser establecida. Esto se debe a que TCP requiere que haya un proceso escuchando en el puerto destino para poder establecer una conexión, mientras que si no hay ningún proceso escuchando, el host envía un RST para indicar que la conexión no puede ser establecida.

## 11. ¿Qué sucede si llega un datagrama UDP a un host que no tiene ningún proceso esperando en el puerto destino de dicho datagrama (es decir, que dicho puerto no está en estado LISTEN)?
### a.​ Utilice hping3 para enviar datagramas UDP al puerto destino 5353 de la máquina virtual.
```bash
hping3 -2 -p 5353 127.0.0.1
redes@debian:/$ sudo /usr/sbin/hping3 --udp -p 5353 127.0.0.1
HPING 127.0.0.1 (lo 127.0.0.1): udp mode set, 28 headers + 0 data bytes
^C
--- 127.0.0.1 hping statistic ---
6 packets transmitted, 0 packets received, 100% packet loss
round-trip min/avg/max = 0.0/0.0/0.0 ms
```

### b.​ Utilice hping3 para enviar datagramas UDP al puerto destino 40 de la máquina virtual.
```bash
hping3 -2 -p 40 127.0.0.1
redes@debian:/$ sudo /usr/sbin/hping3 --udp -p 40 127.0.0.1
HPING 127.0.0.1 (lo 127.0.0.1): udp mode set, 28 headers + 0 data bytes
ICMP Port Unreachable from ip=127.0.0.1 name=localhost 
status=0 port=1666 seq=0
ICMP Port Unreachable from ip=127.0.0.1 name=localhost 
status=0 port=1667 seq=1
ICMP Port Unreachable from ip=127.0.0.1 name=localhost 
status=0 port=1668 seq=2
ICMP Port Unreachable from ip=127.0.0.1 name=localhost 
status=0 port=1669 seq=3
^C
--- 127.0.0.1 hping statistic ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 5.7/11.3/22.1 ms

```
### c.​ ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores?  ¿Puede explicar a qué se debe? (Ayuda: utilice el comando ss visto anteriormente).
En el primer caso, se envían paquetes UDP al puerto 5353 que puede ser que este escuchando algun proceso, pero como no se recibe respuesta porque es UDP, no se puede determinar si el puerto está abierto o cerrado. En el segundo caso, se envían paquetes UDP al puerto 40 que no tiene ningún proceso escuchando, por lo que el host responde con un mensaje ICMP "Port Unreachable", indicando que el puerto está cerrado y no hay ningún proceso escuchando en ese puerto. Esto se debe a que UDP es un protocolo sin conexión y no garantiza la entrega de los datos, por lo que no hay una respuesta esperada si el puerto destino está abierto, mientras que si el puerto destino está cerrado, se recibe un mensaje ICMP indicando que el puerto no está disponible. 

## 12. Investigue los distintos tipos de estado que puede tener una conexión TCP.
Ver
https://users.cs.northwestern.edu/~agupta/cs340/project2/TCPIP_State_Transition_Diagram.pdf

Los estados de una conexión TCP son los siguientes:
- LISTEN: El servidor está esperando una conexión entrante.
- SYN-SENT: El cliente ha enviado un segmento SYN y está esperando un segmento SYN-ACK del servidor.
- SYN-RECEIVED: El servidor ha recibido un segmento SYN y ha enviado un segmento SYN-ACK al cliente, esperando un segmento ACK del cliente.
- ESTABLISHED: La conexión está establecida y los datos pueden ser enviados y recibidos.
- FIN-WAIT-1: El cliente ha enviado un segmento FIN y está esperando un segmento ACK del servidor.
- FIN-WAIT-2: El cliente ha recibido un segmento ACK del servidor y está esperando un segmento FIN del servidor.
- CLOSE-WAIT: El servidor ha recibido un segmento FIN del cliente y está esperando a que la aplicación cierre la conexión.
- CLOSING: El cliente ha enviado un segmento FIN y ha recibido un segmento FIN del servidor, esperando un segmento ACK del servidor.
- LAST-ACK: El servidor ha enviado un segmento FIN y está esperando un segmento ACK del cliente.
- TIME-WAIT: El cliente ha recibido un segmento FIN del servidor y ha enviado un segmento ACK, esperando un tiempo para asegurarse de que el servidor ha recibido el ACK antes de cerrar la conexión.
- CLOSED: La conexión está cerrada y no hay comunicación entre el cliente y el servidor.

## 13. Dada la siguiente salida del comando ss, responda: (Ver pdf)
### a.​ ¿Cuántas conexiones hay establecidas?
Hay 9 conexiones establecidas. State = ESTABLISHED.

### b.​ ¿Cuántos puertos hay abiertos a la espera de posibles nuevas conexiones?
Hay 5 puertos abiertos a la espera de posibles nuevas conexiones. State = LISTEN.

### c.​ El cliente y el servidor de las comunicaciones HTTPS (puerto 443), ¿residen en la misma máquina?
Las ips que salen en Local Address y Peer Address son diferentes, por lo que el cliente y el servidor de las comunicaciones HTTPS (puerto 443) no residen en la misma máquina.

### d.​ El cliente y el servidor de la comunicación SSH (puerto 22), ¿residen en la misma máquina?
Si, las ips que salen en Local Address y Peer Address son iguales, por lo que el cliente y el servidor de la comunicación SSH (puerto 22) residen en la misma máquina.

### e.​ Liste los nombres de todos los procesos asociados con cada comunicación. Indique para cada uno si se trata de un proceso cliente o uno servidor.
Para saber si un proceso es cliente o servidor, se puede observar el estado de la conexión. Si el estado es LISTEN, entonces es un proceso servidor, mientras que si el estado es ESTABLISHED, entonces es un proceso cliente.

El proceso sshd es un proceso servidor, y al mismo tiempo es un proceso cliente.
El proceso apache2 es un proceso servidor.
El proceso named es un proceso servidor.
Todos los procesos x-www-browser son procesos cliente.
El proceso postfix es un proceso servidor.

### f.​ ¿Cuáles conexiones tuvieron el cierre iniciado por el host local y cuáles por el remoto?
Las conexiones que tuvieron el cierre iniciado por el host local son aquellas donde el estado es TIME_WAIT o CLOSED. Las conexiones que tuvieron el cierre iniciado por el remoto son aquellas donde el estado es FIN_WAIT_1, FIN_WAIT_2, o LAST_ACK.

El proceso con pid=1079 tuvo el cierre iniciado por el host local, ya que su estado es TIME_WAIT.
El proceso con pid=1079 tuvo el cierre iniciado por el remoto, ya que su estado es CLOSE-WAIT.

### g.​ ¿Cuántas conexiones están aún pendientes por establecerse?
Una sola y es la que tiene el estado SYN-SENT, que indica que el cliente ha enviado un segmento SYN y está esperando un segmento SYN-ACK del servidor.

## 14. Dadas las salidas de los siguientes comandos ejecutados en el cliente y el servidor, responder: (Ver pdf)
```
servidor# ss -natu | grep 110
tcp LISTEN 0 0 *:110 *:*
tcp SYN-RECV 0 0 157.0.0.1:110 157.0.11.1:52843
cliente# ss -natu | grep 110
tcp SYN-SENT 0 1 157.0.11.1:52843 157.0.0.1:110
```
### a. ¿Qué segmentos llegaron y cuáles se están perdiendo en la red?
Llega el segmento SYN del cliente al servidor, y el servidor responde con un segmento SYN-ACK al cliente. Sin embargo, el segmento SYN-ACK del servidor no llega al cliente, por lo que se está perdiendo en la red.

### b. ¿A qué protocolo de capa de aplicación y de transporte se está intentando conectar el cliente?
Se esta intentando conectar al protocolo POP3 de capa de aplicacion y al protocolo TCP de capa de transporte.

### c. ¿Qué flags tendría seteado el segmento perdido?
El segmento perdido tendria seteado el flag SYN y el flag ACK, ya que es un segmento SYN-ACK enviado por el servidor al cliente.


## 15. Use CORE para armar una topología como la siguiente, sobre la cual deberá realizar:
### a.​ En ambos equipos inspeccionar el estado de las conexiones y mantener abiertas ambas ventanas con el comando corriendo para poder visualizar los cambios a medida que se realiza el ejercicio. Ayuda: watch -n1 ’ss -nat’.
Primero ejecutar export TERM = xterm para poder ejecutar el comando watch en la terminal de CORE. Luego ejecutar el comando watch -n1 'ss -nat' en ambos equipos para inspeccionar el estado de las conexiones y mantener abiertas ambas ventanas con el comando corriendo para poder visualizar los cambios a medida que se realiza el ejercicio.
### b.​ En Servidor, utilice la herramienta ncat para levantar un servicio que escuche en el puerto 8001/TCP. Utilice la opción -k para que el servicio sea persistente. Verifique el estado de las conexiones.
Ejectuar el comando `ncat -l -k 8001` en el Servidor para levantar un servicio que escuche en el puerto 8001/TCP de manera persistente. Luego, verificar el estado de las conexiones utilizando el comando `ss -nat` para asegurarse de que el servicio está escuchando correctamente en el puerto especificado.
### c.​ Desde CLIENTE1 conectarse a dicho servicio utilizando también la herramienta ncat. Inspeccione el estado de las conexiones.
Ejecutar el comando `ncat -v <IP_DEL_SERVIDOR> 8001` desde CLIENTE1 para conectarse al servicio levantado en el Servidor. Luego, inspeccionar el estado de las conexiones utilizando el comando `ss -nat` en ambos equipos para verificar que la conexión se ha establecido correctamente y que el estado de la conexión es ESTABLISHED. 


### d.​ Iniciar otra conexión desde CLIENTE1 de la misma manera que la anterior y verificar el estado de las conexiones. ¿De qué manera puede identificar cada conexión?
Para identificar cada conexión, se puede utilizar el comando `ss -nat` en ambos equipos y observar los diferentes pares de direcciones IP y puertos que se están utilizando para cada conexión.

### e.​ En base a lo observado en el item anterior, ¿es posible iniciar más de una conexión desde el cliente al servidor en el mismo puerto destino? ¿Por qué? ¿Cómo se garantiza que los datos de una conexión no se mezclarán con los de la otra?
Sí, es posible iniciar más de una conexión desde el cliente al servidor en el mismo puerto destino. Esto se debe a que cada conexión se identifica por un par de direcciones IP y puertos únicos. La combinación de dirección IP del cliente, puerto del cliente, dirección IP del servidor y puerto del servidor forma un identificador único para cada conexión. Por lo tanto, los datos de una conexión no se mezclarán con los de la otra.

### f.​ Analice en el tráfico de red, los flags de los segmentos TCP que ocurren cuando:
##### i. Cierra la última conexión establecida desde CLIENTE1. Evalúe los estados de las conexiones en ambos equipos.
Los flags que se observan en los segmentos TCP durante el cierre de la última conexión establecida desde CLIENTE1 son los siguientes:
1. CLIENTE1 envía un segmento FIN al Servidor, indicando que desea cerrar la conexión.
2. El Servidor responde con un segmento ACK, confirmando la recepción del FIN.
3. El Servidor envía un segmento FIN al CLIENTE1, indicando que también desea cerrar la conexión.
4. CLIENTE1 responde con un segmento ACK, confirmando la recepción del FIN del Servidor.

##### ii. Corta el servicio de ncat en el servidor (Ctrl+C). Evalúe los estados de las conexiones en ambos equipos.
Los flags que se observan en los segmentos TCP durante el corte del servicio de ncat en el servidor son los siguientes:
1. El Servidor envía un segmento FIN al CLIENTE1, indicando que desea cerrar la conexión.
2. CLIENTE1 responde con un segmento ACK, confirmando la recepción del FIN.
3. CLIENTE1 envía un segmento FIN al Servidor, indicando que también desea cerrar la conexión.
4. El Servidor responde con un segmento ACK, confirmando la recepción del FIN del CLIENTE1.

##### iii. Cierra la conexión en el cliente. Evalúe nuevamente los estados de las conexiones.
Los flags que se observan en los segmentos TCP durante el cierre de la conexión en el cliente son los siguientes:
1. CLIENTE1 envía un segmento FIN al Servidor, indicando que desea cerrar la conexión.
2. El Servidor responde con un segmento ACK, confirmando la recepción del FIN.
3. El Servidor envía un segmento FIN al CLIENTE1, indicando que también desea cerrar la conexión.
4. CLIENTE1 responde con un segmento ACK, confirmando la recepción del FIN del Servidor.
