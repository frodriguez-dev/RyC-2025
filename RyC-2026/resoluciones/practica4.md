# Correo Electrónico
## 1. ¿Qué protocolos se utilizan para el envío de mails entre el cliente y su servidor de correo? ¿Y entre servidores de correo?
Entre el envio de mails entre el clinete y el serividor se utiliza el protocolo SMTP (Simpole Mail Transfer Protocol) y
entre los servidores de correo se utiliza el mismo protocolo SMTP.
## 2. ¿Qué protocolos se utilizan para la recepción de mails? Enumere y explique características y diferencias entre las alternativas posibles.
Para la recepcion de mails se utilizan los protocolos POP3 (Post Office Protocol) e IMAP4 (Internet Message Access
Protocol).

POP3:
- Puede descargar los correos al cliente y eliminarlos del servidor, lo que significa que los correos solo estarán disponibles en el dispositivo donde se descargaron.

IMAP4:
- Permite acceder a los correos directamente en el servidor, lo que significa que los correos permanecen en el servidor
y se pueden acceder desde múltiples dispositivos.
- Permite la sincronización de carpetas y estados de los correos (leído, no leído, etc.) entre diferentes dispositivos.
- Permite descargar solo los encabezados de los correos, lo que puede ser útil para revisar rápidamente los correos sin
descargar todo el contenido.

## 3. Utilizando la VM y teniendo en cuenta los siguientes datos, abra el cliente de correo (Thunderbird) y configure dos cuentas de correo. Una de las cuentas utilizará POP para solicitar al servidor los mails recibidos para la misma mientras que la otra utilizará IMAP. Al ingresar a cada una de las cuentas, seleccionar Manual config y luego de configurar las mismas según lo indicado, ignorar advertencias por uso de conexión sin cifrado.
### Datos para POP
- Cuenta de correo: alumnopop@redes.unlp.edu.ar
- Nombre de usuario: alumnopop
- Contraseña: alumnopoppass
- Puerto: 110
### Datos para IMAP
- Cuenta de correo: alumnoimap@redes.unlp.edu.ar
- Nombre de usuario: alumnoimap
- Contraseña: alumnoimappass
- Puerto: 143

### Datos comunes para ambas cuentas

Servidor de correo entrante (POP/IMAP):
- Nombre: mail.redes.unlp.edu.ar' 
- SSL: None
- Autenticación: Normal password

Servidor de correo saliente (SMTP):
- Nombre: mail.redes.unlp.edu.ar
- Puerto: 25
- SSL: None
- Autenticación: Normal password

### a.Verificar el correcto funcionamiento enviando un email desde el cliente de una cuenta a la otra y luego desde la otra responder el mail hacia la primera.
Esta todo funcionando correctamente ya que se puede enviar y recibir correos entre las dos cuentas configuradas.

### b. Análisis del protocolo SMTP
#### i. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo mientras desde la cuenta alumnopop@redes.unlp.edu.ar envía un correo a alumnoimap@redes.unlp.edu.ar

#### ii. Utilice el filtro SMTP para observar los paquetes del protocolo SMTP en la captura generada y analice el intercambio de dicho protocolo entre el cliente y el servidor para observar los distintos comandos utilizados y su correspondiente respuesta. Ayuda: filtre por protocolo SMTP y sobre alguna de las líneas del intercambio haga click derecho y seleccione Follow TCP Stream. . .

```bash
220 mail.redes.unlp.edu.ar ESMTP Postfix (Lihuen-4.01/GNU)
EHLO [172.28.0.1]
250-mail.redes.unlp.edu.ar
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-STARTTLS
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-DSN
250 CHUNKING
MAIL FROM:<alumnopop@redes.unlp.edu.ar> BODY=8BITMIME SIZE=452
250 2.1.0 Ok
RCPT TO:<alumnoimap@redes.unlp.edu.ar>
250 2.1.5 Ok
DATA
354 End data with <CR><LF>.<CR><LF>
Message-ID: <7a460a71-2b53-c6e9-6b8f-f11b649e1ed1@redes.unlp.edu.ar>
Date: Mon, 5 Oct 2026 19:22:54 -0300
MIME-Version: 1.0
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101
 Thunderbird/91.12.0
Content-Language: en-US
To: alumnoimap@redes.unlp.edu.ar
From: alumnopop <alumnopop@redes.unlp.edu.ar>
Subject: Punto 3B
Content-Type: text/plain; charset=UTF-8; format=flowed
Content-Transfer-Encoding: 7bit

A VER QUE ONDA

.
250 2.0.0 Ok: queued as D7692600F2
QUIT
221 2.0.0 Bye
```

