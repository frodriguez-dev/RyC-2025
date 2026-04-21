Introducción
## 1. Investigue y describa cómo funciona el DNS. ¿Cuál es su objetivo?

El Sistema de Nombres de Dominio (DNS, por sus siglas en inglés, Domain Name System) es un servicio de directorio fundamental en Internet. El objetivo principal del DNS es traducir los nombres de host legibles por humanos (como www.ietf.org) en direcciones IP numéricas de longitud fija (por ejemplo, 32 bits para IPv4), que son las que prefieren los routers para el enrutamiento. Las personas prefieren nombres de host fáciles de recordar, mientras que los routers operan con direcciones IP jerárquicas. El DNS reconcilia estas dos preferencias

Su funcionamiento se basa en una base de datos jerárquica y distribuida implementada en una red de servidores DNS. Esta arquitectura distribuida es crucial porque un diseño centralizado sería inviable debido a problemas de escalabilidad, como un único punto de fallo, un volumen de tráfico excesivo, una base de datos centralizada distante y dificultades de mantenimiento . El protocolo DNS opera sobre UDP y utiliza el puerto 53 para sus mensajes de consulta y respuesta

## 2. ¿Qué es un root server? ¿Qué es un generic top-level domain (gtld)?

*   **Servidores DNS raíz (Root Servers):** Son una parte fundamental de la jerarquía del DNS. Existen aproximadamente 400 servidores de nombres raíz distribuidos por todo el mundo, y son gestionados por trece organizaciones diferentes. Su función principal es **proporcionar las direcciones IP de los servidores TLD (Top-Level Domain)**. Actúan como el punto de partida en la resolución de nombres para la mayoría de las consultas DNS, aunque el almacenamiento en caché hace que una pequeña fracción de consultas lleguen a ellos. También pueden utilizar IP-anycast para enrutar consultas al servidor raíz más cercano.
*   **Dominios de nivel superior (TLD) / Generic Top-Level Domain (gTLD):** Los servidores TLD son responsables de todos los dominios de nivel superior. Los documentos mencionan ejemplos como **.com, .org, .net, .edu y .gov**, además de los dominios de nivel superior correspondientes a los distintos países, como .uk, .fr, .ca y .jp. Estos servidores TLD, mantenidos por empresas como Verisign Global Registry Services para .com y Educause para .edu, **proporcionan las direcciones IP de los servidores DNS autoritativos** para dominios específicos. Aunque el término "generic top-level domain" (gTLD) no se define explícitamente, los ejemplos como .com, .org, .net, .edu y .gov se ajustan a esta categoría y se utilizan para describir los dominios de nivel superior.

## 3. ¿Qué es una respuesta del tipo autoritativa?

Una respuesta autoritativa es aquella que proviene de un **servidor DNS que tiene autoridad directa** sobre el nombre solicitado. En un mensaje de respuesta DNS, un **indicador autoritativo de 1 bit se activa** cuando el servidor DNS es el servidor autoritativo para el nombre consultado. Las organizaciones que tienen hosts accesibles públicamente (como servidores web y de correo) deben proporcionar registros DNS accesibles públicamente que establezcan la correspondencia entre los nombres de sus hosts y sus direcciones IP. Un servidor DNS autoritativo de una organización alberga estos registros DNS. Si un servidor DNS es autoritativo para un nombre de host, contendrá un registro de tipo A para ese nombre de host.

## 4. ¿Qué diferencia una consulta DNS recursiva de una iterativa?

La diferencia entre una consulta DNS recursiva y una iterativa radica en cómo se delega la responsabilidad de la resolución del nombre:

*   **Consulta recursiva:** En una consulta recursiva, el cliente DNS (o un servidor DNS actuando en nombre del cliente) solicita al servidor DNS que **obtenga por sí mismo la correspondencia completa**. Esto significa que si el servidor DNS al que se consulta no tiene la respuesta, él mismo se encargará de contactar a otros servidores DNS en la jerarquía (raíz, TLD, autoritativos) hasta obtener la dirección IP final. La consulta de un host a su servidor DNS local es típicamente recursiva.
*   **Consulta iterativa:** En una consulta iterativa, el servidor DNS consultado **responde con la dirección IP de otro servidor DNS** en la jerarquía que está más cerca de tener la respuesta, o con la respuesta final si la posee. El cliente DNS es entonces responsable de enviar una nueva consulta al servidor que le ha sido proporcionado. Las consultas subsiguientes entre los servidores DNS (es decir, del servidor DNS local al servidor raíz, del servidor raíz al servidor TLD, etc.) son normalmente iterativas.

## 5. ¿Qué es el resolver?

El término "resolver" no se define explícitamente como una entidad separada en las fuentes proporcionadas. Sin embargo, se refiere a la **funcionalidad del lado del cliente de la aplicación DNS**. Esta aplicación cliente se ejecuta en el host del usuario y es la encargada de traducir un nombre de host en una dirección IP cuando una aplicación (como un navegador web o un lector de correo) lo necesita. Desde la perspectiva de la aplicación que la invoca, el DNS actúa como una "caja negra" que proporciona este servicio de traducción. En sistemas UNIX, la llamada a función `gethostbyname()` se utiliza comúnmente para realizar esta traducción.

