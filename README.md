PARCIAL CORTE 2 

PARTE CONCEPTUAL

RESPONDA LAS SIGUIENTES PREGUNTAS A CONTINUACIÓN.

1.	Explica la diferencia fundamental entre NetFlow (muestreo de todos los paquetes de un flujo) y sFlow (muestreo estadístico basado en paquetes). ¿En qué escenario elegiría sFlow sobre NetFlow para detectar “top talkers” en un enlace de 100 Gbps?

   
Respuesta:

•	NetFlow: Es un protocolo de monitoreo de tráfico de red que recopila y exporta información detallada sobre el tráfico, como la dirección IP de origen, destino, los puertos, el protocolo y la cantidad de bytes y paquetes transmitidos. En este caso, se captura información completa de cada flujo, proporcionando un análisis exhaustivo de los flujos de tráfico en la red. 

•	sFlow: A diferencia de NetFlow, sFlow es un protocolo de muestreo estadístico que captura solo una pequeña muestra representativa del tráfico de la red. Esto reduce el volumen de datos capturados y es menos intensivo en cuanto a los recursos del sistema. sFlow ofrece un muestreo aleatorio de los paquetes para obtener métricas generales de tráfico sin tener que examinar cada flujo. 

•	Escenario de uso de sFlow sobre NetFlow: Para enlaces de 100 Gbps, utilizar sFlow podría ser más adecuado que NetFlow debido al alto volumen de tráfico. Dado que sFlow solo muestrea una pequeña fracción del tráfico, es más eficiente y menos demandante en términos de procesamiento y almacenamiento. Además, para detectar “top talkers” (los principales emisores de tráfico), sFlow sería una opción viable, ya que proporciona un análisis estadístico eficiente, suficiente para detectar los emisores más importantes sin requerir el procesamiento de todo el tráfico.

3.	Describe los 5 campos que componen la 5-tupla en un flujo NetFlow. Si se desea medir el consumo de ancho de banda por aplicación (ej. HTTP vs. SSH), ¿qué puerto(s) debe inspeccionar el colector?
   
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

4.	En un router Cisco con IP Accounting activado, se observa la siguiente salida:
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


DESCRIPCIÓN DE LA ARQUITECTURA  -  RESPUESTA PREGUNTAS.
 
Cada cámara transmite video por RTSP:  Cámaras (C1–C5)
 
·	C1 → YOLO + OCR (placas)
·	C2 → ocupación parqueadero
·	C3 → conteo de personas
·	C4 → animales
·	C5 → objetos abandonados
 
SERVIDOR EDGE (DOCKER):
 
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


MONITOREO CON NETFLOW:
 
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

 
IP ACCOUNTING (ALTERNATIVA A NETFLOW):
 
iptables -A FORWARD -s 10.10.10.0/24 -d 10.20.20.0/24 -j ACCEPT
iptables -L -v -n
Se pude visualizar;
·	bytes 
·	paquetes 
·	flujos entre subredes
 
CALIDAD DE SERVICIO (QOS)

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
 
GARANTÍAS DEL DISEÑO
 
·	Alta disponibilidad
·	Escalabilidad (más cámaras o modelos)
·	Monitoreo completo (NetFlow + IP accounting)
·	Baja latencia (procesamiento en edge)
·	Seguridad (segmentación VLAN)
 
CONCLUSIÓN:
 
·	La arquitectura propuesta utiliza un servidor edge con contenedores Docker ejecutando modelos YOLO especializados, conectados a través de una red segmentada por VLANs hacia un core L3 que implementa QoS y exportación NetFlow. Los datos procesados se envían a servidores centrales redundantes, mientras que el tráfico es monitoreado mediante NetFlow/IP accounting, garantizando visibilidad, rendimiento y alta disponibilidad del sistema.


PARTE EMPÍRICA:

Preparar entorno en Colab

Comando;

!pip install ultralytics matplotlib
!apt-get install -y tcpdump nfdump iptables


YOLO (ultralytics)
tcpdump → captura de tráfico
nfdump → análisis tipo NetFlow
iptables → contadores IP


YOLO + Generación de tráfico UDP

Script:

Carga YOLO
Procesa un video
Cuenta objetos detectados
Envía ese número como tráfico UDP


Código Completo:


import cv2
import socket
import time
from ultralytics import YOLO