Luego del handshake inicial, el servidor envia un mensaje de bioenvenida y el cliente envia el comando EHLO para identificarse. 
El servidor responde con una lista de capacidades que soporta. 
Luego, el cliente envia el comando MAIL FROM para indicar el remitente del correo, seguido del comando RCPT TO para indicar el destinatario. 
El servidor responde con un mensaje de confirmación para cada comando. 
Luego, el cliente envia el comando DATA para indicar que va a enviar el contenido del correo. 
El servidor responde con un mensaje indicando que puede enviar los datos. 
El cliente envia el contenido del correo y finaliza con un punto (.) en una línea separada. El servidor responde con un mensaje de confirmación indicando que el correo ha sido aceptado y encolado para su entrega. 
Finalmente, el cliente envia el comando QUIT para finalizar la sesión y el servidor responde con un mensaje de despedida.


### c. Usando el cliente de correo Thunderbird del usuario alumnopop@redes.unlp.edu.ar envíe un correo electrónico alumnoimap@redes.unlp.edu.ar el cual debe tener: un asunto, datos en el body y una imagen adjunta.
#### i. Verifique las fuentes del correo recibido para entender cómo se utiliza el header “Content-Type: multipart/mixed“ para poder realizar el envío de distintos archivos adjuntos.

Con el header 'Content-Type: multipart/mixed' se indica que el mensaje contiene múltiples partes, cada una con su propio tipo de contenido. Esto permite enviar tanto texto como archivos adjuntos en un solo correo electrónico. Cada parte del mensaje está separada por un límite (boundary) definido en el header, y cada parte puede tener su propio header indicando el tipo de contenido y la codificación utilizada.


#### ii. Extraiga la imagen adjunta del mismo modo que lo hace el cliente de correo a partir de los fuentes del mensaje.

Lo hace decodificando en base64 la imagen

## 4. Análisis del protocolo POP
### a. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo mientras desde la cuenta alumnoimap@redes.unlp.edu.ar le envía una correo a alumnopop@redes.unlp.edu.ar y mientras alumnopop@redes.unlp.edu.ar recepciona dicho correo.

### b. Utilice el filtro POP para observar los paquetes del protocolo POP en la captura generada y analice el intercambio de dicho protocolo entre el cliente y el servidor para observar los distintos comandos utilizados y su correspondiente respuesta.

```bash
+OK Dovecot ready.
CAPA
+OK
CAPA
TOP
UIDL
RESP-CODES
PIPELINING
AUTH-RESP-CODE
STLS
USER
SASL PLAIN
.
AUTH PLAIN
+ 
AGFsdW1ub3BvcABhbHVtbm9wb3BwYXNz
+OK Logged in.
STAT
+OK 2 1789
LIST
+OK 2 messages:
1 1018
2 771
.
UIDL
+OK
1 0000000356eaa394
2 0000000456eaa394
.
RETR 2
+OK 771 octets
Return-Path: <alumnoimap@redes.unlp.edu.ar>
X-Original-To: alumnopop@redes.unlp.edu.ar
Delivered-To: alumnopop@redes.unlp.edu.ar
Received: from [172.28.0.1] (unknown [172.28.0.1])
	by mail.redes.unlp.edu.ar (Postfix) with ESMTP id EDD28600F2
	for <alumnopop@redes.unlp.edu.ar>; Mon,  5 Oct 2026 22:54:55 +0000 (UTC)
Message-ID: <5221be76-2d6b-9759-8e2b-2468a08c52a9@redes.unlp.edu.ar>
Date: Mon, 5 Oct 2026 19:54:50 -0300
MIME-Version: 1.0
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101
 Thunderbird/91.12.0
Content-Language: en-US
To: alumnopop <alumnopop@redes.unlp.edu.ar>
From: alumnoimap <alumnoimap@redes.unlp.edu.ar>
Subject: 4a
Content-Type: text/plain; charset=UTF-8; format=flowed
Content-Transfer-Encoding: 7bit

HOLA

.
QUIT
+OK Logging out.
```

