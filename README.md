# Impresora 3D Cartesiana 1000×1000×1000 mm

> Proyecto de construcción de una impresora 3D cartesiana de gran formato desde cero.  
> Firmware: **Klipper** | Placa: **BTT Octopus Pro V1.1 (H723)** | Control: **BTT CB1**

---

## Especificaciones técnicas

| Parámetro | Valor |
|-----------|-------|
| **Volumen de impresión** | 1000 × 1000 × 1000 mm |
| **Cinemática** | Cartesiana (ejes X/Y/Z independientes) |
| **Firmware** | Klipper + Fluidd (interfaz web) |
| **Placa base** | BigTreeTech Octopus Pro V1.1 (STM32H723) |
| **Ordenador de control** | BigTreeTech CB1 (ARM Cortex-A55) |
| **Velocidad máxima** | 300 mm/s (XY), 5 mm/s (Z) |
| **Aceleración máxima** | 3000 mm/s² |
| **Extrusor** | Orbiter SO3 (direct drive, reducción 7.5:1) |
| **Motor extrusor** | LDO-36STH20-1004AHG |
| **Hotend** | Boquilla 0.4 mm, 24V 72W calentador cerámico |
| **Sensor Z** | CR Touch (Creality ALT04) |
| **Drivers XY/Extrusor** | TMC2209 (UART, StealthChop) |
| **Drivers Z** | TMC5160-T Pro (SPI, alta corriente) |
| **Motores XY** | NEMA 17 |
| **Motores Z** | NEMA 23 (2 unidades, dual Z independiente) |
| **Guías lineales** | MGN15R |
| **Husillo Z** | Métrico 12, 4 entradas → 8mm/vuelta |
| **Marco** | Perfil aluminio 2040 |
| **Piezas mecánicas** | Impresas en 3D (PETG verde + ASA/ABS gris) + algunas mecanizadas en aluminio |

---

## Fotos del proyecto

### Primera puesta en marcha — Klipper funcionando

![Fluidd funcionando](fotos/05-montaje-electronica/fluidd-funcionando-primer-arranque.jpeg)
*Primer arranque con Fluidd en el monitor Dell, KlipperScreen en tablet naranja, y el sistema de electrónica completo a la izquierda.*

### Placa base — BTT Octopus Pro V1.1

![Octopus Pro con drivers](fotos/02-electronica/IMG_20260408_193713.jpg)
*Vista frontal de la BTT Octopus Pro V1.1. TMC5160 rojos para Z (alta corriente), TMC2209 azules para X, Y y extrusor.*

![Octopus Pro trasera](fotos/00-prototipo-inicial/IMG_20260225_190124.jpg)
*Vista trasera de la Octopus Pro mostrando el serigrafíado de todos los slots: MOTOR0 a MOTOR7, MOTOR2_1, MOTOR2_2.*

### Cableado en progreso

![Cableado completo](fotos/05-montaje-electronica/cableado-completo-en-progreso.jpeg)
*Proceso de conexión de todos los motores, sensores y calefactores a la placa.*

![Cable hotend etiquetado](fotos/02-electronica/cable-thermistor-heater-etiquetado.jpeg)
*Cable del hotend con etiquetas identificando cada hilo: Thermistor104NT-4 y 24V/72W Heater.*

### CR Touch — Sonda de nivelación

![CR Touch ALT04](fotos/02-electronica/IMG_20260408_193645.jpg)
*CR Touch modelo ALT04. Los 5 colores de cable: azul (sensor), rojo (GND), amarillo (control), negro (5V), blanco (GND).*

### Estructura y guías lineales

![Guía lineal](fotos/01-estructura/IMG_20260422_173948.jpg)
*Guía lineal MGN15R con carro montado sobre perfil de aluminio 2040.*

![Cables extrusor en máquina](fotos/01-estructura/cables-extrusor-etiquetados-montados.jpeg)
*Mazo de cables del cabezal (etiquetados "EXTRUSOR") ya instalados en la impresora.*

### Piezas impresas en 3D

