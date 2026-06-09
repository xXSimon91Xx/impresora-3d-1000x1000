# Problema: Endstop Z de seguridad — Configuración incorrecta

## Descripción del problema

Instalamos un **final de carrera mecánico** en el tope superior del eje Z como parada de emergencia. Si el eje Z sube demasiado (fallo de software o calibración incorrecta), el endstop detiene la máquina para evitar daños.

El problema: **no sabíamos a qué conector de la placa conectarlo**, y varios intentos de configuración no funcionaron.

## La confusión con los conectores

La BTT Octopus Pro tiene varios grupos de conectores que parecen similares:
- Conectores de endstop (STOP) — para los finales de carrera de homing
- Conectores de temperatura (T0–T3) — para termistores NTC
- Conectores de calentador (HE0–HE3) — para hotends

Los conectores T (temperatura) y los STOP (endstop) usan **2 pines** y tienen aspecto similar.

> «No chaval, te estás confundiendo, no vamos a utilizar la zona de STOP sino la de T, porque son de 2 pines» — conversación

> «Claro, caraulo, yo tengo lo siguiente: en J44 la cama que no hay nada, en J45 el extrusor, en J46 el extrusor...» — conversación

### Intentos fallidos

1. **Conectar al endstop STOP** — la Octopus Pro no tiene suficientes slots de STOP libres para añadir un endstop de seguridad extra
2. **Configurar como endstop virtual** — complicaba el homing

## Solución

Usar el conector **T3 (PF7)** — un conector de temperatura libre — como entrada digital para el endstop de seguridad.

Esto se hace con `[gcode_button]` en lugar de `[endstop_pin]`:

```ini
[gcode_button endstop_z_max]
pin: ~PF7           # ~ = pulldown activado. J49 (T3) pin
press_gcode:
  M118 EMERGENCIA - Endstop Z maximo activado
  M112             # M112 = parada de emergencia inmediata
```

`M112` corta el movimiento y desactiva todos los calefactores instantáneamente.

> **¿Por qué `~` (pulldown)?**  
> Se cambió de `^` (pullup) a `~` (pulldown) tras pruebas físicas. Con pullup el pin daba activaciones falsas. Con pulldown (`~`, resistencia a GND interna del STM32) el final de carrera funciona de forma estable.  
> `~` en Klipper = activar resistencia pulldown interna del MCU (STM32H723 lo soporta).

## Cableado

El final de carrera mecánico (2 pines) se conecta a:
- Pin de señal → T3 (PF7)
- GND → GND del mismo conector

```
Octopus Pro T3:
  PF7  ────── Señal endstop
  GND  ────── GND endstop
```

## Resultado

El eje Z se para inmediatamente si el tope superior es alcanzado, sin interferir con el sistema de homing normal.

## Lección

- Los conectores T de temperatura pueden usarse como entradas digitales generales con `[gcode_button]`
- Probar pullup (`^`) y pulldown (`~`) según el cableado físico — en este caso pulldown resultó más estable
- `M112` es la parada de emergencia más segura — no `M0` ni `CANCEL_PRINT`