Primero el servidor envia un mensaje de bienvenida y el cliente envia el comando CAPA para solicitar las capacidades del servidor.
Luego el clinete envia el comando AUTH PLAIN para autenticarse y el servidor responde con un mensaje de confirmación.
El clinete envia su token de autenticación codificado en base64 y el servidor responde con un mensaje de confirmación indicando que el usuario ha sido autenticado correctamente.
El clinete envia el comando STAT para solicitar el número de mensajes y el tamaño total del buzón, y el servidor responde con un mensaje que indica que hay 2 mensajes en el buzón con un tamaño total de 1789 bytes.
El cliente envia el comando LIST para solicitar una lista de los mensajes en el buzón, y el servidor responde con un mensaje que indica que hay 2 mensajes en el buzón con sus respectivos tamaños.
El cliente envia el comando UIDL para solicitar una lista de los identificadores únicos de los mensajes.
El cliente envia el comando RETR 2 para solicitar el contenido del segundo mensaje, y el servidor responde con el contenido del mensaje.
El cliente envia el comando QUIT para finalizar la sesión y el servidor responde con un mensaje de despedida.

## 5. Análisis del protocolo IMAP
### a.Utilizando Wireshark, capture el tráfico de red contra el servidor de correo mientras desde la cuenta alumnopop@redes.unlp.edu.ar le envía un correo a alumnoimap@redes.unlp.edu.ar y mientras alumnoimap@redes.unlp.edu.ar recepciona dicho correo.
### b. Utilice el filtro IMAP para observar los paquetes del protocolo IMAP en la captura generada y analice el intercambio de dicho protocolo entre el cliente y el servidor para observar los distintos comandos utilizados y su correspondiente respuesta.

```bash
* 7 EXISTS
* 7 RECENT
DONE
207 OK Idle completed (32.559 + 32.557 + 32.558 secs).
208 noop
208 OK NOOP completed (0.001 + 0.000 secs).
209 UID fetch 9:* (FLAGS)
* 7 FETCH (UID 9 FLAGS (\Recent))
209 OK Fetch completed (0.001 + 0.000 secs).
210 UID fetch 9 (UID RFC822.SIZE FLAGS BODY.PEEK[HEADER.FIELDS (From To Cc Bcc Subject Date Message-ID Priority X-Priority References Newsgroups In-Reply-To Content-Type Reply-To)])
* 7 FETCH (UID 9 RFC822.SIZE 773 FLAGS (\Recent) BODY[HEADER.FIELDS (FROM TO CC BCC SUBJECT DATE MESSAGE-ID PRIORITY X-PRIORITY REFERENCES NEWSGROUPS IN-REPLY-TO CONTENT-TYPE REPLY-TO)] {265}
Message-ID: <38be801c-fc6e-e4e9-80c4-4bbd5917a5f2@redes.unlp.edu.ar>
Date: Mon, 5 Oct 2026 20:07:51 -0300
To: alumnoimap@redes.unlp.edu.ar
From: alumnopop <alumnopop@redes.unlp.edu.ar>
Subject: PUNTO 5
Content-Type: text/plain; charset=UTF-8; format=flowed

)
210 OK Fetch completed (0.001 + 0.000 secs).
211 UID fetch 9 (UID RFC822.SIZE BODY.PEEK[])
* 7 FETCH (UID 9 RFC822.SIZE 773 BODY[] {773}
Return-Path: <alumnopop@redes.unlp.edu.ar>
X-Original-To: alumnoimap@redes.unlp.edu.ar
Delivered-To: alumnoimap@redes.unlp.edu.ar
Received: from [172.28.0.1] (unknown [172.28.0.1])
	by mail.redes.unlp.edu.ar (Postfix) with ESMTP id DE4FA600F2
	for <alumnoimap@redes.unlp.edu.ar>; Mon,  5 Oct 2026 23:07:56 +0000 (UTC)
Message-ID: <38be801c-fc6e-e4e9-80c4-4bbd5917a5f2@redes.unlp.edu.ar>
Date: Mon, 5 Oct 2026 20:07:51 -0300
MIME-Version: 1.0
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101
 Thunderbird/91.12.0
Content-Language: en-US
To: alumnoimap@redes.unlp.edu.ar
From: alumnopop <alumnopop@redes.unlp.edu.ar>
Subject: PUNTO 5
Content-Type: text/plain; charset=UTF-8; format=flowed
Content-Transfer-Encoding: 7bit

punto 5 jeje

)
211 OK Fetch completed (0.001 + 0.000 secs).
212 UID fetch 9 (UID BODY.PEEK[HEADER.FIELDS (Content-Type Content-Transfer-Encoding)] BODY.PEEK[TEXT]<0.2048>)
* 7 FETCH (UID 9 BODY[HEADER.FIELDS (CONTENT-TYPE CONTENT-TRANSFER-ENCODING)] {91}
Content-Type: text/plain; charset=UTF-8; format=flowed
Content-Transfer-Encoding: 7bit

 BODY[TEXT]<0> {16}
punto 5 jeje

)
212 OK Fetch completed (0.001 + 0.000 secs).
213 IDLE
+ idling

```

