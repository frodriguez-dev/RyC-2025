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

## 11. Responda y justifique los siguientes ejercicios.
### a. En la VM, utilice el comando dig para obtener la dirección IP del host www.redes.unlp.edu.ar y responda:
#### i. ¿La solicitud fue recursiva? ¿Y la respuesta? ¿Cómo lo sabe?
#### ii. ¿Puede indicar si se trata de una respuesta autoritativa? ¿Qué significa que lo sea?
#### iii. ¿Cuál es la dirección IP del resolver utilizado? ¿Cómo lo sabe?

### b. ¿Cuáles son los servidores de correo del dominio redes.unlp.edu.ar? ¿Por qué hay más de uno y qué significan los números que aparecen entre MX y el nombre? Si se quiere enviar un correo destinado a redes.unlp.edu.ar, ¿a qué servidor se le entregará? ¿En qué situación se le entregará al otro?
### c. ¿Cuáles son los servidores de DNS del dominio redes.unlp.edu.ar?
### d. Repita la consulta anterior cuatro veces más. ¿Qué observa? ¿Puede explicar a qué se debe?
### e. Observe la información que obtuvo al consultar por los servidores de DNS del dominio. En base a la salida, ¿es posible indicar cuál de ellos es el primario?
### f. Consulte por el registro SOA del dominio y responda.
#### i. ¿Puede ahora determinar cuál es el servidor de DNS primario?
#### ii. ¿Cuál es el número de serie, qué convención sigue y en qué casos es importante actualizarlo? iii. ¿Qué valor tiene el segundo campo del registro? Investigue para qué se usa y cómo se interpreta el valor.
#### iv. ¿Qué valor tiene el TTL de caché negativa y qué significa?
### g. Indique qué valor tiene el registro TXT para el nombre saludo.redes.unlp.edu.ar. Investigue para qué es usado este registro.
### h. Utilizando dig, solicite la transferencia de zona de redes.unlp.edu.ar, analice la salida y responda.

#### i. ¿Qué significan los números que aparecen antes de la palabra IN? ¿Cuál es su finalidad?
#### ii. ¿Cuántos registros NS observa? Compare la respuesta con los servidores de DNS del dominio redes.unlp.edu.ar que dio anteriormente. ¿Puede explicar a qué se debe la diferencia y qué significa?
### i. Consulte por el registro A de www.redes.unlp.edu.ar y luego por el registro A de www.practica.redes.unlp.edu.ar. Observe los TTL de ambos. Repita la operación y compare el valor de los TTL de cada uno respecto de la respuesta anterior. ¿Puede explicar qué está ocurriendo? (Pista: observar los flags será de ayuda).
### j. Consulte por el registro A de www.practica2.redes.unlp.edu.ar. ¿Obtuvo alguna respuesta? Investigue sobre los códigos de respuesta de DNS. ¿Para qué son utilizados los mensajes NXDOMAIN y NOERROR?

## 12. Investigue los comandos nslookup y host. ¿Para qué sirven? Intente con ambos comandos obtener:
- Dirección IP de www.redes.unlp.edu.ar.
- Servidores de correo del dominio redes.unlp.edu.ar.
- Servidores de DNS del dominio redes.unlp.edu.ar.
## 13. ¿Qué función cumple en Linux/Unix el archivo /etc/hosts o en Windows el archivo \WINDOWS\system32\drivers\etc\hosts?
## 14. Abra el programa Wireshark para comenzar a capturar el tráfico de red en la interfaz con IP 172.28.0.1. Una vez abierto realice una consulta DNS con el comando dig para averiguar el registro MX de redes.unlp.edu.ar y luego, otra para averiguar los registros NS correspondientes al dominio redes.unlp.edu.ar. Analice la información proporcionada por dig y compárelo con la captura.
## 15. Dada la siguiente situación: “Una PC en una red determinada, con acceso a Internet, utiliza los servicios de DNS de un servidor de la red”. Analice:
### a. ¿Qué tipo de consultas (iterativas o recursivas) realiza la PC a su servidor de DNS?
### b. ¿Qué tipo de consultas (iterativas o recursivas) realiza el servidor de DNS para resolver requerimientos de usuario como el anterior? ¿A quién le realiza estas consultas?
## 16. Relacione DNS con HTTP. ¿Se puede navegar si no hay servicio de DNS?
## 17. Observar el siguiente gráfico y contestar

### a. Si la PC-A, que usa como servidor de DNS a "DNS Server", desea obtener la IP de www.unlp.edu.ar, cuáles serían, y en qué orden, los pasos que se ejecutarán para obtener la respuesta.
### b. ¿Dónde es recursiva la consulta? ¿Y dónde iterativa?
## 18. ¿A quién debería consultar para que la respuesta sobre www.google.com sea autoritativa?
## 19. ¿Qué sucede si al servidor elegido en el paso anterior se lo consulta por www.info.unlp.edu.ar? ¿Y si la consulta es al servidor 8.8.8.8?