## 6. Describa para qué se utilizan los siguientes tipos de registros de DNS:

Todos los registros de recursos (RR) de DNS tienen el formato general (Nombre, Valor, Tipo, TTL).

*   **a. A (Address record):** Se utiliza para la **correspondencia estándar entre un nombre de host y su dirección IPv4**. El campo "Nombre" es el nombre de host y el campo "Valor" es la dirección IP correspondiente. Por ejemplo, `(relay1.bar.foo.com, 145.37.93.126, A)`.
*   **b. MX (Mail Exchanger record):** Permite que los nombres de host de los servidores de correo tengan alias simples. El campo "Nombre" es un alias de dominio (por ejemplo, `foo.com`) y el campo "Valor" es el nombre canónico de un servidor de correo que gestiona ese alias. Una empresa puede tener el mismo alias para su servidor de correo y para su servidor web utilizando registros MX.
*   **c. PTR (Pointer record):** Se utiliza en las consultas inversas de DNS, para mapear una dirección IP a un nombre de host. El campo "Nombre" es la dirección IP y el "Valor" es el nombre de dominio asociado.
*   **d. AAAA:** Similar al registro A, pero se usa para mapear un nombre de host a una dirección IPv6
*  **e. SRV (Service record)::** Se utiliza para especificar la ubicación de servicios específicos dentro de un dominio, incluyendo el puerto y el protocolo. Es útil en servicios como SIP, LDAP o aplicaciones de mensajería instantánea.
*   **f. NS (Name Server record):** Se utiliza para **delegar autoridad en un dominio y enrutar las consultas DNS** a lo largo de la cadena de consultas. El campo "Nombre" es un dominio (por ejemplo, `foo.com`) y el campo "Valor" es el nombre de host de un servidor DNS autoritativo que sabe cómo obtener las direcciones IP de los hosts dentro de ese dominio.
*   **g. CNAME (Canonical Name record):** Se utiliza para proporcionar un **alias para un nombre de host canónico (original)**. El campo "Nombre" es el alias y el campo "Valor" es el nombre de host canónico al que apunta. Los alias de nombres de host suelen ser más mnemónicos que los nombres canónicos.
*   **h. SOA (Start of Authority record):** Indica la información principal de un dominio y define el servidor DNS primario para la zona, el correo electrónico del administrador, el número de serie de la zona y parámetros de control de refresco y caducidad.
*   **i. TXT (Text record):** Permite asociar información arbitraria en texto plano a un dominio. Se usa, por ejemplo, para verificaciones de propiedad de dominio, políticas de seguridad como SPF (anti-spam), DKIM o configuraciones de servicios externos.

## 7. En Internet, un dominio suele tener más de un servidor DNS, ¿por qué cree que esto es así?

Por si alguno de los servidores falla, para balancear la carga de consultas y para mejorar la disponibilidad y redundancia del servicio DNS. Tener múltiples servidores DNS asegura que si uno está inactivo o sobrecargado, otros pueden manejar las solicitudes, garantizando así una resolución de nombres más rápida y confiable para los usuarios.

## 8. Cuando un dominio cuenta con más de un servidor, uno de ellos es el primario (o maestro) y todos los demás son secundarios (o esclavos). ¿Cuál es la razón de que sea así?
El servidor primario (o maestro) es el que contiene la copia original y autorizada de los datos de la zona DNS. Los servidores secundarios (o esclavos) obtienen sus datos del servidor primario a través de un proceso llamado transferencia de zona. Esta configuración permite distribuir la carga de consultas entre varios servidores, mejorar la redundancia y asegurar que haya siempre una copia actualizada de los datos DNS disponible, incluso si el servidor primario falla. Además, los servidores secundarios pueden estar ubicados en diferentes ubicaciones geográficas para mejorar la latencia y la resiliencia del servicio.

## 9. Explique brevemente en qué consiste el mecanismo de transferencia de zona y cuál es su finalidad.
La transferencia de zona es el proceso mediante el cual un servidor DNS secundario (o esclavo) obtiene una copia actualizada de los datos de la zona DNS desde el servidor primario (o maestro). Este mecanismo es crucial para mantener la coherencia y la sincronización entre los servidores DNS que gestionan un mismo dominio. La finalidad de la transferencia de zona es asegurar que todos los servidores DNS autoritativos para un dominio tengan la información más reciente, permitiendo así una resolución de nombres precisa y confiable. 

## 10. Imagine que usted es el administrador del dominio de DNS de la UNLP (unlp.edu.ar). A su vez, cada facultad de la UNLP cuenta con un administrador que gestiona su propio dominio (por ejemplo, en el caso de la Facultad de Informática se trata de info.unlp.edu.ar). Suponga que se crea una nueva facultad, Facultad de Redes, cuyo dominio será redes.unlp.edu.ar, y el administrador le indica que quiere poder manejar su propio dominio. ¿Qué debe hacer usted para que el administrador de la Facultad de Redes pueda gestionar el dominio de forma independiente? (Pista: investigue en qué consiste la delegación de dominios). Indicar qué registros de DNS se deberían agregar

