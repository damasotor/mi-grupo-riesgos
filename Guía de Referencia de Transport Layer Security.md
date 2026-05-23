A continuación se presenta una traducción técnica y formal al español del documento, adaptada a la terminología estándar utilizada en la ingeniería de sistemas y la ciberseguridad.

---

# Guía de Referencia (Cheat Sheet) de Transport Layer Security (TLS)

## Introducción

Esta guía de referencia proporciona pautas para implementar la protección de la capa de transporte en aplicaciones mediante el uso de *Transport Layer Security* (TLS). Se centra principalmente en cómo utilizar TLS para proteger a los clientes que se conectan a una aplicación web a través de HTTPS, aunque gran parte de estas directrices también son aplicables a otros usos de TLS. Cuando se implementa correctamente, TLS puede proporcionar varios beneficios de seguridad:

* **Confidencialidad:** Proporciona protección contra atacantes que intenten leer el contenido del tráfico.
* **Integridad:** Proporciona protección contra la modificación del tráfico, evitando por ejemplo que un atacante replique solicitudes (*replay attacks*) contra el servidor.
* **Autenticación:** Permite al cliente confirmar que está conectado al servidor legítimo. Cabe destacar que la identidad del cliente no se verifica a menos que se empleen certificados de cliente.

## SSL vs. TLS

*Secure Socket Layer* (SSL) fue el protocolo original utilizado para proporcionar cifrado al tráfico HTTP, dando forma a HTTPS. Se lanzaron públicamente dos versiones de SSL: las versiones 2 y 3. Ambas presentan graves debilidades criptográficas y ya no deben utilizarse.

Por diversos motivos, la siguiente versión del protocolo (que funcionalmente era SSL 3.1) se denominó *Transport Layer Security* (TLS) versión 1.0. Posteriormente, se han lanzado las versiones 1.1, 1.2 y 1.3 de TLS.

Los términos "SSL", "SSL/TLS" y "TLS" se utilizan con frecuencia de manera intercambiable y, en muchos casos, se usa "SSL" al referirse al protocolo TLS más moderno. Esta guía utilizará el término "TLS", excepto cuando se haga referencia a los protocolos heredados (*legacy*).

## Configuración del Servidor

### Soportar Únicamente Protocolos Robustos

Las aplicaciones web deben configurarse de forma predeterminada para usar TLS 1.3 y pueden soportar TLS 1.2 por motivos de compatibilidad. TLS 1.0 y TLS 1.1 están formalmente declarados obsoletos por el RFC 8996 (marzo de 2021) y deben deshabilitarse. También están prohibidos por PCI DSS, desaprobados por la directriz NIST SP 800-52 Rev. 2 y eliminados de todos los navegadores principales. SSLv2 y SSLv3 siempre deben estar deshabilitados.

Si la interoperabilidad con clientes en fin de vida útil (*end-of-life*) es un requisito estricto del negocio, aíslelos en un punto de enlace (*endpoint*) dedicado sin acceso a datos sensibles; no debilite el *endpoint* principal. Se debe habilitar la extensión "TLS_FALLBACK_SCSV" para evitar ataques de degradación de protocolo (*downgrade attacks*).

### Soportar Únicamente Cifrados Robustos

TLS soporta una gran cantidad de algoritmos de cifrado diferentes (o suites de cifrado / *cipher suites*) que proporcionan distintos niveles de seguridad. Siempre que sea posible, solo deben habilitarse los cifrados en modo GCM. Sin embargo, si es necesario dar soporte a clientes heredados, es posible que se requieran otros cifrados. Como mínimo, los siguientes tipos de cifrados siempre deben deshabilitarse:

* Cifrados nulos (*Null ciphers*)
* Cifrados anónimos (*Anonymous ciphers*)
* Cifrados de exportación (*EXPORT ciphers*)

La Fundación Mozilla proporciona un generador de configuraciones seguras fácil de usar para servidores web, bases de datos y correo electrónico. Esta herramienta permite a los administradores de sistemas seleccionar el software que están utilizando y recibir un archivo de configuración optimizado para equilibrar la seguridad y la compatibilidad en una amplia variedad de versiones de navegadores y software de servidor.

### Establecer los Grupos Diffie-Hellman Adecuados

La práctica de las versiones de protocolo anteriores a TLS 1.3 para la generación de parámetros Diffie-Hellman —utilizados por el intercambio de claves efímeras Diffie-Hellman (identificado por las cadenas "DHE" o "EDH" en el nombre de la suite de cifrado)— presentaba problemas prácticos. Por ejemplo, el cliente no tenía intervención en la selección de los parámetros del servidor, lo que significaba que solo podía aceptarlos incondicionalmente o descartar la conexión, y la generación de parámetros aleatorios a menudo derivaba en ataques de denegación de servicio (CVE-2022-40735, CVE-2002-20001).