![Piezas en aula](fotos/03-piezas-impresas/piezas-impresas-todas-aula.jpeg)
*Todas las piezas impresas antes del montaje: soportes de motor (verde), guías y clips (gris).*

![Soportes NEMA23](fotos/03-piezas-impresas/soportes-nema23-grandes.jpeg)
*Soportes para los motores NEMA 23 de los ejes X e Y.*

---

## Índice de documentación

### Hardware
- [Lista de materiales — Estructura](hardware/lista-materiales-estructura.md) — perfiles, guías, tornillería, cantidades
- [Lista de componentes electrónicos (BOM)](hardware/componentes.md) — placa, drivers, motores, con razonamiento
- [Placa BTT Octopus Pro](hardware/placa-octopus-pro.md) — slots, drivers, jumpers, alimentación
- [Motores y drivers](hardware/motores-drivers.md) — NEMA17 vs NEMA23, TMC2209 vs TMC5160
- [Piezas impresas en 3D](hardware/piezas-impresas.md) — catálogo de todas las piezas
- **Cableado:**
  - [Diagrama general](hardware/cableado/diagrama-general.md) — vista completa de todas las conexiones
  - [Eje X](hardware/cableado/eje-x.md)
  - [Eje Y — Dual motor en paralelo](hardware/cableado/eje-y.md)
  - [Eje Z — Dual Z independiente](hardware/cableado/eje-z.md)
  - [Extrusor — Orbiter SO3](hardware/cableado/extrusor.md)
  - [CR Touch — Sonda Z virtual](hardware/cableado/crtouch.md)
  - [Cama calefactada](hardware/cableado/cama.md)

### Firmware
- [Configuración Klipper (printer.cfg)](firmware/printer.cfg) — archivo completo con comentarios explicativos
- [Instalación y flasheo de Klipper en CB1](firmware/klipper-setup.md)
- [Calibración](firmware/calibracion.md) — PID, Z offset, Bed Mesh, Z_TILT

### Problemas y soluciones
- [Índice de problemas](problemas/README.md) — todos los bugs encontrados y cómo se resolvieron
  - [Slot MOTOR 3 defectuoso](problemas/motor-z-slot-defectuoso.md) ← **leer esto**
  - [Motor Y no funcionaba — era el cable](problemas/eje-y-dual.md)
  - [Ventilador hotend siempre encendido](problemas/ventilador-hotend.md)
  - [Endstop Z de seguridad](problemas/endstop-z.md)
  - [CR Touch — z_offset obligatorio](problemas/crtouch-z-offset.md)
  - [Calentador subía solo a 220°C](problemas/calentador-runaway.md)
  - [Husillo T12 de 1200mm](problemas/husillo-trapezoidal.md)
  - [Termistor cama pendiente](problemas/termistor-cama.md)

### Diario del proyecto
- [Progreso cronológico](diario/progreso.md) — de los primeros tests en febrero a la impresora de 1m³

---

## Historia del proyecto

Este proyecto fue diseñado desde el principio como una impresora de **1000×1000×1000mm**. Antes de construirla, usamos una impresora de 500×500mm que ya teníamos para validar el firmware y la electrónica — así llegamos a la impresora grande con todo ya probado.

Todos los **soportes, esquinas y piezas mecánicas** están impresos en 3D en PLA verde y gris, diseñados específicamente para este proyecto.

El mayor reto técnico fue el **doble eje Z con NEMA 23**: necesitan 2A de corriente, lo que obliga a usar drivers TMC5160 (los TMC2209 normales no aguantan esa corriente). Además, uno de los slots de la placa resultó estar defectuoso de fábrica y hubo que mover el motor al slot MOTOR 5.

---

![Klipper](https://img.shields.io/badge/Firmware-Klipper-brightgreen)
![BTT](https://img.shields.io/badge/Board-BTT%20Octopus%20Pro%20V1.1-blue)
![GitHub last commit](https://img.shields.io/github/last-commit/xXSimon91Xx/impresora-3d-1000x1000)
