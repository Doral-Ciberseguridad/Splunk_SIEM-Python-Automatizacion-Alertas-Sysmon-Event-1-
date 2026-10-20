Este repositorio incluye 2 archivos:

<br>

**1.Sysmon Evento 1 - Mi top reglas de detección Splunk.pdf**:

En este PDF teórico selecciono y explico las reglas de detección comunitarias que yo considero que son más relevantes basadas en Sysmon Event ID 1 (creación de procesos Windows), priorizando aquellas que permiten identificar ejecuciones sospechosas, actividades maliciosas y técnicas de ataque. El objetivo de este PDF es recopilar las reglas de detección comunitarias que implementaré como alertas individuales mediante mi script en Python.

<br>
<br>

**2.splunk-alertas-sysmon1.py**

Este es mi script en Python que automatiza la creación de alertas en Splunk a partir de las consultas SPL definidas en el archivo PDF anterior, configurando su monitorización y el envío de notificaciones por correo electrónico cuando se detectan eventos que coinciden con las reglas.

Pasos para ejecutar este script en Splunk Server:

1.Instalar dependencias necesarias:

```
sudo apt install python3-pip
```

```
sudo pip install splunk-sdk
```

```
sudo pip3 install splunklib
```

2.Crear el archivo python:

```
sudo nano splunk_automatizar_creacion_alertas.py
```

3.Copiar el contenido del archivo.py de este repositorio y pegarlo dentro. Guardar cambios con Ctrl+X.


4.Cambiar y ajustar el contenido de estas variables:

- USERNAME: usuario de Splunk. Ej.: "admin"
  
- PASSWORD: contraseña del usuario de Splunk.
  
- EMAIL: correo que recibirá las alertas. (Debes tener configurado previamente el envio de correos por SMTP en los ajustes de tu Splunk Server)
  
- HOST: dirección del servidor Splunk. Ej.: "localhost"
  
- PORT: puerto de la API de Splunk. Ej.: 8089
  
- OWNER: propietario de las alertas. Ej.: "admin"
  
- APP: aplicación de Splunk donde se guardarán las alertas. Ej.: "search"
  
- SHARING: ámbito de acceso a las alertas. Ej.: "user"


5.Ejecutar el script python:

```
python splunk_automatizar_creacion_alertas.py
```

Verás como empieza a añadir las alertas a Splunk una por una progresivamente:

<img width="858" height="670" alt="image" src="https://github.com/user-attachments/assets/bc7d71fe-52f0-4fc0-98a2-31349ab0c042" />

<br>

<img width="898" height="638" alt="image" src="https://github.com/user-attachments/assets/23f298be-23c1-415e-83f9-371a0d979a8b" />

<br>

<img width="854" height="517" alt="image" src="https://github.com/user-attachments/assets/76047c95-8f1d-4f95-8f52-88bb2c21da03" />

<br>


<br>

6.Dirigete a > Splunk Server > Search & Reporting > Settings > Searches, Reports, and Alerts
Comprueba que las alertas se hayan creado correctamente:

<img width="1056" height="810" alt="image" src="https://github.com/user-attachments/assets/67c23cd0-303a-4211-a16a-8c646333a43e" />

<br>

<img width="709" height="719" alt="image" src="https://github.com/user-attachments/assets/af359f89-a0d9-48ba-8543-7ea39bd0dfad" />



<br>

7.Dirijete a tu máquina Windows y provoca uno de los eventos de detección de Sysmon 1. Ejecuta este comando en Powershell:

```
Invoke-CimMethod -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine = "$env:windir\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe"}
```

Si todo ha salido bien, deberías de recibir una alerta por correo electrónico:

<img width="906" height="1147" alt="image" src="https://github.com/user-attachments/assets/4c3cea61-ca3e-454f-8059-11e509bc807f" />

Si no recibes ninguna alerta, asegurate bien de tener Sysmon habilitado y configurado en tu agente Windows.