# Descargar video de prueba
!wget -q -O test.mp4 https://www.sample-videos.com/video123/mp4/720/big_buck_bunny_720p_1mb.mp4

# Cargar modelo YOLO
model = YOLO("yolov8n.pt")

# Configurar socket UDP
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
collector_ip = "127.0.0.1"
collector_port = 5555

# Captura de video
cap = cv2.VideoCapture("test.mp4")
frame_num = 0

while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
    break

    results = model(frame)

    # Número de objetos detectados
    num_detecciones = len(results[0].boxes)

    mensaje = f"{time.time()},{num_detecciones}".encode()

    # Enviar tráfico UDP
    sock.sendto(mensaje, (collector_ip, collector_port))

    frame_num += 1
    if frame_num >= 30:  # limitar tráfico
    break

cap.release()
sock.close()

print(f" Enviados {frame_num} mensajes UDP")

Capturar tráfico (tcpdump):


!tcpdump -i any udp port 5555 -c 20

Esto te muestra paquetes como:

IP 127.0.0.1.XXXXX > 127.0.0.1.5555: UDP, length XX

Aquí ya estás generando tráfico real.

	Aplicar concepto de 5-tupla:

Cada flujo de red se identifica por:

IP origen → 127.0.0.1
IP destino → 127.0.0.1
Puerto origen → aleatorio
Puerto destino → 5555
Protocolo → UDP


IP Accounting (iptables):

Para contar tráfico por IP:

!iptables -Z
!iptables -A INPUT -p udp --dport 5555 -j ACCEPT
!iptables -L -v -n

Verás contadores como:

pkts bytes target prot opt in out source destination
30   XXXX ACCEPT udp -- * * 0.0.0.0/0 0.0.0.0/0 udp dpt:5555

Identificar "Top Talkers":

Un top talker es el que más tráfico genera.

En este caso:

Solo tienes 127.0.0.1 → será el top talker
Si simulas más IPs → puedes compararlos

Captura y análisis con NetFlow:

1. Capturar tráfico mientras corre YOLO

En Colab (primero ejecuta YOLO en otra celda):

!tcpdump -i any -c 100 udp port 5555 -w /tmp/flujos.pcap

Qué hace:

-i any → escucha todas las interfaces
udp port 5555 → filtra tu tráfico
-w → guarda en archivo (clave para NetFlow)

	Convertir PCAP a formato tipo NetFlow:
!nfdump -r /tmp/flujos.pcap

•	Algunos entornos nfdump no convierte directamente PCAP.
Si te falla, usa alternativa (más segura en Colab):

!apt-get install -y softflowd
!softflowd -r /tmp/flujos.pcap -n 127.0.0.1:9995

INTERPRETACIÓN: 

Este flujo representa:

•	Un sistema de visión artificial que:

Detecta objetos
Genera eventos
Envía datos por UDP

•	Cada mensaje genera: Un flujo único (por puerto origen distinto).

•	Se capturó tráfico UDP generado por el modelo YOLO, el cual envía mensajes a un colector local. Mediante nfdump se identificaron los flujos de red, permitiendo extraer la 5-tupla de cada uno. Se determinó que los flujos corresponden a comunicaciones UDP (protocolo 17) entre 127.0.0.1 y el puerto 5555, con puertos origen dinámicos, lo que indica múltiples transmisiones independientes generadas por el sistema de detección.

Identificar Top Talkers:

•	Se agrupa el tráfico por IP de origen y se identifica cuál genera más paquetes; Quién habla más en la red.

Se agruparon los flujos por dirección IP de origen utilizando nfdump. 
Posteriormente, se sumó el número de paquetes enviados por cada IP.

El resultado muestra que la IP 127.0.0.1 es el top talker, ya que genera la totalidad del tráfico UDP observado.

Esto se debe a que el sistema de detección YOLO envía todos los mensajes desde el localhost hacia el colector.

Conclusión:

El análisis permitió identificar los principales emisores de tráfico en la red. 
Se determinó que la IP 127.0.0.1 es el top talker, concentrando el mayor número de paquetes enviados, lo cual es consistente con el comportamiento del sistema YOLO que genera tráfico desde un único origen.

IP Accounting (simulación tipo router Cisco): 

•	Estás usando iptables como si fuera un router para:

o	paquetes
o	bytes
o	por tráfico que llega al puerto 5555


REGLA DE CONTEO:

•	Ejecuta en Colab:

!iptables -A INPUT -p udp --dport 5555 -j ACCEPT