Se debe agregar el registra NS al servidor de unlp.edu.ar para transferir la autoridad de redes.unlp.edu.ar. Es necesario un registro A?

## 11. Responda y justifique los siguientes ejercicios.
### a. En la VM, utilice el comando dig para obtener la dirección IP del host www.redes.unlp.edu.ar y responda:
#### i. ¿La solicitud fue recursiva? ¿Y la respuesta? ¿Cómo lo sabe?

```bash
redes@debian:~$ dig www.redes.unlp.edu.ar

; <<>> DiG 9.16.27-Debian <<>> www.redes.unlp.edu.ar
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 2077
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 34bdec21760969fb0100000069e57a4dd4a85085692beb9d (good)
;; QUESTION SECTION:
;www.redes.unlp.edu.ar.		IN	A

;; ANSWER SECTION:
www.redes.unlp.edu.ar.	300	IN	A	172.28.0.50

;; Query time: 0 msec
;; SERVER: 172.28.0.29#53(172.28.0.29)
;; WHEN: Sun Apr 19 21:58:53 -03 2026
;; MSG SIZE  rcvd: 94

```
* **qr (Query/Response):** Indica si el mensaje es una **consulta (0)** o una **respuesta (1)**. En tu salida de `dig`, al ser una respuesta del servidor, este flag está presente.
* **aa (Authoritative Answer):** Significa **Respuesta Autoritativa**. Cuando está presente, indica que el servidor que responde es el "dueño" (maestro o esclavo oficial) de la zona DNS consultada y no está entregando una copia guardada en caché.
* **rd (Recursion Desired):** Significa **Recursión Deseada**. Este flag lo setea el cliente (vos) para pedirle al servidor DNS que, si no conoce la respuesta, busque por todo el árbol de jerarquía hasta encontrarla.
* **ra (Recursion Available):** Significa **Recursión Disponible**. Es la respuesta del servidor indicando que **soporta el modo recursivo**. Si este flag no aparece, el servidor solo responderá sobre dominios de los que sea autoridad.

La consulta fue recursiva ya que al utilizar dig, por defecto manda la consulta con rd. Y al tener el flag ra como retorno, significa que el servidor acepta la funcion de la busqueda recursiva.

#### ii. ¿Puede indicar si se trata de una respuesta autoritativa? ¿Qué significa que lo sea?

La respuesta fue autoritaiva ya que contiene el flag aa. Esto significa que el servidor es el dueno de esa informacion que se esta solicitando. 

#### iii. ¿Cuál es la dirección IP del resolver utilizado? ¿Cómo lo sabe?

La direccion del resolver utilizado es 172.28.0.29, y esta definida en la linea:


;; SERVER: 172.28.0.29#53(172.28.0.29)

### b. ¿Cuáles son los servidores de correo del dominio redes.unlp.edu.ar? ¿Por qué hay más de uno y qué significan los números que aparecen entre MX y el nombre? Si se quiere enviar un correo destinado a redes.unlp.edu.ar, ¿a qué servidor se le entregará? ¿En qué situación se le entregará al otro?

```bash

redes@debian:~$ dig redes.unlp.edu.ar MX

; <<>> DiG 9.16.27-Debian <<>> redes.unlp.edu.ar MX
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 7400
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 3

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 271a136de1e0f4ab0100000069e57ee3bcdba42c0b627240 (good)
;; QUESTION SECTION:
;redes.unlp.edu.ar.		IN	MX

;; ANSWER SECTION:
redes.unlp.edu.ar.	86400	IN	MX	5 mail.redes.unlp.edu.ar.
redes.unlp.edu.ar.	86400	IN	MX	10 mail2.redes.unlp.edu.ar.

;; ADDITIONAL SECTION:
mail.redes.unlp.edu.ar.	86400	IN	A	172.28.0.90
mail2.redes.unlp.edu.ar. 86400	IN	A	172.28.0.91

;; Query time: 0 msec
;; SERVER: 172.28.0.29#53(172.28.0.29)
;; WHEN: Sun Apr 19 22:18:27 -03 2026
;; MSG SIZE  rcvd: 149
``` 

* **Servidores:** `mail.redes.unlp.edu.ar` y `mail2.redes.unlp.edu.ar`.
* **Por qué hay más de uno:** Principalmente para ofrecer **tolerancia a fallos**. Si el servidor principal no está disponible, los correos no se pierden y son recibidos por el secundario.
* **Significado de los números:** Es el valor de **preferencia o prioridad**. El emisor intentará entregar el correo al servidor con el número más bajo.
* **A quién se entrega:** Se le entregará a `mail.redes.unlp.edu.ar` por tener la prioridad más alta (valor 5).
* **Cuándo se entrega al otro:** Se utilizará `mail2.redes.unlp.edu.ar` solo si el primero es inalcanzable.

### c. ¿Cuáles son los servidores de DNS del dominio redes.unlp.edu.ar?

