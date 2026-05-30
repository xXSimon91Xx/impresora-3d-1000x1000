# Cableado — Eje Y (Dual motor en paralelo)

## Componentes

| Componente | Especificación |
|-----------|----------------|
| Motores | 2× NEMA 17 (en paralelo) |
| Driver | 1× TMC2209 (UART) — comparte los 2 motores |
| Slot placa | MOTOR 2_1 (driver) + MOTOR 2_2 (sin driver, paralelo físico) |
| Endstop | Final de carrera mecánico |
| Transmisión | Correa GT2, polea 20 dientes |

---

## Configuración en printer.cfg

```ini
[stepper_y]
step_pin: PF11             # MOTOR 2_1 — único driver
dir_pin: PG3
enable_pin: !PG5
microsteps: 16
rotation_distance: 40      # polea 20T × 2mm paso GT2 = 40mm/vuelta
endstop_pin: ^PG9          # ^ = pullup
position_endstop: 0
position_max: 1000
homing_speed: 50

[tmc2209 stepper_y]
uart_pin: PC6              # UART del slot MOTOR 2_1
run_current: 1.000         # 1A total ÷ 2 motores en paralelo = 0.5A cada uno
hold_current: 0.500
stealthchop_threshold: 999999
```

> El segundo motor (slot 2_2) se conecta físicamente en paralelo al primero. **No tiene sección propia en printer.cfg** — un solo driver los controla a ambos.

---

## Diagrama de cableado en paralelo

```
Driver TMC2209 (MOTOR 2_1)
    └── Motor Y izquierdo (MOTOR 2_1)
         ├── A+ ──── A+ motor izquierdo
         ├── A- ──── A- motor izquierdo
         ├── B+ ──── B+ motor izquierdo
         └── B- ──── B- motor izquierdo

    └── Motor Y derecho (MOTOR 2_2 físicamente, paralelo eléctrico)
         ├── A+ ──── A+ motor derecho
         ├── A- ──── A- motor derecho  
         ├── B+ ──── B+ motor derecho
         └── B- ──── B- motor derecho
```

Los dos conectores de salida de la Octopus Pro para MOTOR 2_1 y MOTOR 2_2 están eléctricamente conectados — ambos llevan las mismas señales step/dir/enable del mismo driver.

---

## Historia: de 1 motor a 2 en paralelo

**Configuración inicial:** 1 solo motor Y en MOTOR 2_1.

**Problema:** Con 1000mm de recorrido, un solo motor en un extremo causaría:
- Asimetría de fuerzas → el eje se tuerce levemente
- Pérdida de pasos en aceleraciones altas

**Solución:** Añadir un segundo motor NEMA 17 en paralelo en el otro extremo del eje.

Ver: [Problema — Eje Y dual](../../problemas/eje-y-dual.md)

---

## Endstop Y

```
Endstop Y → STOP_2 (PG9)
  Señal → PG9
  GND   → GND
```

El endstop se activa cuando el carro Y llega al extremo trasero (posición 0). El home mueve el carro hacia atrás hasta activar el endstop, luego retrocede a position_endstop.
