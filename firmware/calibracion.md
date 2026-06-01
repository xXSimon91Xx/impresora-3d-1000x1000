# Calibración — PID, Z offset, Bed Mesh, Z_TILT

> Todos los pasos de calibración necesarios para una impresión correcta.

---

## 1. PID del hotend

El PID controla el calefactor del hotend para mantener la temperatura estable.

### Procedimiento

```gcode
PID_CALIBRATE HEATER=extruder TARGET=220
SAVE_CONFIG
```

Klipper oscila la temperatura y calcula los valores óptimos K_p, K_i, K_d. Los guarda automáticamente en `printer.cfg`.

### Nuestros valores calibrados

```ini
#*# [extruder]
#*# control = pid
#*# pid_kp = 22.602
#*# pid_ki = 1.408
#*# pid_kd = 90.690
```

---

## 2. PID de la cama (pendiente)

Una vez instalado el termistor:

```gcode
PID_CALIBRATE HEATER=heater_bed TARGET=60
SAVE_CONFIG
```

---

## 3. Z_TILT_ADJUST — Nivelación dual Z

Iguala los dos motores Z para que la cama quede perfectamente horizontal.

### Procedimiento

```gcode
G28         # Home completo primero
Z_TILT_ADJUST
```

Klipper:
1. Sondea el punto izquierdo de la cama
2. Sondea el punto derecho
3. Calcula la diferencia de altura
4. Ajusta independientemente cada motor Z

### Configuración

```ini
[z_tilt]
z_positions:
    -50, 150    # Posición física del husillo izquierdo
    350, 150    # Posición física del husillo derecho
points:
    30, 150     # Punto de sondeo izquierdo
    270, 150    # Punto de sondeo derecho
speed: 50
horizontal_move_z: 5
```

---

## 4. Z Offset — Distancia boquilla-cama

Calibra la distancia exacta entre la boquilla y la cama cuando el CR Touch detecta el "cero".

### Procedimiento

```gcode
G28
PROBE_CALIBRATE
```

Usa el **método del papel**:
1. Coloca un folio debajo de la boquilla
2. Baja con `TESTZ Z=-0.05` hasta que el papel tenga ligera resistencia
3. Confirma con `ACCEPT`
4. Guarda con `SAVE_CONFIG`

```gcode
# Comandos de ajuste fino:
TESTZ Z=-0.1    # bajar 0.1mm
TESTZ Z=+0.05   # subir 0.05mm
TESTZ Z=-0.025  # micro-ajuste
```

> El z_offset real se guarda en `#*# SAVE_CONFIG`. Varía según el montaje físico del CR Touch.

---

## 5. Bed Mesh — Mapa de la cama

Mapea la superficie completa de la cama y compensa las irregularidades en tiempo real.

### Procedimiento

```gcode
G28
Z_TILT_ADJUST    # siempre antes del bed mesh
BED_MESH_CALIBRATE
SAVE_CONFIG
```

Con `probe_count: 5, 5`, el CR Touch sondea **25 puntos** en una cuadrícula 5×5 entre `mesh_min: 30,30` y `mesh_max: 970,970`.

### Activar en cada impresión

La macro START_PRINT lo hace automáticamente:

```ini
[gcode_macro START_PRINT]
gcode:
  G28
  Z_TILT_ADJUST
  BED_MESH_CALIBRATE
  # ... resto de la secuencia
```

---

## 6. Calibración del extrusor (rotation_distance)

```gcode
# 1. Temperatura mínima para extruir
M109 S200

# 2. Marcar filamento a 100mm de la entrada del extrusor
# 3. Extruir exactamente 100mm
G92 E0
G1 E100 F100

# 4. Medir cuánto filamento entró realmente (con regla)
# Ejemplo: entró 97.3mm en lugar de 100mm

# 5. Calcular nuevo rotation_distance:
# nuevo = actual × (pedido / medido)
# 4.637 × (100 / 97.3) = 4.76 → actualizar en printer.cfg
```

Nuestro valor final calibrado: **`rotation_distance: 4.637`**

---

## 7. Calibración de impresión — Piezas de prueba

Una vez calibrado el firmware, hay que verificar que la impresora produce piezas correctas dimensionalmente.

### Cubo de calibración 20×20×20mm

Imprimir un cubo de 20×20×20mm y medirlo con calibre:

```
Objetivo: 20.0mm en X, Y y Z
Tolerancia aceptable: ±0.2mm
```

Si hay desviación:
- **X/Y**: ajustar `rotation_distance` del stepper correspondiente
- **Z**: ajustar `rotation_distance` del husillo (actualmente 8mm/vuelta)
- **Extrusor (desbordamiento/hueco)**: ajustar `rotation_distance` del extrusor

### Benchy (3DBenchy)

Imprimir el modelo estándar de referencia [3DBenchy](https://www.3dbenchy.com/). Evaluar:
- Puentes sin soporte (zona de la cabina)
- Voladizos
- Texto en relieve
- Tolerancias en orificios circulares

### Archivos STL de tolerancias

Imprimir piezas de ajuste (tolerance test) para medir holguras reales entre piezas ensambladas. Útil antes de imprimir piezas funcionales con tolerancias precisas.

---

## Orden de calibración recomendado

1. Flashear firmware
2. Copiar `printer.cfg` base
3. Verificar que todos los motores se mueven correctamente
4. `PID_CALIBRATE` hotend
5. `G28` (home completo)
6. `PROBE_CALIBRATE` (z_offset)
7. `Z_TILT_ADJUST`
8. `BED_MESH_CALIBRATE`
9. Calibrar extrusor (rotation_distance)
10. `PID_CALIBRATE` cama (cuando se instale el termistor)