```bash
redes@debian:~$ dig redes.unlp.edu.ar NS

; <<>> DiG 9.16.27-Debian <<>> redes.unlp.edu.ar NS
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 45597
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 3

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: bfa4fc7499f9ea0c0100000069e58056944d250d2ff68b58 (good)
;; QUESTION SECTION:
;redes.unlp.edu.ar.		IN	NS

;; ANSWER SECTION:
redes.unlp.edu.ar.	86400	IN	NS	ns-sv-b.redes.unlp.edu.ar.
redes.unlp.edu.ar.	86400	IN	NS	ns-sv-a.redes.unlp.edu.ar.

;; ADDITIONAL SECTION:
ns-sv-a.redes.unlp.edu.ar. 604800 IN	A	172.28.0.30
ns-sv-b.redes.unlp.edu.ar. 604800 IN	A	172.28.0.29

;; Query time: 0 msec
;; SERVER: 172.28.0.29#53(172.28.0.29)
;; WHEN: Sun Apr 19 22:24:38 -03 2026
;; MSG SIZE  rcvd: 150
```

Los servidores DNS del dominio redes.unlp.edu.ar son:
- ns-sv-b.redes.unlp.edu.ar
- ns-sv-a.redes.unlp.edu.ar

### d. Repita la consulta anterior cuatro veces más. ¿Qué observa? ¿Puede explicar a qué se debe?

Observo que hay un cambio en El id, la COOKIE y el When.

### e. Observe la información que obtuvo al consultar por los servidores de DNS del dominio. En base a la salida, ¿es posible indicar cuál de ellos es el primario?
No, no puedo indicar cual de ellos es el primario
### f. Consulte por el registro SOA del dominio y responda.
```bash
redes@debian:~$ dig redes.unlp.edu.ar SOA

; <<>> DiG 9.16.27-Debian <<>> redes.unlp.edu.ar SOA
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 44689
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 4e09ebe8396934600100000069e6b150a49049e772752687 (good)
;; QUESTION SECTION:
;redes.unlp.edu.ar.		IN	SOA

;; ANSWER SECTION:
redes.unlp.edu.ar.	86400	IN	SOA	ns-sv-b.redes.unlp.edu.ar. root.redes.unlp.edu.ar. 2020031700 604800 86400 2419200 86400

;; Query time: 0 msec
;; SERVER: 172.28.0.29#53(172.28.0.29)
;; WHEN: Mon Apr 20 20:05:52 -03 2026
;; MSG SIZE  rcvd: 123

```
#### i. ¿Puede ahora determinar cuál es el servidor de DNS primario?
El servidor de DNS primario es ns-sv-b.redes.unlp.edu.ar

#### ii. ¿Cuál es el número de serie, qué convención sigue y en qué casos es importante actualizarlo?
El numero de serie es 2020031700.
Sigue la convencion de AAAAMMDSS. Ano, mes, dia y s es un valor incremental que se resetea por dia.
Es importante actualizarlo cuando se cambian los registros del servidor para que los servidores slaves se puedan actualizar

#### iii. ¿Qué valor tiene el segundo campo del registro? Investigue para qué se usa y cómo se interpreta el valor.
El segundo campo es de refresh e indica cuando timpo en segundos debe actulizarse el servidor secundario. En este caso 604800 segundos.

#### iv. ¿Qué valor tiene el TTL de caché negativa y qué significa?
Tiene el valor de 86400, significa que si el servidor no respondio correctamente, se quedara con esa respuesta hasta pasados los 86400 segundos que volvera a consultar.


### g. Indique qué valor tiene el registro TXT para el nombre saludo.redes.unlp.edu.ar. Investigue para qué es usado este registro.
```bash
redes@debian:~$ dig saludo.redes.unlp.edu.ar TXT

; <<>> DiG 9.16.27-Debian <<>> saludo.redes.unlp.edu.ar TXT
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 1748
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 60a7b42f4b2675d80100000069e6b362156246aaf9032661 (good)
;; QUESTION SECTION:
;saludo.redes.unlp.edu.ar.	IN	TXT

;; ANSWER SECTION:
saludo.redes.unlp.edu.ar. 86400	IN	TXT	"HOLA"

;; Query time: 0 msec
;; SERVER: 172.28.0.29#53(172.28.0.29)
;; WHEN: Mon Apr 20 20:14:42 -03 2026
;; MSG SIZE  rcvd: 98

```
Tiene el valor de "HOLA" y el registro TXT se utiliza para asociar información arbitraria en texto plano a un dominio. Se usa, por ejemplo, para verificaciones de propiedad de dominio, políticas de seguridad como SPF (anti-spam), DKIM o configuraciones de servicios externos. En este caso, parece ser un mensaje de saludo personalizado.


