# Práctica 6 Capa de Transporte - Parte II

## 1. ¿Cuál es el puerto por defecto que se utiliza en los siguientes servicios?  
Web / SSH / DNS / Web Seguro / POP3 / IMAP / SMTP
Investigue en qué lugar en Linux y en Windows está descrita la asociación utilizada por defecto para cada servicio.

- Web: 80 TCP
- SSH: 22 TCP
- DNS: 53 UDP - TCP
- Web Seguro: 443 TCP
- POP3: 110 TCP
- IMAP: 143 TCP
- SMTP: 25 TCP

En linux , la asociación de puertos por defecto se encuentra en el archivo `/etc/services`, mientras que en Windows se encuentra en el archivo `C:\Windows\System32\drivers\etc\services`.

## 2. Investigue qué es multicast. ¿Sobre cuál de los protocolos de capa de transporte funciona? ¿Se podría adaptar para que funcione sobre el otro protocolo de capa de transporte? ¿Por qué?

El multicast es un método de comunicación en redes donde un solo emisor envía datos a múltiples receptores simultáneamente, utilizando una dirección IP especial (rango 224.0.0.0 a 239.255.255.255). Funciona principalmente sobre el protocolo UDP, ya que este permite la transmisión de datos sin necesidad de establecer una conexión previa y sin garantizar la entrega, lo que es adecuado para aplicaciones de transmisión en tiempo real como video o audio.
No se puede adaptar fácilmente para que funcione sobre TCP debido a que TCP es un protocolo orientado a la conexión y garantiza la entrega de datos, lo que implica un control de flujo y retransmisión que no es compatible con el modelo de comunicación multicast. Además, TCP requiere una conexión punto a punto entre el emisor y cada receptor, lo que no es eficiente ni práctico para la transmisión a múltiples destinatarios simultáneamente.

## 3. Investigue cómo funciona el protocolo de aplicación FTP teniendo en cuenta las diferencias en su funcionamiento cuando se utiliza el modo activo de cuando se utiliza el modo pasivo ¿En qué se diferencian estos tipos de comunicaciones del resto de los protocolos de aplicación vistos?

Se diferencian principalmente en la forma en que se establece la conexión de datos entre el cliente y el servidor. En el modo activo, el cliente abre un puerto y espera a que el servidor se conecte a él para la transferencia de datos, mientras que en el modo pasivo, el servidor abre un puerto y espera a que el cliente se conecte a él. Esto permite superar problemas de firewall y NAT que pueden bloquear conexiones entrantes al cliente.
La gran diferencia es quien inicia la conexion de datos.
En el modo activo lo inicia el servidor y en el modo pasivo lo inicia el cliente.
FPT corre en el puerto 21 TCP para control y en el puerto 20 TCP para datos en modo activo, mientras que en modo pasivo, el puerto de datos es dinámico y negociado entre cliente y servidor.

## 4. Suponiendo Selective Repeat; tamaño de ventana 4 y sabiendo que E indica que el mensaje llegó con errores. Indique en el siguiente gráfico, la numeración de los ACK que el host B envía al Host A.

La ventana de Selective Repeat permite que el receptor envíe ACKs individuales para cada paquete recibido correctamente, incluso si algunos paquetes anteriores se han perdido o recibido con errores. Dado un tamaño de ventana de 4, el host B enviará ACKs para los paquetes recibidos correctamente, mientras que los paquetes con errores serán ignorados hasta que se reciban correctamente.
Cuando hay un error en un paquete, el host B no enviará un ACK para ese paquete, sino que esperará a que se retransmita. Por lo tanto, los ACKs enviados por el host B reflejarán únicamente los paquetes recibidos correctamente dentro de la ventana de 4.
Cuando El host A no recibe un ACK para un paquete específico, asumirá que ese paquete se perdió y lo retransmitirá. Esto permite una comunicación más eficiente y confiable, ya que solo los paquetes con errores necesitan ser retransmitidos, en lugar de todos los paquetes enviados después del error.

## 5. ¿Qué restricción existe sobre el tamaño de ventanas en el protocolo Selective Repeat?
La restriccion sobre el tamaño de ventana en el protocolo Selective Repeat es que debe ser menor o igual a la mitad del espacio de secuencia total disponible. Esto se debe a que si la ventana es demasiado grande, podría haber ambigüedad en la identificación de los paquetes, lo que dificultaría la correcta recepción y confirmación de los mismos. Por ejemplo, si el espacio de secuencia es de 8 (0-7), el tamaño máximo de la ventana sería 4. Esto asegura que el receptor pueda diferenciar entre los paquetes nuevos y los retransmitidos, evitando confusiones y garantizando una comunicación confiable.

## 6. De acuerdo a la captura TCP de la siguiente figura, indique los valores de los campos borroneados.

1 linea FLAG SYN, Seq=3933822137
3 linea source 172.20.1.1 destination 172.20.1.100 info 41749 > vce [ACK] Seq=3933822138 Ack=1047471502

