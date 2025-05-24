# sift-workstation-display-guide
Guía para configurar resolución completa y entorno gráfico funcional en SIFT Workstation con VMware y VirtualBox.

# Guía para configurar resolución completa en SIFT Workstation (VirtualBox y VMware)

Esta guía resuelve un problema muy común: la baja resolución de pantalla o falta de redimensionado automático al iniciar **SIFT Workstation**, una distribución forense basada en Ubuntu. Aplica tanto a **VirtualBox** como a **VMware Workstation/Player**.

---

## 🧰 ¿Qué es SIFT y por qué pasa esto?

**SIFT Workstation** es una distribución de SANS para análisis forense. Al iniciarla en una VM, muchas veces presenta:

* Resolución fija (800x600)
* Bordes negros
* No responde al cambio de tamaño de ventana
* Modo pantalla completa con bordes o escalado borroso

Esto ocurre porque **Ubuntu 22.04 usa Wayland por defecto**, y no se lleva bien con VirtualBox ni VMware.

---

## ⚙️ Parte 1: Configuración en VirtualBox

### 1. Importar la OVA

* En VirtualBox: `Archivo > Importar servicio virtualizado`
* Seleccioná el archivo `.ova` de SIFT (por ejemplo, `sift-22.04-20240201.ova`)

### 2. Configurar hardware

* CPU: 2 o más
* RAM: 4 GB mínimo
* Video: 128 MB
* **Controlador gráfico: VMSVGA**
* Activar: **Habilitar aceleración 3D**

### 3. Instalar Guest Additions

Dentro de la VM:
sudo mkdir /mnt/cdrom
sudo mount /dev/cdrom /mnt/cdrom
sudo /mnt/cdrom/VBoxLinuxAdditions.run
sudo reboot


### 4. Verificar si estás usando Wayland

echo $XDG_SESSION_TYPE

* Si responde `wayland`, seguí el paso siguiente.

### 5. Forzar Xorg (desactivar Wayland)

sudo nano /etc/gdm3/custom.conf

Agregá o descomentá esta línea:
WaylandEnable=false

Guardá y reiniciá:
sudo reboot

Verificá:
echo $XDG_SESSION_TYPE  # Debe devolver: x11

### 6. Habilitar el ajuste automático

* En VirtualBox: `Ver > Redimensionado automático del huésped`
* Modo pantalla completa: `Host + F` (Ctrl derecho + F)

---

## 💻 Parte 2: Configuración en VMware Workstation / Player

### 1. Importar la OVA

* Menú `File > Open…`
* Seleccioná el archivo `.ova`
* Aceptá la importación

### 2. Configurar hardware

* RAM: 4 GB
* CPU: 2+
* Display: Activar **Accelerate 3D graphics**

### 3. Activar ajuste automático

En el menú `View`:

* ✅ `Autofit Window`
* ✅ `Autofit Guest`
* Entrar a `Full Screen` (Ctrl + Alt + Enter)

### 4. Confirmar VMware Tools

vmware-toolbox-cmd -v

Debe mostrar una versión (ejemplo: `12.1.0.37487`)

### 5. Si sigue sin funcionar → forzar Xorg

Editar:
sudo nano /etc/gdm3/custom.conf

Agregar:
WaylandEnable=false

Reiniciar:
sudo reboot

Verificar:
echo $XDG_SESSION_TYPE  # Debe decir: x11

---

## ✅ Resultado final

* SIFT se ajusta al tamaño de ventana dinámicamente
* Funciona el modo pantalla completa
* La interfaz se ve nítida, sin bordes ni escalado raro
* VMware Tools y/o Guest Additions funcionan correctamente

---

## 📌 Recursos útilez

* 🧪 SIFT Workstation: [https://digital-forensics.sans.org/community/downloads/](https://digital-forensics.sans.org/community/downloads/)
* 🛠️ VirtualBox: [https://www.virtualbox.org/](https://www.virtualbox.org/)
* 💻 VMware Workstation Player: [https://www.vmware.com/products/workstation-player.html](https://www.vmware.com/products/workstation-player.html)

---

Guía creada por [Emiliano Zarate](https://github.com/EmilianoAZarate) para la comunidad forense.
**✨ Compartila si te fue útil y ayudá a otros investigadores a tener un entorno SIFT listo para trabajar.**