### h. Utilizando dig, solicite la transferencia de zona de redes.unlp.edu.ar, analice la salida y responda.
```bash
redes@debian:~$ dig redes.unlp.edu.ar AXFR
; <<>> DiG 9.16.27-Debian <<>> redes.unlp.edu.ar AXFR
;; global options: +cmd
redes.unlp.edu.ar.	86400	IN	SOA	ns-sv-b.redes.unlp.edu.ar. root.redes.unlp.edu.ar. 2020031700 604800 86400 2419200 86400
redes.unlp.edu.ar.	86400	IN	NS	ns-sv-a.redes.unlp.edu.ar.
redes.unlp.edu.ar.	86400	IN	NS	ns-sv-b.redes.unlp.edu.ar.
redes.unlp.edu.ar.	86400	IN	MX	5 mail.redes.unlp.edu.ar.
redes.unlp.edu.ar.	86400	IN	MX	10 mail2.redes.unlp.edu.ar.
ftp.redes.unlp.edu.ar.	86400	IN	CNAME	www.redes.unlp.edu.ar.
mail.redes.unlp.edu.ar.	86400	IN	A	172.28.0.90
mail2.redes.unlp.edu.ar. 86400	IN	A	172.28.0.91
ns-sv-a.redes.unlp.edu.ar. 604800 IN	A	172.28.0.30
ns-sv-b.redes.unlp.edu.ar. 604800 IN	A	172.28.0.29
practica.redes.unlp.edu.ar. 86400 IN	NS	ns1.practica.redes.unlp.edu.ar.
practica.redes.unlp.edu.ar. 86400 IN	NS	ns2.practica.redes.unlp.edu.ar.
ns1.practica.redes.unlp.edu.ar.	86400 IN A	172.28.0.120
ns2.practica.redes.unlp.edu.ar.	86400 IN A	172.28.0.121
saludo.redes.unlp.edu.ar. 86400	IN	TXT	"HOLA"
www.redes.unlp.edu.ar.	300	IN	A	172.28.0.50
redes.unlp.edu.ar.	86400	IN	SOA	ns-sv-b.redes.unlp.edu.ar. root.redes.unlp.edu.ar. 2020031700 604800 86400 2419200 86400
;; Query time: 0 msec
;; SERVER: 172.28.0.29#53(172.28.0.29)
;; WHEN: Mon Apr 20 20:16:41 -03 2026
;; XFR size: 17 records (messages 1, bytes 441)
```

#### i. ¿Qué significan los números que aparecen antes de la palabra IN? ¿Cuál es su finalidad?
Es el TTL (Time To Live) y su finalidad es indicar el tiempo en segundos que un registro puede ser almacenado en caché por un servidor DNS o un cliente antes de que deba ser descartado o actualizado. Un valor más alto significa que la información se mantendrá en caché durante más tiempo, mientras que un valor más bajo indica que la información debe ser actualizada con mayor frecuencia.
#### ii. ¿Cuántos registros NS observa? Compare la respuesta con los servidores de DNS del dominio redes.unlp.edu.ar que dio anteriormente. ¿Puede explicar a qué se debe la diferencia y qué significa?
Observo 4 registros NS, dos para redes.unlp.edu.ar y dos para practica.redes.unlp.edu.ar. La diferencia se debe a que practica.redes.unlp.edu.ar es un subdominio de redes.unlp.edu.ar y tiene su propia delegación de servidores DNS. Esto significa que el dominio principal (redes.unlp.edu.ar) delega la autoridad sobre el subdominio (practica.redes.unlp.edu.ar) a los servidores ns1.practica.redes.unlp.edu.ar y ns2.practica.redes.unlp.edu.ar, permitiendo así una gestión independiente del subdominio.

### i. Consulte por el registro A de www.redes.unlp.edu.ar y luego por el registro A de www.practica.redes.unlp.edu.ar. Observe los TTL de ambos. Repita la operación y compare el valor de los TTL de cada uno respecto de la respuesta anterior. ¿Puede explicar qué está ocurriendo? (Pista: observar los flags será de ayuda).
```bash
redes@debian:~$ dig redes.unlp.edu.ar A

; <<>> DiG 9.16.27-Debian <<>> redes.unlp.edu.ar A
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 53473
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 128d0a94421237770100000069e6b4b03252cf1df016ce12 (good)
;; QUESTION SECTION:
;redes.unlp.edu.ar.		IN	A

;; AUTHORITY SECTION:
redes.unlp.edu.ar.	86400	IN	SOA	ns-sv-b.redes.unlp.edu.ar. root.redes.unlp.edu.ar. 2020031700 604800 86400 2419200 86400

;; Query time: 0 msec
;; SERVER: 172.28.0.29#53(172.28.0.29)
;; WHEN: Mon Apr 20 20:20:16 -03 2026
;; MSG SIZE  rcvd: 123

redes@debian:~$ dig practica.redes.unlp.edu.ar A

; <<>> DiG 9.16.27-Debian <<>> practica.redes.unlp.edu.ar A
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50333
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 22d817f457e10ec30100000069e6b4b89fc122f3f4c6ec5b (good)
;; QUESTION SECTION:
;practica.redes.unlp.edu.ar.	IN	A

;; AUTHORITY SECTION:
practica.redes.unlp.edu.ar. 8600 IN	SOA	ns1.practicas.redes.unlp.edu.ar. admin.practicas.redes.unlp.edu.ar. 2020031904 604800 86400 2419200 604800

;; Query time: 1791 msec
;; SERVER: 172.28.0.29#53(172.28.0.29)
;; WHEN: Mon Apr 20 20:20:24 -03 2026
;; MSG SIZE  rcvd: 156

```

