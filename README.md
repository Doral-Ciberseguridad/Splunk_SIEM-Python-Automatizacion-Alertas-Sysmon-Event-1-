Este repositorio incluye 2 archivos:

<br>

**1.Sysmon Evento 1 - Mi top reglas de detección Splunk.pdf**:

En este PDF teórico selecciono y explico las reglas de detección comunitarias que yo considero que son más relevantes basadas en Sysmon Event ID 1 (creación de procesos Windows), priorizando aquellas que permiten identificar ejecuciones sospechosas, actividades maliciosas y técnicas de ataque. El objetivo de este PDF es recopilar las reglas de detección comunitarias que implementaré como alertas individuales mediante mi script en Python.

<br>
<br>

**2.splunk-alertas-sysmon1.py**

Este es mi script en Python que automatiza la creación de alertas en Splunk a partir de las consultas SPL seleccionadas, configurando su monitorización y el envío de notificaciones por correo electrónico cuando se detectan eventos que coinciden con las reglas.

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

4.Ejecutar el script python:

```
python splunk_automatizar_creacion_alertas.py
```

El script registra en Splunk las reglas de detección y sus consultas SPL como alertas individuales, automatizando su creación y configuración:

![[Pasted image 20261020224618.png]]

5.Dirigete a > Splunk Server > Search & Reporting > Settings > Searches, Reports, and Alerts
Comprueba que las alertas se hayan creado correctamente:

![[Pasted image 20261020220545.png]]

![[Pasted image 20261020220930.png]]

6.Provoca

<img width="906" height="1147" alt="image" src="https://github.com/user-attachments/assets/4c3cea61-ca3e-454f-8059-11e509bc807f" />