El servidor envia un mensaje de bienvenida y el cliente envia el comando IDLE para indicar que está listo para recibir notificaciones de nuevos mensajes.
El servidor responde con un mensaje indicando que hay 7 mensajes en el buzón y que hay 7 mensajes recientes.
El cliente envia el comando NOOP para mantener la conexión activa y el servidor responde con un mensaje.
El cliente le envia un UID fetch 9 (FLAGS) para obtener las banderas del mensaje con UID 9 y el servidor responde con un mensaje indicando que el mensaje tiene la bandera \Recent.
El cliente solicita mas información del mensaje con UID 9, incluyendo el tamaño del mensaje y los encabezados del mensaje, y el servidor responde con la información solicitada.
El clinete solicita el contenido del mensaje con UID 9 y el servidor responde con el contenido del mensaje.
El cliente solicita los encabezados del mensaje con UID 9 y el servidor responde con los encabezados del mensaje.
Por ultimo el clinete envia el comando IDLE para indicar que está listo para recibir notificaciones de nuevos mensajes y el servidor responde con un mensaje indicando que está en modo de espera.

## 6. IMAP vs POP
### a.Marque como leídos todos los correos que tenga en el buzón de entrada de alumnopop y de alumnoimap. Luego, cree una carpeta llamada POP en la cuenta de alumnopop y una llamada IMAP en la cuenta de alumnoimap. Asegúrese que tiene mails en el inbox y en la carpeta recientemente creada en cada una de las cuentas.
### b. Cierre la sesión de la máquina virtual del usuario redes e ingrese nuevamente identificándose como usuario root y password packer, ejecute el cliente de correos. De esta forma, iniciará el cliente de correo con el perfil del superusuario (diferente del usuario con el que ya configuró las cuentas antes mencionadas). Luego configure las cuentas POP e IMAP de los usuarios alumnopop y alumnoimap como se describió anteriormente pero desde el cliente de correos ejecutado con el usuario root. Responda:
#### i. ¿Qué correos ve en el buzón de entrada de ambas cuentas? ¿Están marcados como leídos o como no leídos? ¿Por qué?
En el POP estan marcados como no leidos ya que el protocolo POP descarga los correos al cliente y los elimina del servidor, por lo que no se sincroniza el estado de los correos entre diferentes clientes. En cambio, en el IMAP los correos están marcados como leídos ya que el protocolo IMAP permite acceder a los correos directamente en el servidor y sincroniza el estado de los correos entre diferentes clientes.

