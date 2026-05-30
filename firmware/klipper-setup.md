# Instalación y configuración de Klipper

> Guía para instalar Klipper en el BTT CB1 y flashear el firmware en la BTT Octopus Pro V1.1 (STM32H723).

---

## Arquitectura del sistema

```
CB1 (ARM Linux) ─── USB-C ─── Octopus Pro (STM32H723)
      │                              │
      │ Klipper host                 │ Klipper MCU firmware
      │ Fluidd (web UI)              │ Gestiona: motores, temp, fans
      │                              │
  microSD                        Flash interna
```

El CB1 corre el **host de Klipper** (Python) y se comunica con el **firmware de Klipper** en el STM32 de la Octopus Pro vía USB. Fluidd es la interfaz web para controlar la impresora desde el navegador.

---

## Paso 1: Imagen del CB1

1. Descargar la imagen de BTT CB1 con Klipper preinstalado desde el repositorio oficial de BigTreeTech
2. Flashear la microSD con **Balena Etcher** o **Raspberry Pi Imager**
3. Insertar la microSD en el slot del CB1
4. La imagen ya incluye: Klipper + Moonraker + Fluidd + KlipperScreen

---

## Paso 2: Compilar y flashear el firmware para el STM32H723

### Configuración de compilación

```bash
cd ~/klipper
make menuconfig
```

En el menú seleccionar:
- **Micro-controller Architecture:** STMicroelectronics STM32
- **Processor model:** STM32H723
- **Bootloader offset:** 128KiB bootloader
- **Clock Reference:** 25 MHz crystal
- **Communication interface:** USB (on PA11/PA12)

```bash
make clean
make
```

Esto genera `out/klipper.bin`.

### Método de flasheo: SD card

1. Renombrar `klipper.bin` a `firmware.bin`
2. Copiar a una microSD formateada en FAT32
3. Insertar la SD en el slot de la **Octopus Pro** (no en el CB1)
4. Encender la placa — el bootloader carga el firmware automáticamente
5. El LED parpadea durante el flasheo, luego queda fijo

### Verificar que el firmware cargó

```bash
ls /dev/serial/by-id/
# Debe aparecer: usb-Klipper_stm32h723xx_XXXX-if00
```

---

## Paso 3: Configurar printer.cfg

El archivo de configuración principal está en:
```
~/printer_data/config/printer.cfg
```

Copiar el `printer.cfg` de este repositorio como punto de partida.

Actualizar el serial del MCU:
```ini
[mcu]
serial: /dev/serial/by-id/usb-Klipper_stm32h723xx_XXXXXXXX-if00
```
(El ID exacto varía por placa — usar el `ls /dev/serial/by-id/` para obtenerlo)

---

## Paso 4: Acceder a Fluidd

Una vez todo conectado:

1. La Octopus Pro conectada al CB1 por USB-C
2. El CB1 conectado a la red WiFi
3. Abrir el navegador y acceder a la IP del CB1 (ej: `http://192.168.1.XX`)

Fluidd muestra:
- Estado de la impresora en tiempo real
- Temperatura de hotend y cama
- Controles de movimiento manual
- Consola para enviar comandos G-code

---

## Comandos de prueba inicial

```gcode
# Verificar temperatura (sin calentar)
M105

# Mover eje X sin hacer home (para probar)
SET_KINEMATIC_POSITION X=100 Y=100 Z=10
G1 X200 F3000

# Test de extrusor (forzado, hotend frío)
SET_KINEMATIC_POSITION X=100 Y=100 Z=10
# (con min_extrude_temp: 0 temporalmente)

# Home completo
G28

# Calibración Z con CR Touch
PROBE_CALIBRATE

# Z_TILT (ecualizar los dos motores Z)
Z_TILT_ADJUST

# Mapa de cama
BED_MESH_CALIBRATE
```

---

## Estructura de archivos de Klipper en el CB1

```
~/printer_data/
├── config/
│   ├── printer.cfg          # Configuración principal
│   └── moonraker.conf       # Config de la API web
├── gcodes/                  # Archivos G-code para imprimir
├── logs/                    # Logs del sistema
└── database/                # Base de datos de Klipper
```