En la primera consulta por el registro A de www.redes.unlp.edu.ar, se obtiene una respuesta con un TTL de 86400 segundos, pero no se devuelve una dirección IP en la sección de respuestas. En cambio, se devuelve un registro SOA en la sección de autoridad, lo que indica que el servidor DNS es autoritativo para el dominio pero no tiene un registro A para www.redes.unlp.edu.ar.
En la segunda consulta por el registro A de www.practica.redes.unlp.edu.ar, se obtiene una respuesta similar: no se devuelve una dirección IP en la sección de respuestas, pero se devuelve un registro SOA en la sección de autoridad. El TTL para este registro SOA es de 8600 segundos.
La diferencia en los TTLs se debe a que el registro SOA para practica.redes.unlp.edu.ar tiene un TTL más corto (8600 segundos) en comparación con el registro SOA para redes.unlp.edu.ar (86400 segundos). Esto puede indicar que el servidor DNS para practica.redes.unlp.edu.ar espera que los registros de esa zona cambien con más frecuencia, o simplemente es una configuración diferente para esa zona. En ambos casos, la ausencia de un registro A en la sección de respuestas sugiere que no hay una dirección IP asociada a esos nombres de host, o que el servidor DNS no tiene esa información disponible en ese momento.
### j. Consulte por el registro A de www.practica2.redes.unlp.edu.ar. ¿Obtuvo alguna respuesta? Investigue sobre los códigos de respuesta de DNS. ¿Para qué son utilizados los mensajes NXDOMAIN y NOERROR?
```bash
redes@debian:~$ dig practica2.redes.unlp.edu.ar A

; <<>> DiG 9.16.27-Debian <<>> practica2.redes.unlp.edu.ar A
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 52701
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 409751c7a2eee80d0100000069e6b59948886ed352a7102f (good)
;; QUESTION SECTION:
;practica2.redes.unlp.edu.ar.	IN	A

;; AUTHORITY SECTION:
redes.unlp.edu.ar.	86400	IN	SOA	ns-sv-b.redes.unlp.edu.ar. root.redes.unlp.edu.ar. 2020031700 604800 86400 2419200 86400

;; Query time: 0 msec
;; SERVER: 172.28.0.29#53(172.28.0.29)
;; WHEN: Mon Apr 20 20:24:09 -03 2026
;; MSG SIZE  rcvd: 133

```

En la consulta por el registro A de www.practica2.redes.unlp.edu.ar, se obtiene una respuesta con el código de estado NXDOMAIN. Esto significa que el nombre de dominio solicitado no existe en el servidor DNS consultado. En este caso, el servidor DNS no tiene información sobre www.practica2.redes.unlp.edu.ar, lo que indica que ese subdominio no está registrado o no tiene un registro A asociado. 

El código NOERROR se utiliza para indicar que la consulta fue procesada correctamente y que se encontró una respuesta válida, aunque esa respuesta puede ser vacía (por ejemplo, si no hay un registro A para el nombre consultado).

## 12. Investigue los comandos nslookup y host. ¿Para qué sirven? Intente con ambos comandos obtener:
- Dirección IP de www.redes.unlp.edu.ar.
- Servidores de correo del dominio redes.unlp.edu.ar.
- Servidores de DNS del dominio redes.unlp.edu.ar.

El comando `nslookup` es una herramienta de línea de comandos utilizada para consultar el sistema de nombres de dominio (DNS) para obtener información sobre dominios, como direcciones IP, registros MX, NS, entre otros. Permite realizar consultas tanto recursivas como iterativas y es útil para diagnosticar problemas relacionados con DNS.

```bash
redes@debian:~$ nslookup
> www.redes.unlp.edu.ar
Server:		172.28.0.29
Address:	172.28.0.29#53

Name:	www.redes.unlp.edu.ar
Address: 172.28.0.50
> set type 
*** Invalid option: type
> set type=MX
> redes.unlp.edu.ar
Server:		172.28.0.29
Address:	172.28.0.29#53

redes.unlp.edu.ar	mail exchanger = 10 mail2.redes.unlp.edu.ar.
redes.unlp.edu.ar	mail exchanger = 5 mail.redes.unlp.edu.ar.
> set type=NS
> redes.unlp.edu.ar
Server:		172.28.0.29
Address:	172.28.0.29#53

redes.unlp.edu.ar	nameserver = ns-sv-b.redes.unlp.edu.ar.
redes.unlp.edu.ar	nameserver = ns-sv-a.redes.unlp.edu.ar.

```


El comando `host` es otra herramienta de línea de comandos que se utiliza para realizar consultas DNS. Es más simple que `nslookup` y se enfoca principalmente en resolver nombres de host a direcciones IP y viceversa. También puede mostrar información sobre registros MX, NS, entre otros.

