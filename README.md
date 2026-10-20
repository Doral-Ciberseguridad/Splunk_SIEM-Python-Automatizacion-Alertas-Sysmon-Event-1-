Este repositorio incluye 2 archivos:

<br>

**1.Sysmon Evento 1 - Mi top reglas de detección Splunk.pdf**:

En este PDF teórico selecciono y explico las reglas de detección comunitarias que yo considero que son más relevantes basadas en Sysmon Event ID 1 (creación de procesos Windows), priorizando aquellas que permiten identificar ejecuciones sospechosas, actividades maliciosas y técnicas de ataque. El objetivo de este PDF es recopilar las reglas de detección comunitarias que implementaré como alertas individuales mediante mi script en Python.

<br>
<br>

**2.splunk-alertas-sysmon1.py**

Este es mi script en Python que automatiza la creación de alertas en Splunk a partir de las consultas SPL definidas en el archivo PDF anterior, configurando su monitorización y el envío de notificaciones por correo electrónico cuando se detectan eventos que coinciden con las reglas. He explicado cada parte del código con un comentario.

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

También asegurate de tener las macros instaladas en Splunk Server:

```bash
# 1. Descargar Splunk Security Content
git clone --depth 1 https://github.com/splunk/security_content.git

# 2. Entrar en el repositorio y comprobar las macros
cd ~/Downloads/security_content
ls macros | head

# 3. Instalar las dependencias para entornos virtuales
sudo apt update
sudo apt install python3.14-venv -y

# 4. Crear el entorno virtual e instalar contentctl-ng
cd ~/Downloads/security_content
python3 -m venv .venv
source .venv/bin/activate
pip install contentctl-ng

# 5. Construir la aplicación de Security Content
cd ~/Downloads/security_content
source .venv/bin/activate
contentctl-ng build

# 6. Instalar la aplicación generada en Splunk
sudo /opt/splunk/bin/splunk install app /home/splunkserver/Downloads/security_content/dist/DA-ESS-ContentUpdate.tar.gz

# 7. Reiniciar Splunk
sudo /opt/splunk/bin/splunk restart --run-as-root

# 8. Comprobar la macro principal de Security Content
# Ejecutar en Search & Reporting:
# | rest /servicesNS/-/DA-ESS-ContentUpdate/admin/macros
# | search title="security_content_summariesonly"
# | table title

# 9. Comprobar las macros auxiliares
# Ejecutar en Search & Reporting:
# | rest /servicesNS/-/DA-ESS-ContentUpdate/admin/macros
# | search title IN ("summariesonly_config","oldsummaries_config","fillnull_config")
# | table title

# 10. Descargar Splunk Common Information Model (CIM)
cd ~/Downloads
wget -O splunk-common-information-model-cim_850.tgz "https://attack-range-appbinaries.s3.us-west-2.amazonaws.com/splunk-common-information-model-cim_850.tgz"

# 11. Instalar CIM
sudo /opt/splunk/bin/splunk install app /home/splunkserver/Downloads/splunk-common-information-model-cim_850.tgz

# 12. Reiniciar Splunk
sudo /opt/splunk/bin/splunk restart --run-as-root

# 13. Comprobar que CIM está instalado
ls -d /opt/splunk/etc/apps/Splunk_SA_CIM

# 14. Descargar el complemento de Sysmon
cd ~/Downloads
wget -O Splunk_TA_microsoft_sysmon.tgz "https://attack-range-appbinaries.s3.us-west-2.amazonaws.com/splunk-add-on-for-sysmon_500.tgz"

# 15. Comprobar si el complemento de Sysmon está instalado
sudo /opt/splunk/bin/splunk list app | grep -i sysmon
```
Esto es necesario para que las macros utilizadas por las consultas SPL de las reglas de detección estén disponibles en Splunk y puedan ejecutarse correctamente.