#### ii. ¿Qué pasó con las carpetas POP e IMAP que creó en el paso anterior?
En el POP no se sincronizan las carpetas creadas en el cliente, por lo que no se ven en el cliente configurado con el usuario root. En cambio, en el IMAP si se sincronizan las carpetas creadas en el cliente, por lo que se ven en el cliente configurado con el usuario root.

### c.En base a lo observado. ¿Qué protocolo le parece mejor? ¿POP o IMAP? ¿Por qué? ¿Qué protocolo considera que utiliza más recursos del servidor? ¿Por qué?

IMAP me parece mejor ya que permite acceder a los correos directamente en el servidor y sincroniza el estado de los correos entre diferentes clientes, lo que permite una mejor gestión de los correos. Sin embargo, IMAP utiliza más recursos del servidor ya que mantiene una conexión constante con el servidor y requiere más ancho de banda para sincronizar los correos entre diferentes clientes.

## 7. ¿En algún caso es posible enviar más de un correo durante una misma conexión TCP? Considere:
-  Destinatarios múltiples del mismo dominio entre MUA-MSA y entre MTA-MTA
- Destinatarios múltiples de diferentes dominios entre MUA-MSA y entre MTA-MTA
## 8. Indique sí es posible que el MSA escuche en un puerto TCP diferente a los convencionales y qué implicancias tendría.
## 9. Indique sí es posible que el MTA escuche en un puerto TCP diferente a los convencionales y qué implicancias tendría.
## 10. Ejercicio integrador HTTP, DNS y MAIL
Suponga que registró bajo su propiedad el dominio redes2024.com.ar y dispone de 4 servidores:
- Un servidor DNS instalado configurado como primario de la zona redes2024.com.ar. (hostname: ns1 - IP: 203.0.113.65).
- Un servidor DNS instalado configurado como secundario de la zona redes2024.com.ar. (hostname: ns2 - IP: 203.0.113.66).
- Un servidor de correo electrónico (hostname: mail - IP: 203.0.113.111). Permitirá a los usuarios envíar y recibir correos a cualquier dominio de Internet.
- Un servidor WEB para el acceso a un webmail (hostname: correo - IP: 203.0.113.8). Permitirá a los usuarios gestionar vía web sus correos electrónicos a través de la URL https://webmail.redes2024.com.ar
### a. ¿Qué información debería informar al momento del registro para hacer visible a Internet el dominio registrado?
Deberia enviar al servidor DNS que maneja .com.ar la informacion de los servidores de nombres (ns1.redes2024.com.ar y ns2.redes2024.com.ar) y sus respectivas direcciones IP (203.0.113.65 y 203.0.113.66 para que puedan resolver las consultas de DNS para el dominio redes2024.com.ar.

### b.¿Qué registros sería necesario configurar en el servidor de nombres? Indique toda la información necesaria del archivo de zona. Puede utilizar la siguiente tabla de referencia (evalúe la necesidad de usar cada caso los siguientes campos): Nombre del registro, Tipo de registro, Prioridad, TTL, Valor del registro.

redes2024.com.ar          3600 IN NS ns1.redes2024.com.ar
redes2024.com.ar          3600 IN NS ns2.redes2024.com.ar
ns1.redes2024.com.ar      3600 IN A 203.0.113.65
ns2.redes2024.com.ar      3600 IN A 203.0.113.66

redes2024.com.ar          56   IN SOA ns1.redes2024.com.ar. admin.redes2024.com.ar. 
                                  2024061501 ; Serial
                                  3600       ; Refresh
                                  1800       ; Retry
                                  604800     ; Expire
                                  86400      ; Minimum TTL

mail.redes2024.com.ar      3600 IN  A      203.0.113.111
redes2024.com.ar           3600 IN  MX  5  mail.redes2024.com.ar

correo.redes2024.com.ar    3600 IN A       203.0.113.8
webmail.redes2024.com.ar   3600 IN CNAME   correo.redes2024.com.ar

### c. ¿Es necesario que el servidor de DNS acepte consultas recursivas? Justifique.
No, no es necesario que el servidor de DNS acepte consultas recursivas ya que su función principal es resolver las consultas de los clientes de su dominio y no de otros dominios. Las consultas recursivas son necesarias para los servidores de nombres que actúan como resolvers para los clientes finales, pero en este caso, los servidores ns1 y ns2 solo necesitan responder a las consultas de su propio dominio.

### d. ¿Qué servicios/protocolos de capa de aplicación configuraría en cada servidor?
En el servidor de nombres (ns1 y ns2) configuraría el servicio de DNS utilizando el protocolo UDP en el puerto 53 para consultas y el protocolo TCP en el puerto 53 para transferencias de zona.
En el servidor de correo (mail) configuraría el servicio de correo electrónico utilizando el protocolo SMTP en el puerto 25 TCP para envío de correos, y los protocolos POP3 en el puerto 110 TCP y IMAP en el puerto 143 TCP para recepción de correos.
En el servidor web (correo) configuraría el servicio de webmail utilizando el protocolo HTTPS en el puerto 443 TCP para acceso seguro a través de la web.

### e. Para cada servidor, ¿qué puertos considera necesarios dejar abiertos a Internet?. A modo de referencia, para cada puerto indique: servidor, protocolo de transporte y número de puerto.
Respondida arriba 

### f.¿Cómo cree que se conectaría el webmail del servidor web con el servidor de correo? ¿Qué protocolos usaría y para qué?
Utilizaria los protocolos IMAP o POP3 para recibir los correos y SMTP para enviar los correos. El webmail se conectaria al servidor de correo utilizando el protocolo IMAP o POP3 para acceder a los correos almacenados en el servidor de correo y el protocolo SMTP para enviar los correos desde el webmail a otros destinatarios.

### g.¿Cómo se podría hacer para que cualquier MTA reconozca como válidos los mails provenientes del dominio redes2024.com.ar solamente a los que llegan de la dirección 203.0.113.111? ¿Afectaría esto a los mails enviados desde el Webmail? Justifique.
Se deberia agregar un registro TXT con la clave publica para que al momento de recibir un correo, el MTA receptor pueda verificar que el correo proviene de un servidor autorizado para enviar correos en nombre del dominio redes2024.com.ar. Esto se hace mediante la implementación de SPF (Sender Policy Framework).
Afectaria a los mails enviados desde el Webmail si este no está configurado para enviar correos a través del servidor de correo autorizado.

### h. ¿Qué característica propia de SMTP, IMAP y POP hace que al adjuntar una imagen o un ejecutable sea necesario aplicar un encoding (ej. base64)?
Es debido a que estos protocolos envian texto plano y no pueden manejar datos binarios directamente. Por lo tanto, es necesario codificar los archivos adjuntos en un formato que pueda ser transmitido como texto, como base64, para asegurar que los datos se mantengan intactos durante la transmisión.

### i.¿Se podría enviar un mail a un usuario de modo que el receptor vea que el remitente es un usuario distinto? En caso afirmativo, ¿Cómo? ¿Es una indicación de una estafa? Justifique
Se podria siempre y cuando el servidor de correo permita enviar correos con remitentes falsificados. Esto se puede hacer configurando el campo "From" del correo electrónico con una dirección de correo diferente a la del remitente real. Sin embargo, esto es una práctica deshonesta y puede ser una indicación de una estafa, ya que se está intentando engañar al receptor haciéndole creer que el correo proviene de una fuente confiable cuando en realidad no es así. Además, muchos servidores de correo implementan medidas de seguridad como SPF, DKIM y DMARC para detectar y bloquear correos con remitentes falsificados.

### j.¿Se podría enviar un mail a un usuario de modo que el receptor vea que el destinatario es un usuario distinto? En caso afirmativo, ¿Cómo? ¿Por qué no le llegaría al destinatario que el receptor ve? ¿Es esto una indicación de una estafa? Justifique
Si se puede haciendo mail spoofing, donde se falsifica la dirección del destinatario en el encabezado del correo electrónico. Esto se puede hacer configurando el campo "To" del correo electrónico con una dirección de correo diferente a la del destinatario real. Sin embargo, esto no garantiza que el correo llegue al destinatario que el receptor ve, ya que los servidores de correo pueden verificar la autenticidad del destinatario y rechazar correos con direcciones falsificadas. Además, esto es una práctica deshonesta y puede ser una indicación de una estafa, ya que se está intentando engañar al receptor haciéndole creer que el correo está destinado a otra persona cuando en realidad no es así.

### k.¿Qué protocolo usará nuestro MUA para enviar un correo con remitente redes@info.unlp.edu.ar? ¿Con quién se conectará? ¿Qué información será necesaria y cómo la obtendría?
Utiliza SMTP para enviar el correo y se conectara con agente MSA del servidor de correo de info.unlp.edu.ar, sera necesaria la ip del servidor de correo y el puerto 25, esta informacion se obtiene a traves de una consulta DNS por registros MX al dominio info.unlp.edu.ar.

### l.Dado que solo disponemos de un servidor de correo, ¿qué sucederá con los mails que intenten ingresar durante un reinicio del servidor?
Se quedan en una cola de espera en el servidor de correo del remitente y se reintentará la entrega una vez que el
servidor de correo de info.unlp.edu.ar vuelva a estar disponible.

### m.Suponga que contratamos un servidor de correo electrónico en la nube para integrarlo con nuestra arquitectura de servicios.
#### i. ¿Cómo configuraría el DNS para que ambos servidores de correo se comporten de manera de dar un servicio de correo tolerante a fallos?
Se configuraria un registro MX adicional en el DNS con una prioridad más alta para el servidor de correo en la nube, de manera que si el servidor de correo local no está disponible, los correos se envíen al servidor de correo en la nube. Esto permite que ambos servidores de correo trabajen juntos para proporcionar un servicio de correo tolerante a fallos.

## 11. Utilizando la herramienta Swaks envíe un correo electrónico con las siguientes características:
- Dirección destino: Dirección de correo de alumnoimap@redes.unlp.edu.ar
- Dirección origen: redesycomunicaciones@redes.unlp.edu.ar
- Asunto: SMTP-Práctica4
- Archivo adjunto: PDF del enunciado de la práctica
- Cuerpo del mensaje: Esto es una prueba del protocolo SMTP
### a. Analice tanto la salida del comando swaks como los fuentes del mensaje recibido para responder las siguientes preguntas:
#### i. ¿A qué corresponde la información enviada por el servidor destino como respuesta al comando EHLO? Elija dos de las opciones del listado e investigue la funcionalidad de la misma.
#### ii. Indicar cuáles cabeceras fueron agregadas por la herramienta swaks.
#### iii. ¿Cuál es el message-id del correo enviado? ¿Quién asigna dicho valor?
#### iv. ¿Cuál es el software utilizado como servidor de correo electrónico?
#### v. Adjunte la salida del comando swaks y los fuentes del correo electrónico.

### b.Descargue de la plataforma la captura de tráfico smtp.pcap y la salida del comando swaks smtp.swaks para responder y justificar los siguientes ejercicios.
#### i. ¿Por qué el contenido del mail no puede ser leído en la captura de tráfico?
### c.Realice una consulta de DNS por registros TXT al dominio info.unlp.edu.ar y entre dichos registros evalúe la información del registro SPF. ¿Por qué cree que aparecen muchos servidores autorizados?
### d.Realice una consulta de DNS por registros TXT al dominio outlook.com y analice el registro correspondiente a SPF. ¿Cuáles son los bloques de red autorizados para enviar mails?. Investigue para qué se utiliza la directiva "~all"

## 12. Observar el gráfico a continuación y teniendo en cuenta lo siguiente , responder:
- El usuario juan@misitio.com.ar en PC-A desea enviar un mail al usuario alicia@example.com
- Cada organización tiene su propios servidores de DNS y Mail
- El servidor ns1 de misitio.com.ar no tiene la recursión habilitadoa
- Los hosts del dominio misitio.com.ar utilizan como servidor recursivo el 8.8.8.8 (DNS de Google)
### a.El servidor de mail, mail1, y de HTTP, www, de example.com tienen la misma IP, ¿es posible esto? Si lo es, ¿cómo lo resolvería?
Si, es posible ya que corren en diferentes puertos, el servidor de mail utiliza el puerto 25 para SMTP y el servidor web utiliza el puerto 80 para HTTP. 
Esto se resuelve mediante la configuración de los registros DNS correspondientes en el servidor dns2, donde se puede tener un registro A apuntando a la misma IP para ambos servicios y luego utilizar registros MX para el correo y registros A o CNAME para el web.

### b.Al enviar el mail, ¿por cuál registro de DNS consultará el MUA?
Consulta por el registro A ya que el MUA ya tiene preconfigurado el dominio de su MSA, entonces soolamente le falta
conocer la ip de ese dominio.

### c.Una vez que el mail fue recibido por el servidor smtp-5, ¿por qué registro de DNS consultará?
Por el o los registros MX de example.com

### d. Si en el punto anterior smtp-5 recibiese un listado de nombres de servidores de correo, ¿será necesario realizar una consulta de DNS adicional? Si es afirmativo, ¿por qué tipo de registro y de cuál servidor preguntaría?
Tiene que realizar una consulta adicional por el registro A del serividor MX con menor prioridad, ya que el registro MX
solo devuelve el nombre del servidor de correo y no su dirección IP. Por lo tanto, es necesario realizar una consulta
adicional para obtener la dirección IP del servidor de correo al que se debe enviar el correo.

### e. Indicar todo el proceso que deberá realizar el servidor ns1 de misitio.com.ar para obtener los servidores de mail de example.com.
- El servidor ns1 de misitio.com.ar le consulta al serivodor DNS 8.8.8.8 por el registro MX de example.com
- El servidor DNS de google le responde

### f. Teniendo en cuenta el proceso de encapsulación/desencapsulación y definición de protocolos, responder V o F y justificar:
- Los datos de la cabecera de SMTP deben ser analizados por el servidor DNS para responder a la consulta de los registros MX. 
Falso -> El servidor DNS no analiza las cabeceras de SMTP, el servidor origen  (MTA) de correo es quien realiza la consulta por los registros MX del dominio destino. 

- Al ser recibidos por el servidor smtp-5 los datos agregados por el protocolo SMTP serán analizados por cada una de las capas inferiores. 
FALSO -> Los datos son analizados por la capa de aplicacion.

- Cada protocolo de la capa de aplicación agrega una cabecera con información propia de ese protocolo. 
Verdadero -> Cada protocolo de la capa de aplicación agrega una cabecera con información propia de ese protocolo.

- Como son todos protocolos de la capa de aplicación, las cabeceras agregadas por el protocolo de DNS puede ser analizadas y comprendidas por el protocolo SMTP o HTTP.
FALSO -> los diferentes protocolos de la capa de aplicación no pueden analizarse entre sí, ya que cada uno tiene su propio formato y estructura de cabecera.

- Para que los cliente en misitio.com.ar puedan acceder el servidor HTTP www.example.com y mostrar correctamente su contenido deben tener el mismo sistema operativo.
FALSO -> El sistema operativo es irrelevante para la comunicacion entre clientes y servidores.

### g. Un cliente web que desea acceder al servidor www.example.com y que no pertenece a ninguno de estos dos dominios puede usar a ns1 de misitio.com.ar como servidor de DNS para resolver la consulta?
No puede ya que ns1 no es autorizativo de www.ecample.com y no tiene recursión habilitada, por lo que no puede resolver la consulta para un dominio que no es de su propiedad.

### h. Cuando Alicia quiera ver sus mails desde PC-D, ¿qué registro de DNS deberá consultarse?
Debe consultar por el registro A ya que el MUA ya tiene preconfigurado el dominio de su MAA, entonces solamente le falta la IP de ese dominio.

### i.Indicar todos los protocolos de mail involucrados, puerto y si usan TCP o UDP, en el envío y recepción de dicho mail

SMPT -> Puerto 25 TCP -> Envio
POP3 -> Puerto 110 TCP -> Recepcion
IMAP -> Puerto 143 TCP -> Recepcion

HTTP -> Puerto 80 TCP -> Acceso a webmail