TLS 1.3 restringe los parámetros de los grupos Diffie-Hellman a grupos conocidos a través de la extensión `supported_groups`. Los grupos Diffie-Hellman disponibles son ffdhe2048, ffdhe3072, ffdhe4096, ffdhe6144, ffdhe8192, según lo especificado en el RFC 7919.

Por defecto, OpenSSL 3.0 habilita todos los grupos anteriores. Para modificarlos, asegúrese de que los parámetros correctos del grupo Diffie-Hellman estén presentes en `openssl.cnf`. Por ejemplo:

```ini
openssl_conf = openssl_init
[openssl_init]
ssl_conf = ssl_module
[ssl_module]
system_default = tls_system_default
[tls_system_default]
Groups = x25519:prime256v1:x448:ffdhe2048:ffdhe3072

```

Una configuración de Apache se vería de la siguiente manera:

```apache
SSLOpenSSLConfCmd Groups x25519:secp256r1:ffdhe3072

```

El mismo grupo en NGINX se configuraría de la siguiente forma:

```nginx
ssl_ecdh_curve x25519:secp256r1:ffdhe3072;

```

Para TLS 1.2 o versiones anteriores, se recomienda no establecer parámetros Diffie-Hellman.

### Deshabilitar la Compresión

La compresión TLS debe deshabilitarse para proteger al sistema contra una vulnerabilidad (denominada CRIME) que potencialmente podría permitir a un atacante recuperar información sensible, como las cookies de sesión.

### Aplicar Parches a las Bibliotecas Criptográficas

Además de las vulnerabilidades en los protocolos SSL y TLS, históricamente han existido un gran número de vulnerabilidades en las bibliotecas de SSL y TLS, siendo *Heartbleed* la más conocida. Por lo tanto, es fundamental garantizar que estas bibliotecas se mantengan actualizadas con los últimos parches de seguridad.

### Probar la Configuración del Servidor

Una vez que el servidor ha sido bastionado (*hardened*), se debe verificar la configuración. El capítulo sobre pruebas de SSL/TLS de la Guía de Pruebas de OWASP (*OWASP Testing Guide*) contiene más información al respecto.

Existen varias herramientas en línea que se pueden utilizar para validar rápidamente la configuración de un servidor, entre ellas:

* SSL Labs Server Test
* CryptCheck
* Hardenize
* ImmuniWeb
* Observatory by Mozilla
* Scanigma
* Stellastra
* OWASP PurpleTeam (nube)

Adicionalmente, se pueden utilizar diversas herramientas instalables (*offline*):

* O-Saft - OWASP SSL advanced forensic tool
* CipherScan
* CryptoLyzer
* SSLScan - Fast SSL Scanner
* SSLyze
* testssl.sh - Testing any TLS/SSL encryption
* tls-scan
* OWASP PurpleTeam (local)

## Certificados

### Utilizar Claves Robustas y Protegerlas

La clave privada utilizada para generar la clave de cifrado debe ser lo suficientemente robusta para la vida útil prevista de la propia clave privada y de su certificado correspondiente. La mejor práctica actual es seleccionar un tamaño de clave de al menos 2048 bits. Se puede encontrar información adicional sobre la vida útil de las claves y las fortalezas comparativas de las mismas en las directrices NIST SP 800-57.

La clave privada también debe protegerse contra accesos no autorizados mediante permisos del sistema de archivos y otros controles técnicos y administrativos.

### Utilizar Algoritmos de Hash Criptográficos Robustos

Los certificados deben utilizar SHA-256 como algoritmo de hash, en lugar de los algoritmos más antiguos MD5 y SHA-1. Estos últimos presentan múltiples debilidades criptográficas y los navegadores modernos no confían en ellos.

### Utilizar Nombres de Dominio Correctos

El nombre de dominio (o *subject*) del certificado debe coincidir con el nombre de dominio completamente calificado (FQDN) del servidor que presenta el certificado. Históricamente, esto se almacenaba en el atributo `commonName` (CN) del certificado. Sin embargo, las versiones modernas de Chrome ignoran el atributo CN y requieren que el FQDN se encuentre en el atributo `subjectAlternativeName` (SAN). Por razones de compatibilidad, los certificados deben incluir el FQDN principal en el CN y la lista completa de FQDNs en el SAN.

