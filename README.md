<p align="center">
  <img src="assets/logo-institut-jaume-huguet.png" alt="Institut Jaume Huguet â€” Antiga Escola del Treball, Valls" width="500"/>
</p>

<h1 align="center">Impresora 3D Cartesiana 1000Ã—1000Ã—1000 mm</h1>

<p align="center">
  <strong>Cicle Formatiu de FabricaciÃ³ Additiva â€” CEFP 2025-2026</strong><br>
  <strong>Institut Jaume Huguet â€” Antiga Escola del Treball</strong>, Valls (Tarragona)<br>
  <a href="https://institutjaumehuguet.cat">institutjaumehuguet.cat</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Firmware-Klipper-brightgreen" alt="Klipper"/>
  <img src="https://img.shields.io/badge/Board-BTT%20Octopus%20Pro%20V1.1-blue" alt="BTT"/>
  <img src="https://img.shields.io/github/last-commit/xXSimon91Xx/impresora-3d-1000x1000" alt="Last commit"/>
</p>

---

## Versiones del repositorio

| Idioma | Repositorio |
|--------|-------------|
| ðŸ‡ªðŸ‡¸ EspaÃ±ol | **este repositorio** |
| ðŸ´ó ¥ó ³ó £ó ´ó ¿ CatalÃ  | [impresora-3d-1000x1000-ca](https://github.com/xXSimon91Xx/impresora-3d-1000x1000-ca) |
| ðŸ‡¬ðŸ‡§ English | [impresora-3d-1000x1000-en](https://github.com/xXSimon91Xx/impresora-3d-1000x1000-en) |

---

## Ãndice de documentaciÃ³n

### Para empezar â€” guÃ­a de la mÃ¡quina
- [Â¿QuÃ© es esta mÃ¡quina?](#quÃ©-es-esta-mÃ¡quina) â€” resumen accesible para todos los niveles
- [El equipo](equipo.md) â€” alumnos, profesores y agradecimientos
- [Diario del proyecto](diario/progreso.md) â€” cronologÃ­a completa desde febrero 2026

### Hardware
- [**Archivos 3D â€” 57 archivos STEP**](hardware/archivos-3d/) â€” ensamble completo, extrusor, ejes, cama
- [Lista de materiales â€” Estructura](hardware/lista-materiales-estructura.md)
- [Planos estructurales â€” Marco item](hardware/planos-estructurales.md)
- [Placa BTT Octopus Pro](hardware/placa-octopus-pro.md)
- [Motores y drivers](hardware/motores-drivers.md)
- [Piezas impresas en 3D](hardware/piezas-impresas.md)
- **Cableado:**
  - [VisiÃ³n general â€” todos los sistemas](hardware/cableado/overview.md)
  - [Eje X](hardware/cableado/eje-x.md) Â· [Eje Y dual](hardware/cableado/eje-y.md) Â· [Eje Z dual](hardware/cableado/eje-z.md)
  - [Extrusor Smart Orbiter v3.0](hardware/cableado/extrusor.md)
  - [CR Touch â€” Sonda Z](hardware/cableado/crtouch.md)
  - [Cama calefactada](hardware/cableado/cama.md)

### Firmware
- [ConfiguraciÃ³n completa (printer.cfg)](firmware/printer.cfg)
- [InstalaciÃ³n de Klipper en CB1](firmware/klipper-setup.md)
- [CalibraciÃ³n â€” PID, Z offset, Bed Mesh](firmware/calibracion.md)

### Problemas encontrados y soluciones
- [Ãndice de problemas](problemas/README.md)

### Ideas de futuro
- [Proyectos y mejoras planificadas](futuro.md)

---

## Â¿QuÃ© es esta mÃ¡quina?

Esta impresora 3D puede fabricar objetos de hasta **1 metro de largo, 1 metro de ancho y 1 metro de alto**. Es una de las impresoras mÃ¡s grandes que se pueden construir con piezas comerciales disponibles.

Se trata de una impresora **cartesiana**: el cabezal de impresiÃ³n se mueve en los ejes X e Y, y la estructura sube y baja en el eje Z. El material (filamento de plÃ¡stico) se funde y se deposita capa a capa hasta formar la pieza.

**Â¿Para quÃ© sirve?** Para fabricar piezas grandes que no caben en una impresora normal: estructuras, moldes, prototipos industriales, piezas de coches, proyectos de arquitectura, etc.

**Â¿QuÃ© la hace especial?** Todo el sistema de control (firmware Klipper) es de cÃ³digo abierto y se puede acceder a Ã©l desde cualquier dispositivo de la red del instituto. No necesita un ordenador conectado para imprimir â€” tiene su propio ordenador de control integrado (BTT CB1).

---

## La mÃ¡quina terminada â€” 5 junio 2026

![MÃ¡quina completa](fotos/08-maquina-completa/maquina-completa-frontal-hero.jpg)
*La impresora 3D de 1000Ã—1000Ã—1000mm completamente ensamblada en el taller del Institut Jaume Huguet. Altura total ~1.5m. KlipperScreen tÃ¡ctil (naranja) montado en el perfil derecho. Cama de cristal provisional instalada.*

![MÃ¡quina vista frontal con pantalla](fotos/08-maquina-completa/maquina-completa-frontal-clase.jpg)
*Vista frontal de la impresora en el aula del taller. La escala respecto al mobiliario del instituto da una idea del tamaÃ±o de la mÃ¡quina.*

---

## Fotos del proyecto

### Panel de electrÃ³nica instalado â€” 1 junio 2026

![Panel electrÃ³nica completo](fotos/05-montaje-electronica/panel-electronica-completo-verde.jpg)
*Panel verde con la BTT Octopus Pro V1.1, el CB1 y todos los cables etiquetados y conectados. El panel se desmonta para mantenimiento.*

![KlipperScreen y electrÃ³nica montados](fotos/05-montaje-electronica/klipperscreen-y-electronica-montados.jpg)
*La pantalla tÃ¡ctil KlipperScreen (naranja) junto al panel de electrÃ³nica ya instalados en la mÃ¡quina.*

![Drivers TMC y cables etiquetados](fotos/05-montaje-electronica/drivers-tmc-cables-etiquetados.jpg)
*Detalle de los drivers TMC2209 (azul) y TMC5160 (rojo) con los cables etiquetados: MY1, MY2, MX, MZ1, MZ2, EXTRUSOR.*

### Primer arranque con Klipper

![Fluidd funcionando](fotos/05-montaje-electronica/fluidd-funcionando-primer-arranque.jpeg)
*Primer arranque con Fluidd en el monitor Dell, KlipperScreen en pantalla naranja, y toda la electrÃ³nica conectada.*

### Placa base â€” BTT Octopus Pro V1.1

![Octopus Pro chip STM32](fotos/05-montaje-electronica/stm32h723-drivers-closeup.jpg)
*Detalle del chip STM32H723 de la Octopus Pro con los drivers TMC5160 (rojo, eje Z) y TMC2209 (azul, ejes XY y extrusor).*

![Octopus Pro con drivers](fotos/02-electronica/IMG_20260408_193713.jpg)
*Vista frontal de la BTT Octopus Pro V1.1. TMC5160 rojos para Z (alta corriente), TMC2209 azules para X, Y y extrusor.*

### Estructura y guÃ­as lineales

![GuÃ­a lineal](fotos/01-estructura/IMG_20260422_173948.jpg)
*GuÃ­a lineal MGN15R con carro montado sobre perfil item 40Ã—80mm.*

### Piezas impresas en 3D

![Piezas en aula](fotos/03-piezas-impresas/piezas-impresas-todas-aula.jpeg)
*Todas las piezas impresas antes del montaje: soportes de motor (verde PETG), guÃ­as y clips (gris ASA).*

### Planos estructurales â€” marcos item

![Plano item isomÃ©trico](fotos/07-planos-item/plano-item-p11-vista-isometrica.jpg)
*Vista isomÃ©trica del marco de aluminio. Planos generados con el configurador de item â€” 17 pÃ¡ginas.*

---

## Especificaciones tÃ©cnicas

| ParÃ¡metro | Valor |
|-----------|-------|
| **Volumen de impresiÃ³n** | 1000 Ã— 1000 Ã— 1000 mm |
| **CinemÃ¡tica** | Cartesiana (ejes X/Y/Z independientes) |
| **Firmware** | Klipper + Fluidd (interfaz web) + KlipperScreen |
| **Placa base** | BigTreeTech Octopus Pro V1.1 (STM32H723) |
| **Ordenador de control** | BigTreeTech CB1 (ARM Cortex-A55, Linux) |
| **Velocidad mÃ¡xima** | 300 mm/s (XY), 5 mm/s (Z) |
| **AceleraciÃ³n mÃ¡xima** | 3000 mm/sÂ² |
| **Extrusor** | LDO Smart Orbiter v3.0 (direct drive, reducciÃ³n 7.5:1) |
| **Motor extrusor** | LDO-36STH20-1004AHG |
| **Hotend** | Boquilla 0.4 mm, 24V 72W calentador cerÃ¡mico |
| **Sensor Z** | CR Touch ALT04 (Creality) |
| **Drivers XY/Extrusor** | TMC2209 (UART, StealthChop) |
| **Drivers Z** | TMC5160-T Pro (SPI, alta corriente, 2A) |
| **Motores XY** | NEMA 17 |
| **Motores Z** | NEMA 23 (2 unidades, dual Z independiente) |
| **GuÃ­as lineales** | MGN15R (rodamientos recirculantes) |
| **Husillo Z** | M12 Ã— 4 entradas â†’ 8 mm/vuelta |
| **Marco** | Perfiles item 40Ã—80 mm (serie XMS, art. 0.0.669.99) |
| **Piezas mecÃ¡nicas** | PETG (verde, no crÃ­tico) + ASA/ABS (gris, crÃ­tico) |

---

## Historia del proyecto

Este proyecto fue diseÃ±ado desde el principio como una impresora de **1000Ã—1000Ã—1000 mm**, con el objetivo de que la mÃ¡quina quede en el **Institut Jaume Huguet** para uso de los alumnos y como referencia para que otros centros puedan replicarla.

Antes de construirla, usamos una impresora de 500Ã—500 mm para validar el firmware y la electrÃ³nica â€” asÃ­ llegamos a la impresora grande con todo ya probado.

El mayor reto tÃ©cnico fue el **doble eje Z con NEMA 23**: necesitan 2 A de corriente, lo que obliga a usar drivers TMC5160 (los TMC2209 normales no aguantan). AdemÃ¡s, uno de los slots de la placa resultÃ³ estar defectuoso de fÃ¡brica y hubo que mover el motor al slot MOTOR 5.

El marco estÃ¡ construido con **perfiles item 40Ã—80 mm**, un sistema modular de aluminio tÃ©cnico usado en maquinaria industrial, lo que garantiza rigidez y precisiÃ³n incluso a esta escala.

---

*Cicle Formatiu de FabricaciÃ³ Additiva â€” CEFP 2025-2026 | Institut Jaume Huguet â€” Antiga Escola del Treball, Valls*