```bash
redes@debian:~$ host www.redes.unlp.edu.ar
www.redes.unlp.edu.ar has address 172.28.0.50

redes@debian:~$ host -t mx redes.unlp.edu.ar
redes.unlp.edu.ar mail is handled by 5 mail.redes.unlp.edu.ar.
redes.unlp.edu.ar mail is handled by 10 mail2.redes.unlp.edu.ar.

redes@debian:~$ host -t ns redes.unlp.edu.ar
redes.unlp.edu.ar name server ns-sv-a.redes.unlp.edu.ar.
redes.unlp.edu.ar name server ns-sv-b.redes.unlp.edu.ar.
```

## 13. ¿Qué función cumple en Linux/Unix el archivo /etc/hosts o en Windows el archivo \WINDOWS\system32\drivers\etc\hosts?
El archivo `/etc/hosts` en Linux/Unix y `\WINDOWS\system32\drivers\etc\hosts` en Windows es un archivo de texto que se utiliza para mapear nombres de host a direcciones IP de manera local. Este archivo actúa como una especie de "libro de direcciones" para el sistema operativo, permitiendo que los usuarios asignen manualmente nombres de host a direcciones IP sin necesidad de consultar un servidor DNS.
## 14. Abra el programa Wireshark para comenzar a capturar el tráfico de red en la interfaz con IP 172.28.0.1. Una vez abierto realice una consulta DNS con el comando dig para averiguar el registro MX de redes.unlp.edu.ar y luego, otra para averiguar los registros NS correspondientes al dominio redes.unlp.edu.ar. Analice la información proporcionada por dig y compárelo con la captura.
Puedo observar que en la captura de Wireshark se muestran los paquetes DNS correspondientes a las consultas realizadas con `dig`. Para la consulta del registro MX, se puede ver un paquete DNS con el tipo de consulta MX y la respuesta que incluye los registros MX para redes.unlp.edu.ar. De manera similar, para la consulta del registro NS, se puede observar un paquete DNS con el tipo de consulta NS y la respuesta que incluye los registros NS para el dominio redes.unlp.edu.ar. La información capturada en Wireshark coincide con las respuestas obtenidas a través de `dig`, confirmando que las consultas DNS se realizaron correctamente y que las respuestas contienen los registros esperados.

## 15. Dada la siguiente situación: “Una PC en una red determinada, con acceso a Internet, utiliza los servicios de DNS de un servidor de la red”. Analice:
### a. ¿Qué tipo de consultas (iterativas o recursivas) realiza la PC a su servidor de DNS?
Las consultas son recursivas, ya que la PC solicita al servidor de DNS que resuelva completamente la consulta, y el servidor se encargará de realizar las consultas necesarias a otros servidores DNS para obtener la respuesta final.
### b. ¿Qué tipo de consultas (iterativas o recursivas) realiza el servidor de DNS para resolver requerimientos de usuario como el anterior? ¿A quién le realiza estas consultas?
El servidor de DNS realiza consultas iterativas para resolver requerimientos de usuario. Cuando el servidor de DNS recibe una consulta recursiva de la PC, si no tiene la respuesta en su caché, realizará consultas iterativas a otros servidores DNS, comenzando por los servidores raíz, luego a los servidores TLD (Top-Level Domain) y finalmente a los servidores autoritativos para obtener la respuesta correcta.
## 16. Relacione DNS con HTTP. ¿Se puede navegar si no hay servicio de DNS?
DNS (Domain Name System) es un sistema que traduce los nombres de dominio legibles por humanos (como www.example.com) en direcciones IP numéricas que las computadoras utilizan para comunicarse entre sí. HTTP (Hypertext Transfer Protocol) es el protocolo utilizado para la transferencia de datos en la web. Cuando un usuario intenta acceder a un sitio web utilizando un nombre de dominio, el navegador primero consulta el servicio de DNS para obtener la dirección IP correspondiente al nombre de dominio. Si no hay servicio de DNS disponible, el navegador no podrá resolver el nombre de dominio a una dirección IP, lo que resultará en la imposibilidad de navegar por ese sitio web utilizando su nombre de dominio. Sin embargo, si el usuario conoce la dirección IP del sitio web, podría acceder a él directamente ingresando la dirección IP en lugar del nombre de dominio, aunque esto no es común ni práctico para la mayoría de los usuarios.

## 17. Observar el siguiente gráfico y contestar

### a. Si la PC-A, que usa como servidor de DNS a "DNS Server", desea obtener la IP de www.unlp.edu.ar, cuáles serían, y en qué orden, los pasos que se ejecutarán para obtener la respuesta.
1. La PC-A envia una consulta a su resolver privado para obtener la dirección IP de www.unlp.edu.ar.
2. Si el resolver privado no tiene la respuesta en su caché, envía una consulta recursiva al servidor DNS configurado (DNS Server).
3. El servidor DNS recibe la consulta y, si no tiene la respuesta en su caché, realiza una consulta iterativa a un servidor raiz mas cercano (A.Root-Server).
4. El servidor raiz responde con la direccion de un servidor TLD (Top-Level Domain) que es responsable del dominio .ar
5. El servidor DNS realiza la consulta iterativa al servidor de a.dns.ar, para obtener la direccion del servidor autoritativo para el dominio .edu.ar
6. El servidor DNS realiza la consulta iterativa al servidor de ns1.riu.edu.ar, que es quien tiene el dominio de edu.ar.
7. El servidor DNS realiza la consulta iterativa al servidor de unlp.unlp.edu.ar, que es el servidor autoritativo para el dominio unlp.edu.ar. 
8. El servidor unlp.edu.ar responde con la direccion IP de www.unlp.edu.ar que es 163.10.0.54.
9. El servidor DNS responde a la PC-A con la dirección IP de www.unlp.edu.ar.

