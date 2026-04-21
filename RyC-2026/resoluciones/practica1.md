
# Practica 1 - Introduccion
## 1. ¿Qué es una red? ¿Cuál es el principal objetivo para construir una red?

Una red es un conjunto de dispositivos interconectados que pueden comunicarse entre sí para compartir recursos e información.
El principal objetivo es permitir la comunicación eficiente entre sistemas, reduciendo costos y mejorando el acceso a servicios y datos.

## 2. ¿Qué es Internet? Describa los principales componentes que permiten su funcionamiento.

Es una red de redes de computadoras, descentralizada, publica, que ejecutan el conjunto abierto de protocolos (suite) TCP/IP.


Los principales componentes son:
- Emisor: Dispositivo que envía datos a través de la red.
- Medio de transmisión: Canal físico o lógico que conecta los dispositivos en la red.
- Receptor: Dispositivo que recibe los datos enviados por el emisor.

Sus principales componentes son: routers y switches para el encaminamiento, servidores y clientes como sistemas finales, enlaces de comunicación (fibra, satélites, cables submarinos) y protocolos como TCP/IP que garantizan la interoperabilidad.

## 3. ¿Qué son las RFCs?

Las RFC (Request for Comments) son documentos técnicos publicados por la IETF que definen estándares, protocolos y recomendaciones para el funcionamiento de Internet. Son la base normativa que asegura que todos los sistemas puedan comunicarse de forma compatible.


## 4. ¿Qué es un protocolo?

El conjunto de conductas y normas a conocer, respetar y cumplir no solo en el medio oficial ya establecido, sino tambien en el medio social, laboral. 


## 5. ¿Por qué dos máquinas con distintos sistemas operativos pueden formar parte de una misma red?

Porque la comunicación se basa en protocolos estándar (como TCP/IP) y no en el sistema operativo. Mientras ambas máquinas implementen correctamente esos protocolos, podrán intercambiar información sin importar sus diferencias internas.

## 6. ¿Cuáles son las 2 categorías en las que pueden clasificarse a los sistemas finales o End Systems? Dé un ejemplo del rol de cada uno en alguna aplicación distribuida que corra sobre Internet.

Se clasifican en clientes y servidores.


*Ejemplo*: en una aplicación de streaming, el cliente (usuario) solicita el video y el servidor lo provee mediante Internet.

## 7. ¿Cuál es la diferencia entre una red conmutada de paquetes de una red conmutada de circuitos?

Conmutada significa que los datos no viajan por una ruta fija, sino que se establecen conexiones dinámicas según la demanda.
En la conmutación de circuitos se establece un canal fijo, unico, y exclusivo entre emisor y receptor durante toda la comunicación (ejemplo: telefonía tradicional, servidor con canal unico de datos).

En la conmutación de paquetes, los datos se dividen en paquetes que viajan de forma independiente por distintas rutas, optimizando recursos (ejemplo: Internet).

## 8. Analice qué tipo de red es una red de telefonía y qué tipo de red es Internet.

La red de telefonía tradicional es de conmutación de circuitos, ya que establece un canal dedicado mientras dura la llamada.
Internet es una red de conmutación de paquetes, en la que los datos se transmiten en bloques independientes que comparten los mismos recursos de red.

## 9. Describa brevemente las distintas alternativas que conoce para acceder a Internet en su hogar.

Las alternativas más comunes son: fibra óptica, cable coaxial, ADSL, redes móviles (4G/5G), e incluso satélite.

Cada opción varía en velocidad, latencia y costo, siendo la fibra la más eficiente en la actualidad.

## 10. ¿Qué ventajas tiene una implementación basada en capas o niveles?

Permite separar funciones complejas en módulos más simples y bien definidos.
Esto facilita el diseño, la estandarización, la interoperabilidad entre equipos y el mantenimiento de los sistemas.

## 11. ¿Cómo se llama la PDU de cada una de las siguientes capas: Aplicación, Transporte, Red y Enlace?

- Aplicación → Datos 
- Transporte → Segmento (TCP) o Datagrama (UDP)
- Red → Paquete
- Enlace → Trama

## 12. ¿Qué es la encapsulación? Si una capa realiza la encapsulación de datos, ¿qué capa del nodo receptor realizará el proceso inverso?

La encapsulación es el proceso mediante el cual cada capa del modelo de red añade su propia información de control (cabeceras y, en algunos casos, colas) a los datos que recibe de la capa superior antes de pasarlos a la capa inferior.
El proceso inverso, llamado desencapsulación, lo realiza la misma capa en el nodo receptor, que extrae y procesa la información de control antes de pasar los datos a la capa superior.

## 13. Describa cuáles son las funciones de cada una de las capas del stack TCP/IP o protocolo de Internet.


- Capa de Aplicación: Proporciona servicios de red directamente a las aplicaciones del usuario, como HTTP, FTP, SMTP.
- Capa de Transporte: Gestiona la comunicación de extremo a extremo entre aplicaciones, asegurando la entrega correcta de datos (TCP, UDP).
- Capa de Internet: Encargada del encaminamiento y direccionamiento de paquetes a través de diferentes redes (IP).
- Capa de Interfaz: Maneja la transmisión de datos entre dispositivos en la misma red física, incluyendo protocolos como Ethernet y Wi-Fi.
- Capa Física: Se encarga de la transmisión y recepción de los bits a través del medio físico, definiendo las características eléctricas, mecánicas y procedimentales (cables, conectores, señales).

Por simplicidad algunos autores hablan de 4 capas, agrupando a la Capa de Enlace y Capa fısica en una sola capa que llaman Capa de acceso a la Red


## 14. Compare el modelo OSI con la implementación TCP/IP.
El modelo OSI tiene 7 capas (Aplicación, Presentación, Sesión, Transporte, Red, Enlace, Física), mientras que TCP/IP simplifica en 4 (Aplicación, Transporte, Red, Enlace).
OSI es un modelo teórico de referencia, mientras que TCP/IP es el modelo práctico usado en Internet, más simple y directamente implementado.


Similitudes:
- Ambos se dividen en capas.
- Ambos tienen capas de aplicacion, aunque incluyen servicios distintos.
- Ambos tienen capas de transporte similares.
- Ambos tienen capa de red similar pero con distinto nombre.
- Se supone que la tecnolog´ıa es de conmutacion de paquetes (no de conmutacion de circuitos).

Diferencias:
- TCP/IP combina las funciones de la capa de presentaci´on y de sesi´on en la capa de aplicaci´on.
- TCP/IP combina la capas de enlace de datos y la capa f´ısica del modelo OSI en una sola capa.
- TCP/IP mas simple porque tiene menos capas.
- Los protocolos TCP/IP son los estandares en torno a los cuales se desarrollo Internet, de modo que la credibilidad del modelo TCP/IP se debe en gran parte a sus protocolos.
- El modelo OSI es un modelo “mas” de referencia, teorico, aunque hay implementaciones.
