# Diario del proyecto — Cronología

> De una impresora 500×500mm a 1000×1000×1000mm.

---

## Fase 1 — Primeros pasos (Abril 2026)

### Semana 1 — Componentes y primer montaje

**8 de abril** — Llegan los primeros componentes: CR Touch (ALT04) y drivers.  
📷 *Fotos del CR Touch con su cable de 5 colores y la Octopus Pro recién sacada de la caja.*

**10 de abril** — Primeras conexiones a la placa.  
- Conectamos el cable de temperatura del hotend al slot T H0
- Primera vez que encendemos la Octopus Pro
- Conexión del CR Touch a PB6 (control) y PB7 (sensor)

📷 *Fotos del proceso de conexión con la placa sobre la mesa.*

---

## Fase 2 — Configuración inicial: 500×500mm

La primera configuración fue para una cama de **500×500mm**, heredada de un diseño tipo Ender 5 Plus.

**Configuración original:**
```
position_max: 500  # X e Y
```

**Problema:** Con la cama pequeña validamos que los motores funcionaban, pero el objetivo siempre fue 1 metro de lado.

---

## Fase 3 — Problemas con el slot MOTOR 3 (Abril 2026)

Descubrimos que el **slot MOTOR 3** de la Octopus Pro estaba **defectuoso**.

- El driver TMC5160 conectado en MOTOR 3 no respondía
- Probamos el driver en otro slot y funcionó → el problema era el slot, no el driver
- Solución: mover el motor Z derecho (stepper_z1) al slot **MOTOR 5**

Ver detalles: [Slot MOTOR 3 defectuoso](../problemas/motor-z-slot-defectuoso.md)

---

## Fase 4 — Doble motor Y en paralelo

**22 de abril** — Instalamos las guías lineales MGN15R en el eje Y.

📷 *Foto de la guía MGN15R con carros y piezas impresas en naranja.*

Inicialmente el eje Y tenía **1 solo motor NEMA 17**. Para el formato 1000mm necesitábamos más fuerza y precisión → añadimos un segundo motor en **paralelo** en el slot MOTOR 2_2, usando el mismo driver TMC2209.

Ver detalles: [Eje Y dual](../problemas/eje-y-dual.md)

---

## Fase 5 — Cableado completo (23 de abril 2026)

📷 *Fotos del cableado completo de la placa: motores X/Y/Z, extrusor, CR Touch, sensores de temperatura.*

En esta fase:
- Conectamos los dos **TMC5160** (rojo) para los motores Z en MOTOR 1 y MOTOR 5
- Conectamos los **TMC2209** (azul) para X, Y y extrusor
- Instalamos el Orbiter SO3 con su motor LDO
- Primer test de movimiento de todos los ejes

---

## Fase 6 — Escalado a 1000×1000mm

Con el hardware validado en 500×500mm, actualizamos la configuración:

```python
# Antes
position_max: 500   # X e Y

# Después  
position_max: 1000  # X e Y
position_max: 1000  # Z
```

También se actualizaron los puntos de sondeo del CR Touch:
```python
# safe_z_home ahora en el centro real de la cama
home_xy_position: 500, 500

# bed_mesh actualizado para la cama completa
mesh_min: 30, 30
mesh_max: 970, 970
```

---

## Fase 7 — Husillo y guías eje Z (Mayo 2026)

**13 de mayo** — Instalación del husillo trapezoidal en Z.

📷 *Fotos del perfil de aluminio 2040 con el husillo métrico 12 y las guías lineales.*

**Dificultad:** Encontrar un husillo métrico 12 (paso 2mm × 4 entradas) de **1200mm de longitud** no era fácil. Fue uno de los cuellos de botella del proyecto.

- `rotation_distance: 8` → M12 con 4 entradas = 2mm × 4 = 8mm por vuelta
- Dos husillos independientes (uno por cada motor Z)
- El `Z_TILT_ADJUST` de Klipper permite nivelar automáticamente los dos lados

---

## Fase 8 — Calibración y puesta en marcha

Con todo el hardware montado:

1. **PID calibrado** para el hotend:
   - `pid_kp = 22.602, pid_ki = 1.408, pid_kd = 90.690`
   - Temperatura objetivo: 220°C

2. **Z_TILT_ADJUST**: corrección automática del paralelismo entre los dos motores Z

3. **Bed Mesh 5×5**: mapeo de 25 puntos de la superficie de la cama

4. **rotation_distance del extrusor**: calibrado a `4.637` (oficial SO3: 4.69, ajustado por medición)

---

## Estado actual

| Sistema | Estado |
|---------|--------|
| Eje X | ✅ Funcionando |
| Eje Y (dual) | ✅ Funcionando |
| Eje Z (dual TMC5160) | ✅ Funcionando |
| Extrusor SO3 | ✅ Funcionando |
| CR Touch | ✅ Funcionando |
| Bed Mesh | ✅ Configurado |
| Z_TILT_ADJUST | ✅ Configurado |
| Cama calefactada | ⚠️ Sin termistor instalado |
| Pantalla táctil | ⚠️ Driver pendiente |
