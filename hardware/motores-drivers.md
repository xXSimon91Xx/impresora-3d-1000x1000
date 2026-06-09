# Motores y Drivers — Por qué elegimos cada uno

> Esta sección explica la elección de motores y drivers para cada eje, con el razonamiento técnico detrás de cada decisión.

---

## Resumen rápido

| Eje | Motor | Driver | Corriente | Protocolo |
|-----|-------|--------|-----------|-----------|
| X | NEMA 17 | TMC2209 | 0.6A run | UART |
| Y (×2 en paralelo) | NEMA 17 | TMC2209 | 1.0A run | UART |
| Z izquierdo | NEMA 23 | TMC5160-T | 2.0A run | SPI |
| Z derecho | NEMA 23 | TMC5160-T | 2.0A run | SPI |
| Extrusor | LDO-36STH20 | TMC2209 | 0.85A run | UART |

---

## TMC2209 vs TMC5160 — ¿Cuándo usar cada uno?

### TMC2209 (UART)

```
Corriente max. recomendada: ~1.2A continuo
Protocolo:                  UART (1 pin de datos)
StealthChop:                Sí
StallGuard:                 Sí (detección sin fin de carrera)
Precio:                     ~3-5€/unidad
```

**Cuándo usarlo:** Motores NEMA 17 con corrientes bajas-medias (X, Y, extrusor).

**Ventaja clave:** Un solo pin UART para configuración y telemetría. El driver reporta temperatura, corriente y carga en tiempo real a Klipper.

**Limitación:** No aguanta 2A de forma continua. Si lo usas con un NEMA 23, el chip se calienta excesivamente y puede morir.

---

### TMC5160-T Pro (SPI)

```
Corriente max:   4.4A peak / 3A RMS
Protocolo:       SPI (4 cables: MISO, MOSI, SCK, CS)
StealthChop:     Sí
Tensión:         8–60V
Color:           Rojo (en nuestra placa)
Precio:          ~8-12€/unidad
```

**Cuándo usarlo:** Motores NEMA 23 que necesitan 2A o más.

**Ventaja clave:** Puede manejar corrientes altas sin calentarse. Los TMC5160 en nuestra placa corren a **2A run / 1A hold** para los NEMA 23 del eje Z — los TMC2209 no aguantarían esto.

**Configuración SPI compartida:** Todos los TMC5160 comparten los mismos pines MISO/MOSI/SCK, pero cada uno tiene su propio `cs_pin` (Chip Select):
```ini
[tmc5160 stepper_z]
cs_pin: PD11    # MOTOR 1
spi_software_miso_pin: PA6
spi_software_mosi_pin: PA7
spi_software_sclk_pin: PA5

[tmc5160 stepper_z1]
cs_pin: PE4     # MOTOR 5
# mismos pines SPI
```

---

## NEMA 17 vs NEMA 23 — ¿Por qué usamos NEMA 23 en Z?

### NEMA 17 (ejes X, Y, extrusor)

```
Frame size: 42 × 42 mm
Par típico: 40–60 N·cm
Corriente:  0.4–1.7A
Peso:       ~250-350g
```

Para X e Y: el carro solo lleva el cabezal de impresión (ligero), así que NEMA 17 es más que suficiente.

### NEMA 23 (eje Z)

```
Frame size: 57 × 57 mm
Par típico: 100–200 N·cm
Corriente:  2–4A
Peso:       ~600-800g
```

**¿Por qué NEMA 23 en Z?**  
En esta impresora **la cama no se mueve** — el eje Z levanta el pórtico completo (eje X + carro + cabezal extrusor), que pesa varios kilos. El eje Z tiene que:
1. Levantar ese peso contra la gravedad en cada cambio de capa
2. Mantener el pórtico estable durante la impresión
3. Hacer micro-ajustes precisos e independientes para el `Z_TILT_ADJUST`

Un NEMA 17 se quedaría corto de torque y perdería pasos al subir.

---

## Eje Y — Doble motor en paralelo con 1 solo driver

### Por qué 2 motores en Y

Con **1000mm de recorrido en Y**, un único motor en un extremo generaría un par de fuerzas que desplazaría lateralmente el carro durante el movimiento. Con un motor en cada lado del eje, el empuje es simétrico: el carro se mantiene centrado y alineado sin desviaciones laterales ni vibraciones desiguales.

### Por qué 1 solo driver para 2 motores

Los dos motores van conectados **en paralelo** al mismo slot (MOTOR 2_1 y MOTOR 2_2 físicamente conectados al driver del 2_1). Ventajas:
- Ahorramos un slot de driver
- Los dos motores se mueven **exactamente igual** (mismo driver = mismas señales)
- No hay riesgo de desincronización

**Inconveniente:** La corriente se divide entre los dos motores. Con `run_current: 1.0A`, cada motor recibe 0.5A — suficiente para NEMA 17 en Y.

```ini
[stepper_y]
step_pin: PF11    # MOTOR 2_1 — driver único
# El segundo motor (MOTOR 2_2) se conecta en paralelo al mismo slot
# No tiene sección propia en printer.cfg
```

### ¿Por qué NO usar Z_TILT para Y?

Podríamos haber puesto 2 drivers independientes para Y (como hacemos en Z con Z_TILT), pero en Y no necesitamos corrección de inclinación — la correa ya asegura que ambos extremos estén sincronizados.

---

## Extrusor — LDO Smart Orbiter v3.0 con TMC2209

El extrusor es especial: queremos **máxima respuesta** en los cambios de velocidad (aceleración/deceleración rápida del filamento).

Por eso, el TMC2209 del extrusor tiene **StealthChop desactivado**:

```ini
[tmc2209 extruder]
stealthchop_threshold: 0   # 0 = StealthChop SIEMPRE desactivado
```

StealthChop hace el motor más silencioso pero menos reactivo. Para el extrusor preferimos precisión sobre silencio.

---

## Configuración de corrientes

La corriente correcta para cada driver se calcula así:

```
run_current = I_nominal_motor × 0.707
```

Por ejemplo, para el SO3 (corriente nominal del motor: 1A):
```
run_current = 1.0 × 0.707 ≈ 0.707A → ponemos 0.85A (ligeramente superior para SO3)
```

Para los NEMA 23 (corriente nominal: ~2.8A), ponemos 2.0A — limitado por el límite de la fuente de alimentación y el calor generado.

| Motor | Corriente nominal | run_current | hold_current |
|-------|-----------------|-------------|--------------|
| X (NEMA17) | ~1.5A | 0.6A | 0.3A |
| Y (NEMA17 ×2 paralelo) | ~1.5A | 1.0A (total) | 0.5A |
| Z (NEMA23) | ~2.8A | 2.0A | 1.0A |
| Extrusor (LDO) | ~1.0A | 0.85A | 0.1A |
