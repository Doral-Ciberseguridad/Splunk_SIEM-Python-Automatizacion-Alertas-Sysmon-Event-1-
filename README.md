Este repositorio incluye 2 archivos:


**1.Sysmon Evento 1 - Mi top reglas de detección Splunk.pdf**:

En este PDF teórico selecciono y explico las reglas de detección comunitarias que yo considero que son más relevantes basadas en Sysmon Event ID 1 (creación de procesos), priorizando aquellas que permiten identificar ejecuciones sospechosas, actividades maliciosas y técnicas de ataque. El objetivo de este PDF es recopilar las reglas de detección comunitarias que implementaré como alertas individuales mediante mi script en Python.


**2.splunk-alertas-sysmon1.py**

Este es mi script en Python que automatiza la creación de alertas en Splunk a partir de las consultas SPL seleccionadas, configurando su monitorización y el envío de notificaciones por correo electrónico cuando se detectan eventos que coinciden con las reglas.
