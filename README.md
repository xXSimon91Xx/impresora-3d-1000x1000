<p align="center">
  <img src="assets/logo-institut-jaume-huguet.png" alt="Institut Jaume Huguet — Antiga Escola del Treball, Valls" width="500"/>
</p>

<h1 align="center">Impresora 3D Cartesiana 1000×1000×1000 mm</h1>

<p align="center">
  Proyecto de construcción de una impresora 3D cartesiana de gran formato desde cero.<br>
  Realizado en el <strong>Institut Jaume Huguet</strong> — Antiga Escola del Treball, Valls (Tarragona)<br>
  <a href="https://institutjaumehuguet.cat">institutjaumehuguet.cat</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Firmware-Klipper-brightgreen" alt="Klipper"/>
  <img src="https://img.shields.io/badge/Board-BTT%20Octopus%20Pro%20V1.1-blue" alt="BTT"/>
  <img src="https://img.shields.io/github/last-commit/xXSimon91Xx/impresora-3d-1000x1000" alt="Last commit"/>
</p>

---

## Índice de documentación

### Para empezar — guía de la máquina
- [¿Qué es esta máquina?](#qué-es-esta-máquina) — resumen accesible para todos los niveles
- [El equipo](equipo.md) — alumnos, profesores y agradecimientos
- [Diario del proyecto](diario/progreso.md) — cronología completa desde febrero 2026

### Hardware
- [Lista de materiales — Estructura](hardware/lista-materiales-estructura.md)
- [Planos estructurales — Marco item](hardware/planos-estructurales.md)
- [Placa BTT Octopus Pro](hardware/placa-octopus-pro.md)
- [Motores y drivers](hardware/motores-drivers.md)
- [Piezas impresas en 3D](hardware/piezas-impresas.md)
- **Cableado:**
  - [Visión general — todos los sistemas](hardware/cableado/overview.md)
  - [Eje X](hardware/cableado/eje-x.md) · [Eje Y dual](hardware/cableado/eje-y.md) · [Eje Z dual](hardware/cableado/eje-z.md)
  - [Extrusor Smart Orbiter v3.0](hardware/cableado/extrusor.md)
  - [CR Touch — Sonda Z](hardware/cableado/crtouch.md)
  - [Cama calefactada](hardware/cableado/cama.md)

### Firmware
- [Configuración completa (printer.cfg)](firmware/printer.cfg)
- [Instalación de Klipper en CB1](firmware/klipper-setup.md)
- [Calibración — PID, Z offset, Bed Mesh](firmware/calibracion.md)

### Problemas encontrados y soluciones
- [Índice de problemas](problemas/README.md)

### Ideas de futuro
- [Proyectos y mejoras planificadas](futuro.md)

---

## ¿Qué es esta máquina?

Esta impresora 3D puede fabricar objetos de hasta **1 metro de largo, 1 metro de ancho y 1 metro de alto**. Es una de las impresoras más grandes que se pueden construir con piezas comerciales disponibles.

Se trata de una impresora **cartesiana**: el cabezal de impresión se mueve en los ejes X e Y, y la estructura sube y baja en el eje Z. El material (filamento de plástico) se funde y se deposita capa a capa hasta formar la pieza.

**¿Para qué sirve?** Para fabricar piezas grandes que no caben en una impresora normal: estructuras, moldes, prototipos industriales, piezas de coches, proyectos de arquitectura, etc.

**¿Qué la hace especial?** Todo el sistema de control (firmware Klipper) es de código abierto y se puede acceder a él desde cualquier dispositivo de la red del instituto. No necesita un ordenador conectado para imprimir — tiene su propio ordenador de control integrado (BTT CB1).

---

## Fotos del proyecto

### Panel de electrónica instalado — 1 junio 2026

![Panel electrónica completo](fotos/05-montaje-electronica/panel-electronica-completo-verde.jpg)
*Panel verde con la BTT Octopus Pro V1.1, el CB1 y todos los cables etiquetados y conectados. El panel se desmonta para mantenimiento.*

![KlipperScreen y electrónica montados](fotos/05-montaje-electronica/klipperscreen-y-electronica-montados.jpg)
*La pantalla táctil KlipperScreen (naranja) junto al panel de electrónica ya instalados en la máquina.*

![Drivers TMC y cables etiquetados](fotos/05-montaje-electronica/drivers-tmc-cables-etiquetados.jpg)
*Detalle de los drivers TMC2209 (azul) y TMC5160 (rojo) con los cables etiquetados: MY1, MY2, MX, MZ1, MZ2, EXTRUSOR.*

### Primer arranque con Klipper

![Fluidd funcionando](fotos/05-montaje-electronica/fluidd-funcionando-primer-arranque.jpeg)
*Primer arranque con Fluidd en el monitor Dell, KlipperScreen en pantalla naranja, y toda la electrónica conectada.*

### Placa base — BTT Octopus Pro V1.1

![Octopus Pro chip STM32](fotos/05-montaje-electronica/stm32h723-drivers-closeup.jpg)
*Detalle del chip STM32H723 de la Octopus Pro con los drivers TMC5160 (rojo, eje Z) y TMC2209 (azul, ejes XY y extrusor).*

![Octopus Pro con drivers](fotos/02-electronica/IMG_20260408_193713.jpg)
*Vista frontal de la BTT Octopus Pro V1.1. TMC5160 rojos para Z (alta corriente), TMC2209 azules para X, Y y extrusor.*

### Estructura y guías lineales

![Guía lineal](fotos/01-estructura/IMG_20260422_173948.jpg)
*Guía lineal MGN15R con carro montado sobre perfil item 40×80mm.*

### Piezas impresas en 3D

![Piezas en aula](fotos/03-piezas-impresas/piezas-impresas-todas-aula.jpeg)
*Todas las piezas impresas antes del montaje: soportes de motor (verde PETG), guías y clips (gris ASA).*

### Planos estructurales — marcos item

![Plano item isométrico](fotos/07-planos-item/plano-item-p11-vista-isometrica.jpg)
*Vista isométrica del marco de aluminio. Planos generados con el configurador de item — 17 páginas.*

---

## Especificaciones técnicas

| Parámetro | Valor |
|-----------|-------|
| **Volumen de impresión** | 1000 × 1000 × 1000 mm |
| **Cinemática** | Cartesiana (ejes X/Y/Z independientes) |
| **Firmware** | Klipper + Fluidd (interfaz web) + KlipperScreen |
| **Placa base** | BigTreeTech Octopus Pro V1.1 (STM32H723) |
| **Ordenador de control** | BigTreeTech CB1 (ARM Cortex-A55, Linux) |
| **Velocidad máxima** | 300 mm/s (XY), 5 mm/s (Z) |
| **Aceleración máxima** | 3000 mm/s² |
| **Extrusor** | LDO Smart Orbiter v3.0 (direct drive, reducción 7.5:1) |
| **Motor extrusor** | LDO-36STH20-1004AHG |
| **Hotend** | Boquilla 0.4 mm, 24V 72W calentador cerámico |
| **Sensor Z** | CR Touch ALT04 (Creality) |
| **Drivers XY/Extrusor** | TMC2209 (UART, StealthChop) |
| **Drivers Z** | TMC5160-T Pro (SPI, alta corriente, 2A) |
| **Motores XY** | NEMA 17 |
| **Motores Z** | NEMA 23 (2 unidades, dual Z independiente) |
| **Guías lineales** | MGN15R (rodamientos recirculantes) |
| **Husillo Z** | M12 × 4 entradas → 8 mm/vuelta |
| **Marco** | Perfiles item 40×80 mm (serie XMS, art. 0.0.669.99, L=1400 mm) |
| **Piezas mecánicas** | PETG (verde, no crítico) + ASA/ABS (gris, crítico) |

---

## Historia del proyecto

Este proyecto fue diseñado desde el principio como una impresora de **1000×1000×1000 mm**, con el objetivo de que la máquina quede en el **Institut Jaume Huguet** para uso de los alumnos y como referencia para que otros centros puedan replicarla.

Antes de construirla, usamos una impresora de 500×500 mm para validar el firmware y la electrónica — así llegamos a la impresora grande con todo ya probado.

El mayor reto técnico fue el **doble eje Z con NEMA 23**: necesitan 2 A de corriente, lo que obliga a usar drivers TMC5160 (los TMC2209 normales no aguantan). Además, uno de los slots de la placa resultó estar defectuoso de fábrica y hubo que mover el motor al slot MOTOR 5.

El marco está construido con **perfiles item 40×80 mm**, un sistema modular de aluminio técnico usado en maquinaria industrial, lo que garantiza rigidez y precisión incluso a esta escala.

---

*Proyecto de clase — Institut Jaume Huguet, Valls. Curso 2025-2026.*
