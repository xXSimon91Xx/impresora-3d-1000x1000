# Cableado — CR Touch (Sonda Z virtual)

> El CR Touch reemplaza el endstop físico de Z y permite nivelación automática de la cama.

---

## Modelo

**Creality CR Touch — ALT04** (CX:24040700098C)

![CR Touch ALT04](../../fotos/02-electronica/IMG_20260408_193645.jpg)
*CR Touch modelo ALT04 con su cable de 5 colores. El lado izquierdo es el conector de la placa (JST), el derecho el del sensor.*

---

## Pinout — Los 5 cables

| Cable | Color | Función | Pin Octopus Pro |
|-------|-------|---------|-----------------|
| 1 | Azul | Señal de sensor (toque) | **PB7** |
| 2 | Rojo | GND | GND |
| 3 | Amarillo | Control del servo (deploy/retract) | **PB6** |
| 4 | Negro | 5V alimentación | 5V |
| 5 | Blanco | GND | GND |

> Los colores pueden variar ligeramente entre fabricantes. Verificar con el manual incluido en la caja.

---

## Configuración en printer.cfg

```ini
[bltouch]
sensor_pin: ^PB7           # ^ = pullup. Señal de detección de toque
control_pin: PB6           # Control del servo
pin_up_touch_mode_reports_triggered: False
x_offset: 0               # Offset respecto a la boquilla (calibrar físicamente)
y_offset: 0               # Offset Y (calibrar físicamente)
z_offset: 0               # Calibrar con PROBE_CALIBRATE
speed: 5.0                # Velocidad de sondeo (lenta para precisión)
lift_speed: 10.0
samples: 2                # 2 muestras por punto (promedio)
sample_retract_dist: 5.0
```

### Homing seguro con CR Touch

```ini
[safe_z_home]
home_xy_position: 500, 500  # Centro de la cama 1000×1000
speed: 100
z_hop: 10                   # Sube 10mm antes de moverse (evita roces)
z_hop_speed: 10
```

El homing en Z funciona así:
1. Hace home en X e Y primero
2. Se mueve al centro de la cama (500, 500)
3. Baja lentamente hasta que el CR Touch detecta la cama
4. Esa posición es el "cero" de Z

---

## Bed Mesh — Mapa de nivelación 5×5

```ini
[bed_mesh]
speed: 100
horizontal_move_z: 5
mesh_min: 30, 30          # Esquina mínima del mapa
mesh_max: 970, 970        # Esquina máxima (margen respecto a 1000)
probe_count: 5, 5         # 5×5 = 25 puntos de sondeo
mesh_pps: 2, 2            # Interpolación entre puntos
algorithm: bicubic        # Algoritmo para superficies complejas
```

El mapa de 25 puntos compensa irregularidades de la cama. Klipper ajusta la altura Z en tiempo real durante la impresión para que la primera capa quede uniforme en toda la superficie de 1m².

---

## Calibración del z_offset

Procedimiento inicial:

```
# 1. Calentar a temperatura de impresión
M104 S210
M190 S60

# 2. Hacer home completo
G28

# 3. Iniciar calibración
PROBE_CALIBRATE

# 4. Bajar manualmente con papel (método del papel)
TESTZ Z=-0.1   # bajar 0.1mm
TESTZ Z=+0.05  # subir 0.05mm
# repetir hasta que el papel tenga resistencia mínima

# 5. Guardar
ACCEPT
SAVE_CONFIG
```

El valor se guarda automáticamente en el bloque `#*# SAVE_CONFIG` del `printer.cfg`.

---

## Por qué CR Touch en lugar de endstop mecánico en Z

Con una cama de 1m², es imposible nivelarla manualmente con suficiente precisión. El CR Touch permite:

1. **Nivelación automática** (`Z_TILT_ADJUST`) — iguala los dos motores Z
2. **Mapa de cama** (`BED_MESH_CALIBRATE`) — compensa ondulaciones de la superficie
3. **Reproducibilidad** — cada impresión empieza exactamente a la misma altura
