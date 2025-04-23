# Vulnerabilidades de los servidores HTTP

A día de hoy, la mayoría de sitios web usan protocolo HTTPS. Sin embargo, sigue habiendo sitios web que en estos tiempos siguen usando protocolos HTTP, en especial los sitios de hospitales y colegios, lo cual es crítico debido a que estas webs manejan formularios y registros donde pasan datos confidenciales.

El objetivo de esta prueba es demostrar varias vulnerabilidades que tienen no solo los servidores HTTP sino también los mal configurados, ya que ser HTTPS no es del todo seguro, esto solo encripta información, pero no significa que no sea vulnerable o que un sitio no tenga malware, pero esto último ya es otro tema.

Para esta demostración hice un HTML muy básico que simula un formulario, aunque no es real ya que para que sea funcional necesitaría integración con JS, ya con lo que muestra el HTML se entiende la idea.

![Ejemplo de formulario en HTML](/HTTPSERVER-VULNS/html.png)

Para mostrar como un servidor se configura mal esta página la serví simplemente usando el comando 

`python3 -m http.server 8080`

Que crea servidores demasiados básicos y sin configuración alguna, así ya tenía el servidor corriendo en mi red.

Así, la primera vulnerabilidad es: Escaneos agresivos o ataques DDoS.

Este servidor no cuenta con protección de algún firewall que impida el tráfico masivo de un dispositivo, por lo que si un atacante hace escaneos con nmap o intenta tumbar la página con un ataque de denegación de servicio, lo logrará y además de poder tumbar la web puede obtener información del sistema del servidor como se ve en este escaneo con nmap:

![Escaneo con Nmap](/HTTPSERVER-VULNS/nmap.png)

Como se puede ver incluso detectó la vulnerabilidad que confirma que se pueden hacer ataques DOS Slowloris, no solo eso también usé la herramienta Nikto para detectar otras vulnerabilidades web y detecté la segunda vulnerabilidad: Clickjacking.

![Escaneo con Nikto](/HTTPSERVER-VULNS/nikto.png)

## Pero, ¿qué es exactamente el clickjacking?

Es un método usado por atacantes, que se aprovecha cuando una web no tiene la configuración ideal para evitar que sea vista de un iframe de otra web, esta vulnerabilidad es aprovechada para montar nuevos servidores por atacantes para distribuirlas como autenticas y así poder interceptar los datos que los usuarios ingresen a la web original. Así como en este ejemplo donde se demuestra la vulnerabilidad:

![Vulnerabilidad clickjacking](/HTTPSERVER-VULNS/clickjacking.png)

La tercera vulnerabilidad es muy obvia y no requiere un análisis mayor y es: intercepción de datos HTTP

Esta vulnerabilidad se debe a que la página al no tener certificados SSL en el servidor, un atacante puede ver los datos que pasan en texto plano usando programas como wireshark, aunque como en esta simulación no se usó un formulario real, no hay datos que interceptar, sin embargo un atacante si puede ver información de quienes ingresan:

![Intercepción con Wireshark](/HTTPSERVER-VULNS/wireshark.png)

Aunque proteger una web implique costos, no hay nada más sencillo y más económico que adquirir los certificados con OpenSSL para poder implementar HTTPS en la web, que aunque estos certificados sean autofirmados y los navegadores puedan lanzar alertas por esta misma razón, ayuda a cifrar la conexión y a dificultar la intercepción de datos. Para evitar vulnerabilidades como el Clickjacking o el DDoS esto si son configuraciones más avanzadas en el servidor pero que son posibles incluso en pequeños servidores que ni siquiera corresponden a grandes corporaciones.

## Importante 

Todo este procedimiento se hizo en un entorno controlado y en una red local propia, la intercepción de datos no autorizada, los ataques DDoS y escaneos de redes no propias conlleva a problemas legales, si desea imitar estos laboratorios, asegurese de no afectar a los demás y hacerlo bajo su propia responsabilidad, estos laboratorios solo se hacen con fines educativos e informativos.