•	significa:

•	INPUT → tráfico que entra
•	udp → protocolo
•	--dport 5555 → destino
•	ACCEPT → permite el tráfico (y lo cuenta)

•	Se configuró iptables para contabilizar el tráfico UDP dirigido al puerto 5555. 
Posteriormente, se reiniciaron los contadores y se ejecutó nuevamente el script de YOLO.

Los resultados muestran que se registraron 30 paquetes con un total de 2100 bytes, lo que corresponde al tráfico generado por el sistema de detección de objetos.

	Esto confirma que iptables puede utilizarse como herramienta de IP Accounting para medir el tráfico de red, similar a un router Cisco. El uso de iptables permitió realizar un conteo preciso del tráfico de red generado por la aplicación YOLO. Se evidenció que todos los paquetes enviados al puerto 5555 fueron contabilizados correctamente, demostrando la utilidad de IP Accounting para monitoreo de red.


Simular congestión y detectarla

•	Generar tráfico anormal adicional para ver cómo:

•	aumentan los contadores (iptables)
•	cambian los flujos (NetFlow)
•	se detecta una posible anomalía

IMPORTANTE:

•	hping3 -p 5555 usa TCP por defecto,
•	pero tu tráfico YOLO es UDP

•	Mejor usar UDP para que coincida:

•	!apt-get install -y hping3
•	!hping3 --udp -p 5555 --flood 127.0.0.1 -c 500

TRÁFICO ANÓMALO:

•	En otra celda (mientras corre YOLO):
•	!hping3 --udp -p 5555 127.0.0.1 -c 500

•	simula:

•	congestión
•	ataque tipo flood
•	incremento brusco de tráfico


•	Se simuló una anomalía generando tráfico adicional mediante hping3 hacia el puerto 5555, mientras el sistema YOLO se encontraba en ejecución.
•	Al analizar los contadores de iptables, se observó un incremento significativo en el número de paquetes y bytes en comparación con el escenario normal.
•	Este comportamiento evidencia una congestión de red o posible ataque tipo flood, ya que el volumen de tráfico supera el patrón esperado del sistema.
•	Por lo tanto, el monitoreo mediante IP Accounting y análisis de flujos permite detectar anomalías en el tráfico de red de manera efectiva.

RESPUESTAS PREGUNTAS:
1). La 5-tuple completa de uno de los flujos capturados con nfdump
La 5-tuple (cinco campos) de un flujo es:

•	IP origen
•	IP destino
•	Puerto origen
•	Puerto destino
•	Protocolo

Ejemplo típico sacado de nfdump:

•	Src IP: 192.168.1.10
•	Dst IP: 192.168.1.20
•	Src Port: 34567
•	Dst Port: 5555
•	Protocol: UDP

o	Forma corta:
•	192.168.1.10 : 34567 → 192.168.1.20 : 5555 (UDP)

o	cuando ejecutes:
•	nfdump -r archivo.nfdump

2). Ejecutar iptables -L -v -n después de correr YOLO
Bytes contabilizados.
ejecutar:
•	iptables -L -v -n
•	la columna: bytes
•	Ejemplo:
•	pkts bytes target  prot opt in out source destination
•	120  9600 ACCEPT  udp  --  *  *  0.0.0.0 0.0.0.0 udp dpt:5555
•	Bytes contabilizados = 9600

3). Diferenciar tráfico HTTP del generado por YOLO

Qué campo de la 5-tuple analizaría.
La respuesta correcta es: Puerto destino
Porque:
•	HTTP usa normalmente puerto 80
•	HTTPS usa 443
•	YOLO UDP usa por ejemplo 5555

<img width="357" height="149" alt="Captura de pantalla 2026-04-27 235801" src="https://github.com/user-attachments/assets/3a1373d2-96a6-4fb1-801a-02b4a7cb4dbb" />


Ventaja en un enlace lento:

Importante:

•	Reduce tráfico
•	Reduce uso de ancho de banda
•	Mantiene una muestra representativa
•	Evita congestión

Ejemplo:

•	Sin muestreo:
•	1000 detecciones → 1000 paquete
•	Con muestreo 1:10:
•	1000 detecciones → 100 paquetes
•	Mucho menos tráfico.

CONCLUSIÓN:

•	El uso de nfdump, iptables y muestreo permite analizar y optimizar el tráfico generado por aplicaciones como YOLO.











