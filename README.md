PARCIAL CORTE 2 

PARTE CONCEPTUAL

RESPONDA LAS SIGUIENTES PREGUNTAS A CONTINUACIÓN.

1.	Explica la diferencia fundamental entre NetFlow (muestreo de todos los paquetes de un flujo) y sFlow (muestreo estadístico basado en paquetes). ¿En qué escenario elegiría sFlow sobre NetFlow para detectar “top talkers” en un enlace de 100 Gbps?
Respuesta:
•	NetFlow: Es un protocolo de monitoreo de tráfico de red que recopila y exporta información detallada sobre el tráfico, como la dirección IP de origen, destino, los puertos, el protocolo y la cantidad de bytes y paquetes transmitidos. En este caso, se captura información completa de cada flujo, proporcionando un análisis exhaustivo de los flujos de tráfico en la red. 
•	sFlow: A diferencia de NetFlow, sFlow es un protocolo de muestreo estadístico que captura solo una pequeña muestra representativa del tráfico de la red. Esto reduce el volumen de datos capturados y es menos intensivo en cuanto a los recursos del sistema. sFlow ofrece un muestreo aleatorio de los paquetes para obtener métricas generales de tráfico sin tener que examinar cada flujo. 
•	Escenario de uso de sFlow sobre NetFlow: Para enlaces de 100 Gbps, utilizar sFlow podría ser más adecuado que NetFlow debido al alto volumen de tráfico. Dado que sFlow solo muestrea una pequeña fracción del tráfico, es más eficiente y menos demandante en términos de procesamiento y almacenamiento. Además, para detectar “top talkers” (los principales emisores de tráfico), sFlow sería una opción viable, ya que proporciona un análisis estadístico eficiente, suficiente para detectar los emisores más importantes sin requerir el procesamiento de todo el tráfico.

2.	Describe los 5 campos que componen la 5-tupla en un flujo NetFlow. Si se desea medir el consumo de ancho de banda por aplicación (ej. HTTP vs. SSH), ¿qué puerto(s) debe inspeccionar el colector?
Respuesta:
La 5-tupla en un flujo NetFlow está compuesta por los siguientes cinco campos:
•	Dirección IP de origen: La dirección IP de la máquina que está enviando los paquetes. 
•	Dirección IP de destino: La dirección IP de la máquina que está recibiendo los paquetes. 
•	Puerto de origen: El puerto de la aplicación en la máquina que envía los paquetes. 
•	Puerto de destino: El puerto de la aplicación en la máquina que recibe los paquetes. 
•	Protocolo: El protocolo de transporte utilizado (por ejemplo, TCP, UDP, etc.). 
Para medir el consumo de ancho de banda por aplicación, como HTTP o SSH, el colector debe inspeccionar los puertos de destino y puertos de origen. Los puertos comunes para estas aplicaciones son:
•	HTTP: Puerto 80 (para tráfico web no cifrado) y 443 (para tráfico web cifrado). 
•	SSH: Puerto 22 (para acceso remoto a través de SSH).

3.	En un router Cisco con IP Accounting activado, se observa la siguiente salida:
Respuesta:

<img width="406" height="138" alt="Captura de pantalla 2026-04-27 210041" src="https://github.com/user-attachments/assets/1da8dde1-6938-49ca-aeda-dd942922913d" />

Interprete estos datos. ¿Qué indicaría una asimetría extrema entre paquetes enviados y recibidos entre 192.168.1.10 y 10.0.0.5?
En la salida proporcionada, se observa una asimetría extrema entre los paquetes enviados y recibidos entre los dispositivos 192.168.1.10 y 10.0.0.5. Aquí se puede interpretar lo siguiente:
•	Desde 192.168.1.10 hacia 10.0.0.5, se enviaron 1500 paquetes y 120000 bytes. Esto indica que 192.168.1.10 está enviando una cantidad considerable de tráfico a 10.0.0.5. 
•	Desde 10.0.0.5 hacia 192.168.1.10, solo se enviaron 50 paquetes y 4000 bytes, lo que es significativamente menor. 
Esta asimetría podría indicar que 10.0.0.5 no está respondiendo adecuadamente a las solicitudes de 192.168.1.10, o que 192.168.1.10 está generando un tráfico de salida mucho mayor en comparación con el tráfico de entrada. Esto podría deberse a un tráfico unidireccional, como en el caso de un cliente que envía grandes cantidades de datos sin recibir una respuesta proporcional, o podría reflejar un error en la configuración de red o en el servicio en uno de los dos dispositivos.

2.A.) DIAGRAMA O ESQUEMA:

<img width="1105" height="615" alt="image" src="https://github.com/user-attachments/assets/efd1a211-b6a9-4f9f-bd9f-c50fb1ce4d40" />