Además, al crear el certificado, se debe tener en cuenta lo siguiente:

* Evalúe si el subdominio "www" también debe ser incluido.
* No incluya nombres de host no calificados (*non-qualified hostnames*).
* No incluya direcciones IP.
* No incluya nombres de dominio internos en certificados expuestos hacia el exterior. Si un servidor es accesible tanto por FQDN internos como externos, configúrelo con múltiples certificados.

### Evaluar Cuidadosamente el Uso de Certificados Comodín (*Wildcard*)

Los certificados comodín pueden ser convenientes; sin embargo, violan el principio de mínimo privilegio, ya que un único certificado es válido para todos los subdominios de un dominio (como `*.example.org`). Cuando múltiples sistemas comparten un certificado comodín, aumenta la probabilidad de que la clave privada del certificado se vea comprometida, dado que la clave puede estar presente en múltiples servidores. Además, el valor de esta clave aumenta significativamente, convirtiéndola en un objetivo más atractivo para los atacantes.

La problemática en torno al uso de certificados comodín es compleja y existen diversos debates al respecto en línea. Al realizar un análisis de riesgos sobre el uso de certificados comodín, se deben considerar los siguientes aspectos:

* Utilice certificados comodín solo cuando exista una necesidad real del negocio, y no por mera conveniencia. Evalúe el uso del protocolo ACME para permitir que los sistemas soliciten y actualicen automáticamente sus propios certificados.
* Nunca utilice un certificado comodín para sistemas que tengan diferentes niveles de confianza.
* Dos pasarelas VPN (*VPN gateways*) podrían utilizar un certificado comodín compartido.
* Múltiples instancias de una misma aplicación web podrían compartir un certificado.
* Una pasarela VPN y un servidor web público **no** deberían compartir un certificado comodín.
* Un servidor web público y un servidor interno **no** deberían compartir un certificado comodín.


* Considere el uso de un servidor proxy de reversa que realice la terminación TLS, de modo que la clave privada del certificado comodín solo esté presente en un único sistema.
* Se debe mantener una lista de todos los sistemas que comparten un certificado para permitir la actualización de todos ellos en caso de que el certificado expire o se comprometa.
* Limite el alcance de un certificado comodín emitiéndolo para un subdominio específico (como `*.foo.example.org`) o para un dominio separado.

### Utilizar una Autoridad de Certificación Adecuada para la Base de Usuarios de la Aplicación

Para que los usuarios confíen en ellos, los certificados deben estar firmados por una Autoridad de Certificación (CA) de confianza. Para aplicaciones orientadas a Internet, esta debe ser una de las CAs reconocidas y aceptadas automáticamente por los sistemas operativos y navegadores.

La CA *Let's Encrypt* proporciona certificados SSL gratuitos con validación de dominio (DV), los cuales son aceptados por todos los navegadores principales. Por lo tanto, evalúe si existe algún beneficio real en comprar un certificado de una CA comercial.

Para aplicaciones internas, se puede utilizar una CA interna. Esto significa que el FQDN del certificado no quedará expuesto (ni ante una CA externa, ni públicamente en las listas de transparencia de certificados o *Certificate Transparency*). Sin embargo, el certificado solo será confiable para los usuarios que hayan importado y marcado como confiable el certificado de la CA interna utilizado para firmarlos.

### Utilizar Registros CAA para Restringir qué CAs Pueden Emitir Certificados

Los registros DNS de Autorización de la Autoridad de Certificación (CAA) se pueden utilizar para definir qué CAs tienen permitido emitir certificados para un dominio. Los registros contienen una lista de CAs autorizadas, y cualquier CA que no esté incluida en esa lista debería rechazar la emisión de un certificado para ese dominio. Esto puede ayudar a prevenir que un atacante obtenga certificados no autorizados para un dominio a través de una CA menos reputada. Cuando se aplica a todos los subdominios, también es útil desde una perspectiva administrativa, ya que limita qué CAs pueden utilizar los administradores o desarrolladores, evitando que obtengan certificados comodín no autorizados.

### Considerar el Tipo de Validación del Certificado

Los certificados se presentan en diferentes tipos de validación. La validación es el proceso que utiliza la Autoridad de Certificación (CA) para asegurarse de que usted tiene permitido poseer el certificado (Autorización). El *CA/Browser Forum* es una organización compuesta por CAs y proveedores de navegadores, además de otras entidades interesadas en la seguridad web. Ellos establecen las reglas que las CAs deben seguir según el tipo de validación.

