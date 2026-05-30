# Problema: Eje Y — Motor no se movía (cable) + Upgrade a dual motor

## Parte 1: Motor Y no se movía — era el cable

### Descripción

El motor Y (un solo motor inicialmente) no respondía al comando de movimiento. El driver TMC2209 estaba correctamente configurado y el slot de la placa parecía funcionar.

### Diagnóstico

Seguimos el mismo proceso de eliminación que con el slot MOTOR 3:
1. Cambiamos el driver del Y al slot del X → el motor X movía el eje X → **driver OK**
2. Comprobamos las señales del slot Y → señales correctas → **slot OK**

La causa: **el cable del motor Y estaba mal conectado** en el conector JST de la placa.

> «Vale ya lo he arreglado, era el cable efectivamente» — conversación

### Solución

Reconectar el cable del motor Y asegurando que el orden de los hilos (A+, A-, B+, B-) es correcto.

### Lección

Antes de cambiar configuración, **siempre comprueba los cables físicamente**:
- Desconecta y reconecta el conector JST
- Verifica el orden de los hilos (A+, A-, B+, B-)
- Un hilo invertido puede hacer que el motor vibre sin moverse o no responda

---

## Parte 2: Upgrade a doble motor Y en paralelo

### Por qué un motor no era suficiente

Con una cama de **1000mm de profundidad**, el eje Y necesitaba tracción en ambos extremos. Un solo motor en un extremo causaría:
- Flexión del perfil de aluminio en el extremo sin tracción
- Pérdida de pasos en movimientos rápidos
- Posible resonancia/vibración asimétrica

### Solución: 2 motores NEMA 17 en paralelo

Ambos motores se conectan **en paralelo** al mismo slot de driver (MOTOR 2_1), usando el slot MOTOR 2_2 solo como paso físico de corriente (sin configuración propia).

```ini
[stepper_y]
step_pin: PF11    # MOTOR 2_1 — único driver para ambos motores
dir_pin: PG3
enable_pin: !PG5
microsteps: 16
rotation_distance: 40
endstop_pin: ^PG9
position_endstop: 0
position_max: 1000
homing_speed: 50

[tmc2209 stepper_y]
uart_pin: PC6     # UART del slot MOTOR 2_1
run_current: 1.000  # 1A total = ~0.5A por motor en paralelo
hold_current: 0.500
stealthchop_threshold: 999999
```

> El segundo motor (en slot 2_2) se conecta físicamente en paralelo al primero. No tiene sección propia en printer.cfg porque comparte el driver.

### ¿Por qué no usar 2 drivers independientes (como en Z)?

En Z usamos `Z_TILT_ADJUST` que necesita drivers independientes para mover cada motor de forma diferente y corregir la inclinación de la cama.

En Y **no necesitamos corrección de inclinación** — ambos motores siempre se mueven exactamente igual. Por eso, 1 driver en paralelo es suficiente y más simple.

### Resultado

Con dos motores en paralelo:
- Tracción simétrica en ambos extremos del eje
- Sin pérdida de pasos en movimientos rápidos
- Sin vibración asimétrica