<img width="1099" height="622" alt="image" src="https://github.com/user-attachments/assets/250f04d7-6edb-4d1c-9e00-9eea4c27da28" />

<img width="1103" height="620" alt="image" src="https://github.com/user-attachments/assets/9e11c872-a9ec-47b9-97e0-2014515105c9" />

DESCRIPCIÓN - RESPUESTA PREGUNTAS.

Sistema de monitoreo de tráfico de detecciones YOLO usando NetFlow.

1). Arquitectura:
 
Contenedor YOLO
Router virtual
VM colectora
Dashboard
 
2). Tecnologías:
 
Docker
iptables / nftables
NetFlow (softflowd)
Wireshark / ntopng
 
3). Configuración paso a paso:
 
Crear red Docker
Levantar contenedor
Configurar router
Activar NetFlow
Configurar colector
 
4). Pruebas:
 
iperf3
captura con Wireshark
ver flujos en dashboard
 
5). Resultados esperados:
 
Tráfico visible en NetFlow
Métricas de detección
Análisis de ancho de banda


2.B.) 

DIAGRAMA:

<img width="1103" height="1044" alt="image" src="https://github.com/user-attachments/assets/b4a35826-bfe3-47b5-8d86-5efa5736e319" />


Ø	DESCRIPCIÓN DE LA ARQUITECTURA  -  RESPUESTA PREGUNTAS.
 
Cada cámara transmite video por RTSP:  Cámaras (C1–C5)
 
·	C1 → YOLO + OCR (placas)
·	C2 → ocupación parqueadero
·	C3 → conteo de personas
·	C4 → animales
·	C5 → objetos abandonados
 
Ø	SERVIDOR EDGE (DOCKER):
 
·	Ejecuta 5 contenedores independientes
·	Cada uno con su modelo YOLO especializado
·	Ventajas:
·	Aislamiento
·	Escalabilidad
·	Procesamiento en tiempo real
·	Ejemplo de red Docker: docker network create yolo_net

 
  FLUJO DE DATOS:
 
·	Video procesado (opcional)
·	Metadata (JSON):
·	tipo de objeto
·	coordenadas
·	timestamp
·	Ejemplo: Cámara → YOLO → Video anotado + metadata → Servidor central

 
  RED SEGMENTADA: VLANS

<img width="291" height="194" alt="Captura de pantalla 2026-04-27 210524" src="https://github.com/user-attachments/assets/aef05130-2901-474d-9b8a-8673933d49a5" />


Ø	MONITOREO CON NETFLOW:
 
En el router L3 (core)
 
ip flow-export destination 192.168.1.50 2055
ip flow-export version 9
 
 
Interfaces monitoreadas;
 
interface GigabitEthernet0/0
ip flow ingress
ip flow egress
 
Lo que se mide;

·	Tráfico de cámaras → YOLO
·	YOLO → servidores
·	Consumo por aplicación
·	Ancho de banda por VLAN

 
Ø	IP ACCOUNTING (ALTERNATIVA A NETFLOW):
 
iptables -A FORWARD -s 10.10.10.0/24 -d 10.20.20.0/24 -j ACCEPT
iptables -L -v -n
Se pude visualizar;
·	bytes 
·	paquetes 
·	flujos entre subredes
 
Ø	CALIDAD DE SERVICIO (QOS)

            Clasificación de tráfico;
·	Alta prioridad: RTSP (video en tiempo real)
·	Media: metadata YOLO
·	Baja: tráfico administrativo
 
·	Ejemplo: 
 
·	class-map VIDEO
·	 match protocol rtsp
 
 
·	policy-map QOS-YOLO
·	 class VIDEO
·	  priority percent 40
·	 class class-default
·	  fair-queue
 
Ø	REDUNDANCIA:
 
·	Servidores centrales: Cluster activo/pasivo o balanceado
·	Red: Enlaces redundantes (LACP), STP habilitado
·	Datos: Replicación (base de datos o almacenamiento)
 
Ø	GARANTÍAS DEL DISEÑO
 
·	Alta disponibilidad
·	Escalabilidad (más cámaras o modelos)
·	Monitoreo completo (NetFlow + IP accounting)
·	Baja latencia (procesamiento en edge)
·	Seguridad (segmentación VLAN)
 
Ø	CONCLUSIÓN:
 
·	La arquitectura propuesta utiliza un servidor edge con contenedores Docker ejecutando modelos YOLO especializados, conectados a través de una red segmentada por VLANs hacia un core L3 que implementa QoS y exportación NetFlow. Los datos procesados se envían a servidores centrales redundantes, mientras que el tráfico es monitoreado mediante NetFlow/IP accounting, garantizando visibilidad, rendimiento y alta disponibilidad del sistema.

	PARTE EMPÍRICA:









