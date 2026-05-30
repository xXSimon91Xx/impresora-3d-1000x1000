# Cableado — Eje Z (Dual independiente)

> El eje Z es el más complejo del proyecto: dos motores NEMA 23 independientes con drivers TMC5160 de alta potencia, configurados para el `Z_TILT_ADJUST` de Klipper.

---

## Componentes

| Componente | Especificación |
|-----------|----------------|
| Motores | 2× NEMA 23 |
| Drivers | 2× TMC5160-T Pro (SPI) |
| Slot Z izquierdo | MOTOR 1 |
| Slot Z derecho | MOTOR 5 (MOTOR 3 defectuoso) |
| Transmisión | Husillo métrico 12, paso 2mm, 4 entradas |
| Endstop Z | CR Touch (virtual) + endstop mecánico de seguridad en tope |

---

## Configuración en printer.cfg

```ini
# Z IZQUIERDO — Motor principal
[stepper_z]
step_pin: PG0
dir_pin: PG1
enable_pin: !PF15
microsteps: 16
rotation_distance: 8       # 2mm paso × 4 entradas = 8mm/vuelta
endstop_pin: probe:z_virtual_endstop  # CR Touch como endstop
position_min: -2           # permite bajar 2mm bajo 0 para calibrar offset
position_max: 1000
homing_speed: 10

[tmc5160 stepper_z]
cs_pin: PD11               # Chip Select SPI — MOTOR 1
spi_software_miso_pin: PA6
spi_software_mosi_pin: PA7
spi_software_sclk_pin: PA5
run_current: 2.000         # NEMA23 necesita 2A
hold_current: 1.000
stealthchop_threshold: 999999
driver_TOFF: 3
driver_HSTRT: 4
driver_HEND: 3

# Z DERECHO — Motor esclavo
[stepper_z1]
step_pin: PC13             # MOTOR 5 (MOTOR 3 estaba defectuoso)
dir_pin: !PF0              # ! invertido — gira en la misma dirección que Z
enable_pin: !PF1
microsteps: 16
rotation_distance: 8

[tmc5160 stepper_z1]
cs_pin: PE4                # Chip Select SPI — MOTOR 5
spi_software_miso_pin: PA6
spi_software_mosi_pin: PA7
spi_software_sclk_pin: PA5
run_current: 2.000
hold_current: 1.000
stealthchop_threshold: 999999
driver_TOFF: 3
driver_HSTRT: 4
driver_HEND: 3
```

---

## Por qué `dir_pin: !PF0` en Z derecho

Los dos motores Z están montados en lados opuestos de la impresora. Si los dos giraran en la misma dirección absoluta, uno bajaría la cama y el otro la subiría — la cama se torcería.

Al añadir `!` (inversión) al `dir_pin` del Z derecho, un motor gira en sentido horario y el otro en antihorario — pero **ambos elevan o bajan la cama en la misma dirección**.

Esto se descubrió durante las pruebas:
> «Vale funciona bb, voy a girar el signo de exclamación porque uno gira al revés bb» — conversación

---

## SPI compartido entre los dos TMC5160

Los dos drivers TMC5160 comparten los pines de bus SPI (MISO, MOSI, SCLK) pero cada uno tiene su propio `cs_pin`:

```
SPI bus:
  MISO → PA6  (todos los TMC5160 conectados aquí)
  MOSI → PA7
  SCLK → PA5

Chip Select individual:
  Z izq (MOTOR 1) → PD11
  Z der (MOTOR 5) → PE4
```

El Chip Select determina cuál driver está activo en cada momento — los demás ignoran los datos del bus.

---

## Z_TILT_ADJUST — Para qué sirve

Con dos motores Z independientes, Klipper puede **corregir automáticamente** si la cama no está perfectamente nivelada de lado a lado.

```ini
[z_tilt]
z_positions:
    -50, 150    # posición física del husillo izquierdo (X, Y)
    350, 150    # posición física del husillo derecho
points:
    30, 150     # donde sondea el CR Touch para el lado izquierdo
    270, 150    # donde sondea para el lado derecho
speed: 50
horizontal_move_z: 5
```

Al ejecutar `Z_TILT_ADJUST`, Klipper:
1. Sondea el punto izquierdo con el CR Touch
2. Sondea el punto derecho
3. Calcula la diferencia
4. Mueve independientemente cada motor para igualar las alturas

---

## Endstop de seguridad Z máximo

```ini
[gcode_button endstop_z_max]
pin: ^PF7    # T3 — conector de temperatura reconvertido en entrada digital
press_gcode:
  M118 EMERGENCIA - Endstop Z maximo activado
  M112         # Parada de emergencia
```

Instalado en el tope físico superior de las guías Z. Si el software falla y el eje sube más de lo debido, el final de carrera mecánico detiene todo con `M112`.

---

## Cálculo de rotation_distance

```
Husillo T12, 4 entradas, 2mm de paso:
rotation_distance = 2mm × 4 = 8mm/vuelta

Con microsteps: 16 y 200 pasos/vuelta:
Resolución = 8mm / (200 × 16) = 0.0025mm = 2.5 micras
```
