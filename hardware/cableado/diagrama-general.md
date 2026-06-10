# Diagrama general de cableado

> Vista simplificada de todas las conexiones eléctricas del sistema.

---

## Diagrama de bloques

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FUENTE DE ALIMENTACIÓN 24V                       │
└──────────┬───────────────────────┬──────────────────────────────────┘
           │                       │
    MOTOR-POWER + POWER         BED-POWER
           │                       │
           ▼                       ▼
┌──────────────────────────────────────────────────────────────────────┐
│                   BTT OCTOPUS PRO V1.1 (STM32H723)                  │
│                                                                      │
│  MOTOR0──TMC2209──► Motor X (NEMA17)                                │
│  MOTOR2_1 TMC2209──► Motor Y izq (NEMA17) ┐ En paralelo            │
│  MOTOR2_2─────────► Motor Y der (NEMA17)  ┘                        │
│  MOTOR1──TMC5160──► Motor Z izq (NEMA23)                            │
│  MOTOR5──TMC5160──► Motor Z der (NEMA23)  [MOTOR3 = DEFECTUOSO]    │
│  MOTOR4──TMC2209──► Extrusor SO3 (LDO)                             │
│                                                                      │
│  T1 (PF5)    ◄──── Endstop X                                       │
│  T2 (PF6)    ◄──── Endstop Y                                       │
│  T3 (PF7)    ◄──── Endstop Z máximo (seguridad)                    │
│  PB6 / PB7   ◄──► CR Touch (control + sensor)                      │
│                                                                      │
│  T0 (PF4)    ◄──── Termistor hotend (ATC Semitec 104NT-4)          │
│  T1 (PF3)    ◄──── Termistor cama (pendiente instalar)             │
│  HE0 (PA3)   ────► Calentador hotend 24V 72W                       │
│  HE_BED(PA1) ────► Cama calefactada 24V                            │
│  FAN2 (PD12) ────► Ventilador heatsink SO3                         │
│  FAN3 (PD13) ────► Ventilador de capa (J53)                        │
│                                                                      │
│  PG11 ◄────────── Sensor filamento SO3                              │
│  PG10 ◄────────── Botón descarga SO3                               │
│                                                                      │
│  USB-C ◄─────────► BTT CB1 (ARM, Klipper host, Fluidd web UI)      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## CR Touch — 5 cables (detalle)

```
CR Touch ALT04
  ┌─────────────┐
  │ Azul   PB7  │── Sensor (señal de toque) → ^ pullup
  │ Rojo   GND  │── Masa
  │ Amarillo PB6│── Control servo (deploy / retract)
  │ Negro  5V   │── Alimentación 5V
  │ Blanco GND  │── Masa
  └─────────────┘
```

![CR Touch cables](../../fotos/02-electronica/IMG_20260408_193645.jpg)
*CR Touch ALT04 con su cable de 5 colores.*

---

## TMC5160 — Bus SPI compartido

Los dos drivers TMC5160 (Z izq y Z der) comparten el mismo bus SPI pero cada uno tiene su propio **Chip Select**:

```
Bus SPI (compartido):
  PA6 ──── MISO  ──── TMC5160 Z izq ──── TMC5160 Z der
  PA7 ──── MOSI
  PA5 ──── SCLK

Chip Select (individual):
  PD11 ──── CS del TMC5160 Z izq (MOTOR 1)
  PE4  ──── CS del TMC5160 Z der (MOTOR 5)
```

---

## Jumpers de voltaje en la Octopus Pro

**Importante**: Los TMC5160 necesitan el jumper en posición `VFused` (tensión del motor), no en 5V.

```
Cada slot de driver tiene 3 jumpers de voltaje:
  [VFused] [VFused] [VFused]  ← posición correcta para TMC5160 y TMC2209
```

---

## Cable del hotend (etiquetado)

![Cable hotend etiquetado](../../fotos/02-electronica/cable-thermistor-heater-etiquetado.jpeg)
*Cable del extrusor con etiquetas: "Thermistor104NT-4" y "24V/72W Heater".*

```
Conector hotend → Octopus Pro:
  Termistor (2 cables blancos) ──── T0 (PF4)
  Calentador (2 cables negros) ──── HE0 (PA3)
```

---

## Cables de motor (NEMA17 y NEMA23)

Los motores paso a paso tienen 4 cables que forman 2 bobinas:

```
Motor → Conector JST 4 pines en placa:
  A+  ──── pin 1
  A-  ──── pin 2
  B+  ──── pin 3
  B-  ──── pin 4

⚠ Si el motor vibra sin moverse: A+ y A- están intercambiados
⚠ Si gira al revés: cambiar ! en dir_pin (o intercambiar A+ con B+)
```

---

## Cables "EXTRUSOR" etiquetados en la máquina

![Cables extrusor montados](../../fotos/01-estructura/cables-extrusor-etiquetados-montados.jpeg)
*Mazo de cables del cabezal etiquetados como "EXTRUSOR" ya montados en la impresora.*

Los cables del cabezal incluyen: termistor, calentador, motor extrusor, ventilador, sensor filamento, botón descarga y CR Touch.

---

## Diagrama de alimentación

```
Fuente 24V
  ├── MOTOR-POWER ──── Todos los slots de driver (MOTOR 0-7)
  ├── POWER       ──── Lógica + ventiladores + hotend heater
  └── BED-POWER   ──── Cama calefactada (rail separado)
```

> **Por qué separar BED-POWER:** La cama consume mucha corriente (puede ser 10-20A). Si comparte rail con los motores, los picos de corriente pueden causar fallos de paso o ruido en las señales.