La validación base se denomina **Validación de Dominio (DV)**. Todos los certificados emitidos públicamente deben ser validados por dominio. Este proceso implica una prueba práctica de control sobre el nombre o punto de enlace solicitado en el certificado. Por lo general, implica un mecanismo de desafío y respuesta (*challenge-response*) en el DNS, hacia una dirección de correo electrónico oficial o hacia el *endpoint* que recibirá el certificado.

Los certificados con **Validación de Organización (OV)** incluyen la información de la organización del solicitante en el *subject* del certificado (por ejemplo: `C = GB, ST = Manchester, O = Sectigo Limited, CN = sectigo.com`). El proceso para adquirir un certificado OV requiere un contacto oficial con la empresa solicitante a través de un método que demuestre a la CA que realmente están hablando con la empresa correcta.

Los certificados de **Validación Extendida (EV)** proporcionan un nivel aún más alto de verificación, además de todas las validaciones de DV y OV. Esto puede verse funcionalmente como la diferencia entre "Este sitio realmente está gestionado por Example Company Inc." frente a "Este dominio realmente es example.org". (Consulte las últimas directrices de Validación Extendida).

Históricamente, estos se mostraban de manera diferente en el navegador, a menudo exhibiendo el nombre de la empresa o un icono/fondo verde en la barra de direcciones. Sin embargo, a partir de 2019, ningún navegador principal muestra el estado EV de esta manera, ya que no consideran que los certificados EV proporcionen ninguna protección adicional (abarcando Chromium, Chrome, Edge, Brave, Opera, Firefox y Safari).

Dado que todos los navegadores y arquitecturas TLS no diferencian la seguridad técnica entre certificados DV, OV y EV, estos son efectivamente iguales en términos de seguridad. Un atacante solo necesita alcanzar el nivel de control práctico del dominio para obtener un certificado fraudulento. El trabajo adicional que requiere un atacante para obtener un certificado OV o EV no incrementa en absoluto el alcance de un incidente; de hecho, esas acciones probablemente facilitarían su detección. Las dificultades adicionales para obtener certificados OV y EV pueden crear un riesgo de disponibilidad, por lo que su uso debe ser evaluado bajo este criterio.

## Aplicación

### Utilizar TLS en Todas las Páginas

Se debe utilizar TLS en todas las páginas, no solo en aquellas que se consideran sensibles, como la página de inicio de sesión. Si existen páginas que no imponen el uso de TLS, podrían brindar a un atacante la oportunidad de interceptar (*sniffing*) información sensible como los tokens de sesión, o de inyectar JavaScript malicioso en las respuestas para llevar a cabo otros ataques contra el usuario.

Para aplicaciones públicas, puede ser adecuado que el servidor web escuche conexiones HTTP no cifradas en el puerto 80 y luego las redireccione inmediatamente mediante un redireccionamiento permanente (HTTP 301) para proporcionar una mejor experiencia a los usuarios que escriben manualmente el nombre de dominio. Esto debe reforzarse con la cabecera *HTTP Strict Transport Security* (HSTS) para evitar que accedan al sitio a través de HTTP en el futuro.

Los puntos de enlace que son exclusivamente APIs deben deshabilitar HTTP por completo y solo soportar conexiones cifradas. Cuando esto no sea posible, los *endpoints* de la API deberían rechazar las solicitudes realizadas a través de conexiones HTTP no cifradas en lugar de redireccionarlas.

### No Mezclar Contenido TLS y No-TLS (Contenido Mixto)

Una página que está disponible a través de TLS no debe incluir ningún recurso (como archivos JavaScript o CSS) que se cargue a través de HTTP no cifrado. Estos recursos inseguros podrían permitir a un atacante interceptar cookies de sesión o inyectar código malicioso en la página. Los navegadores modernos también bloquearán los intentos de cargar contenido activo a través de HTTP no cifrado en páginas seguras.

### Utilizar el Flag "Secure" en las Cookies

Todas las cookies deben marcarse con el atributo `Secure`, el cual instruye al navegador a enviarlas únicamente a través de conexiones HTTPS cifradas, con el fin de evitar que sean interceptadas desde una conexión HTTP no cifrada. Esto es importante incluso si el sitio web no escucha en HTTP (puerto 80), ya que un atacante que realice un ataque activo de tipo *Man-in-the-Middle* (MitM) podría presentar un servidor web falso en el puerto 80 al usuario para robar su cookie.

### Prevenir el Almacenamiento en Caché de Datos Sensibles

Aunque TLS proporciona protección de los datos mientras están en tránsito, no ofrece ninguna protección una vez que estos han llegado al sistema solicitante. Como tal, esta información puede almacenarse en la caché del navegador del usuario o en cualquier proxy de interceptación que esté configurado para realizar el descifrado TLS.