## 7. Dada la sesión TCP de la figura, completar los valores marcados con un signo de interrogación.
Seq = 0
Seq = 0, Ack = 1 
Seq = 1, Ack = 1
Seq = 1, Ack = 1
Seq = 1, Ack = 8
Seq = 8, Ack = 1
Seq = 1, Ack = 17
Seq = 17, Ack = 1
Seq = 1, Ack = 22
Seq = 22, Ack = 1
Seq = 1, Ack = 23
Seq = 23, Ack = 2

## 8. ¿Qué es el RTT y cómo se calcula? Investigue la opción TCP timestamp y los campos TSval y TSecr.

El RTT (Round Trip Time) es el tiempo que tarda un paquete en ir desde el emisor hasta el receptor y regresar al emisor. Se calcula midiendo el tiempo transcurrido entre el envío de un paquete y la recepción de su correspondiente ACK (acknowledgment). El RTT es una métrica importante para evaluar la latencia de la red y ajustar los tiempos de retransmisión en protocolos como TCP.

Los campos TSval y TSecr son parte de la opción TCP timestamp. TSval representa el valor del timestamp en el momento del envío del paquete, mientras que TSecr representa el valor del timestamp en el momento de la recepción del paquete. Estos campos se utilizan para mejorar la precisión del cálculo del RTT y para evitar problemas de reordenamiento de paquetes.


## 9. Para la captura tcp-captura.pcap, responder las siguientes preguntas.
### a. ¿Cuántos intentos de conexiones TCP hay?
### b. ¿Cuáles son la fuente y el destino (IP:port) para c/u?
### c. ¿Cuántas conexiones TCP exitosas hay en la captura? ¿Cómo diferencia las exitosas de las que no lo son? ¿Cuáles flags encuentra en cada una?
### d. Dada la primera conexión exitosa responder:
#### i. ¿Quién inicia la conexión?
#### ii. ¿Quién es el servidor y quién el cliente?
#### iii. ¿En qué segmentos se ve el 3-way handshake?
#### iv. ¿Cuáles ISNs se intercambian?
#### v. ¿Cuál MSS se negoció?
#### vi. ¿Cuál de los dos hosts envía la mayor cantidad de datos (IP:port)?
### e. Identificar primer segmento de datos (origen, destino, tiempo, número de fila y número de secuencia TCP).
#### i. ¿Cuántos datos lleva?
#### ii. ¿Cuándo es confirmado (tiempo, número de fila y número de secuencia TCP)?
#### iii. La confirmación, ¿qué cantidad de bytes confirma?
### f. ¿Quién inicia el cierre de la conexión? ¿Qué flags se utilizan? ¿En cuáles segmentos se ve (tiempo, número de fila y número de secuencia TCP)?

## 10. Responda las siguientes preguntas respecto del mecanismo de control de flujo.
###  a. ¿Quién lo activa? ¿De qué forma lo hace?
### b. ¿Qué problema resuelve?
### c. ¿Cuánto tiempo dura activo y qué situación lo desactiva?
## 11. Responda las siguientes preguntas respecto del mecanismo de control de congestión.
### a. ¿Quién activa el mecanismo de control de congestión? ¿Cuáles son los posibles disparadores?
### b. ¿Qué problema resuelve?
### c. Diferencie slow start de congestion-avoidance.
## 12. Para la captura udp-captura.pcap, responder las siguientes preguntas.
### a. ¿Cuántas comunicaciones (srcIP,srcPort,dstIP,dstPort) UDP hay en la captura?
### b. ¿Cómo se podrían identificar las exitosas de las que no lo son?
### c. ¿UDP puede utilizar el modelo cliente/servidor?
### d. ¿Qué servicios o aplicaciones suelen utilizar este protocolo?¿Qué requerimientos tienen?
### e. ¿Qué hace el protocolo UDP en relación al control de errores?
### f. Con respecto a los puertos vistos en las capturas, ¿observa algo particular que lo diferencie de TCP?
### g. Dada la primera comunicación en la cual se ven datos en ambos sentidos (identificar el primer datagrama):
#### i. ¿Cuál es la dirección IP que envía el primer datagrama?,¿desde cuál puerto?
#### ii. ¿Cuántos datos se envían en un sentido y en el otro?

## 13. Dada la salida que se muestra en la imagen, responda los ítems debajo.

Suponga que ejecuta los siguientes comandos desde un host con la IP 10.100.25.90. Responda qué devuelve la ejecución de los siguientes comandos y, en caso que corresponda, especifique los flags.

### a. hping3 -p 3306 –udp 10.100.25.135
### b. hping3 -S -p 25 10.100.25.135
### c. hping3 -S -p 22 10.100.25.135
### d. hping3 -S -p 110 10.100.25.135

¿Cuántas conexiones distintas hay establecidas? Justifique.