### b. ¿Dónde es recursiva la consulta? ¿Y dónde iterativa?
La consulta es recursiva entre la PC-A y el servidor DNS, ya que la PC-A solicita al servidor DNS que resuelva completamente la consulta. La consulta es iterativa entre el servidor DNS y los servidores raíz, TLD y autoritativos, ya que el servidor DNS realiza consultas a cada uno de estos servidores para obtener la información necesaria para resolver la consulta.
## 18. ¿A quién debería consultar para que la respuesta sobre www.google.com sea autoritativa?
Al servidor que es autoritativo para el dominio google.com, que es el servidor de nombres de Google. Para obtener una respuesta autoritativa, se debería consultar directamente a los servidores DNS que gestionan el dominio google.com, como ns1.google.com, ns2.google.com, etc. Estos servidores tienen la autoridad sobre el dominio google.com y pueden proporcionar respuestas autoritativas sobre los registros DNS asociados a ese dominio.


## 19. ¿Qué sucede si al servidor elegido en el paso anterior se lo consulta por www.info.unlp.edu.ar? ¿Y si la consulta es al servidor 8.8.8.8?
Si se consulta al servidor autoritativo para google.com por www.info.unlp.edu.ar, el servidor no podrá proporcionar una respuesta autoritativa, ya que no tiene autoridad sobre el dominio unlp.edu.ar. En este caso, el servidor podría responder con un mensaje de error o redirigir la consulta a los servidores DNS que gestionan el dominio unlp.edu.ar.
Si la consulta es al servidor 8.8.8.8 (que es un servidor DNS público de Google) por www.info.unlp.edu.ar, el servidor podría proporcionar una respuesta si tiene la información en su caché, pero no sería una respuesta autoritativa. Si el servidor no tiene la información en su caché, realizará consultas iterativas a otros servidores DNS para resolver la consulta, lo que podría resultar en una respuesta más lenta y no autoritativa. En ambos casos, la respuesta no sería autoritativa para el dominio unlp.edu.ar, ya que ni el servidor de Google ni el servidor autoritativo para google.com tienen autoridad sobre el dominio unlp.edu.ar.

## 20. En base a la siguiente salida de dig, conteste las consignas. Justifique en todos los casos.


        ;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 4, ADDITIONAL: 4
        ;; QUESTION SECTION:
        ;ejemplo.com.  IN MX
        ;; ANSWER SECTION:
        ejemplo.com.  1634 IN MX 10 srv01.ejemplo.com.(1)
        ejemplo.com.  1634 IN MX 5 srv00.ejemplo.com.(2)

        ;; AUTHORITY SECTION:
        ejemplo.com.  92354 IN NS ss00.ejemplo.com.
        ejemplo.com.  92354 IN NS ss02.ejemplo.com.
        ejemplo.com.  92354 IN NS ss01.ejemplo.com.
        ejemplo.com.  92354 IN NS ss03.ejemplo.com.

        ;; ADDITIONAL SECTION:
        srv01.ejemplo.com.  272 IN A 64.233.186.26
        srv01.ejemplo.com.  240 IN AAAA 2800:3f0:4003:c00::1a
        srv00.ejemplo.com.  272 IN A 74.125.133.26
        srv00.ejemplo.com.  240 IN AAAA 2a00:1450:400c:c07::1b

### a. Complete las líneas donde aparece __ con el registro correcto.
### b. ¿Es una respuesta autoritativa? En caso de no serlo, ¿a qué servidor le preguntaría para obtener una respuesta autoritativa?
No, no es una respuesta autorivativa, ya que no tiene el flag de aa. Para obtener una respuesta autoritativa, se debe consultar a  cualquiera de los servidores de nombres ss00.ejemplo.com, ss01.ejemplo.com, ss02.ejemplo.com o ss03.ejemplo.com que son los servidores de nombres autoritativos para el dominio ejemplo.com.

### c. ¿La consulta fue recursiva? ¿Y la respuesta?
La consulta fue recursiva, ya que flag de rd está presente, lo que indica que el cliente solicitó una consulta recursiva al servidor DNS.

La respuesta fue recursiva ya que esta el flag de ra, lo que indica que el servidor DNS respondió a la consulta recursiva. 

### d. ¿Qué representan los valores 10 y 5 en las líneas (1) y (2).
Representan la prioridad de los servidores de correo. El servidor con el valor más bajo (5 en este caso) tiene una prioridad más alta, lo que significa que los correos electrónicos se intentarán entregar primero a srv00.ejemplo.com antes de intentar con srv01.ejemplo.com.
