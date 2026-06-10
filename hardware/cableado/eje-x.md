# Cableado — Eje X

## Componentes

| Componente | Especificación |
|-----------|----------------|
| Motor | NEMA 17 |
| Driver | TMC2209 (UART) |
| Slot placa | MOTOR 0 |
| Endstop | Final de carrera mecánico |
| Transmisión | Correa GT2, polea 20 dientes |

## Configuración en printer.cfg

```ini
[stepper_x]
step_pin: PF13
dir_pin: PF12
enable_pin: !PF14
microsteps: 16
rotation_distance: 40      # polea 20T × 2mm paso GT2 = 40mm/vuelta
endstop_pin: ^!PF5         # T1 - pullup + lógica invertida
position_endstop: 0        # home en posición 0 (extremo izquierdo)
position_max: 1000         # recorrido máximo 1000mm
homing_speed: 50

[tmc2209 stepper_x]
uart_pin: PC4
run_current: 0.600
hold_current: 0.300
stealthchop_threshold: 999999   # StealthChop siempre activo
```

## Cálculo de rotation_distance

```
rotation_distance = dientes_polea × paso_correa
rotation_distance = 20 × 2mm = 40mm
```

Con `microsteps: 16` y un motor de 200 pasos/vuelta:
```
Resolución = 40mm / (200 × 16) = 0.0125mm = 12.5 micras por paso
```

## Cableado del motor

El motor NEMA 17 tiene 4 cables que forman 2 bobinas (A y B):

```
Motor NEMA17 → conector JST en MOTOR 0
  A+ →  pin 1
  A- →  pin 2
  B+ →  pin 3
  B- →  pin 4
```

> Si el motor vibra sin moverse, el orden A+/A- o B+/B- está invertido. Intercambiar los dos cables de una bobina.

## Cableado del endstop

```
Endstop X → T1 (PF5)
  Señal → PF5
  GND   → GND del conector
```

El `^!` en `endstop_pin: ^!PF5` activa el pullup interno y la lógica invertida del STM32.

## Diagrama simplificado

```
MOTOR 0 ─────────── NEMA17 eje X
    step: PF13
    dir:  PF12
    ena:  !PF14
    uart: PC4 (TMC2209)

T1 (PF5) ────────── Endstop X
```