Cuando se devuelvan datos sensibles en las respuestas, se deben utilizar cabeceras HTTP para instruir al navegador y a los servidores proxy que no almacenen la información en caché, evitando así que sea guardada o devuelta a otros usuarios. Esto se puede lograr configurando las siguientes cabeceras HTTP en la respuesta:

```http
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
Expires: 0

```

### Utilizar HSTS (*HTTP Strict Transport Security*)

*HTTP Strict Transport Security* (HSTS) instruye al navegador del usuario a solicitar siempre el sitio a través de HTTPS, y también evita que el usuario ignore las advertencias de los certificados. Consulte la Guía de Referencia de HSTS (*HTTP Strict Transport Security Cheat Sheet*) para obtener más información sobre su implementación.

## Certificados de Cliente y Mutual TLS (mTLS)

In una configuración TLS típica, un certificado en el servidor permite al cliente verificar la identidad del servidor y proporciona una conexión cifrada entre ellos. Sin embargo, este enfoque presenta dos debilidades principales:

1. El servidor carece de un mecanismo intrínseco para verificar la identidad del cliente.
2. Un atacante que obtenga un certificado válido para el dominio puede interceptar la conexión. Esta interceptación es utilizada a menudo por las empresas para inspeccionar el tráfico TLS mediante la instalación de un certificado de CA de confianza en sus sistemas cliente.

Los certificados de cliente, fundamentales para *Mutual TLS* (mTLS), abordan estos problemas. En mTLS, tanto el cliente como el servidor se autentican mutuamente utilizando TLS. El cliente demuestra su identidad al servidor con su propio certificado. Esto no solo permite una autenticación robusta del cliente, sino que también evita que un intermediario descifre el tráfico TLS, incluso si cuenta con un certificado de CA de confianza instalado en el sistema cliente.

### Desafíos y Consideraciones

Los certificados de cliente rara vez se utilizan en sistemas públicos debido a varios desafíos:

* La emisión y gestión de certificados de cliente implica una carga administrativa significativa.
* A los usuarios no técnicos les puede resultar difícil instalar certificados de cliente.
* Las prácticas de descifrado TLS de las organizaciones pueden provocar que la autenticación por certificado de cliente (un componente clave de mTLS) falle.

A pesar de estos desafíos, los certificados de cliente y mTLS deben ser considerados para aplicaciones o APIs de alto valor, particularmente donde los usuarios son técnicamente avanzados o forman parte de la misma organización.

## Anclaje de Clave Pública (*Public Key Pinning*)

El anclaje de clave pública se puede utilizar para garantizar que el certificado del servidor no solo sea válido y confiable, sino que también coincida exactamente con el certificado esperado para dicho servidor. Esto proporciona protección contra un atacante que sea capaz de obtener un certificado válido, ya sea explotando una debilidad en el proceso de validación, comprometiendo una autoridad de certificación de confianza o teniendo acceso administrativo al cliente.

El anclaje de clave pública se implementó en los navegadores mediante el estándar HPKP (*HTTP Public Key Pinning*). Sin embargo, debido a una serie de inconvenientes operacionales, posteriormente se declaró obsoleto y ya no es recomendado ni soportado por los navegadores modernos.

No obstante, el anclaje de clave pública aún puede proporcionar beneficios de seguridad para aplicaciones móviles, clientes pesados (*thick clients*) y comunicación de servidor a servidor (M2M). Esto se analiza con más detalle en la Guía de Referencia de Anclaje de Certificados (*Pinning Cheat Sheet*).

---

## Artículos Relacionados (Referencias)

* OWASP - Testing for Weak TLS
* OWASP - Application Security Verification Standard (ASVS) - Communication Security Verification Requirements (V9)
* Mozilla - Mozilla Recommended Configurations
* NIST - SP 800-52 Rev. 2 Guidelines for the Selection, Configuration, and Use of Transport Layer Security (TLS) Implementations
* NIST - NIST SP 800-57 Recommendation for Key Management, Revision 5
* NIST - SP 800-95 Guide to Secure Web Services
* IETF - RFC 5280 Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile
* IETF - RFC 2246 The Transport Layer Security (TLS) Protocol Version 1.0 (JAN 1999)
* IETF - RFC 4346 The Transport Layer Security (TLS) Protocol Version 1.1 (APR 2006)
* IETF - RFC 5246 The Transport Layer Security (TLS) Protocol Version 1.2 (AUG 2008)
