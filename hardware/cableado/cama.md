# Cableado — Cama calefactada

## Estado actual

> ⚠️ **La cama calefactada NO está instalada todavía.**  
> No existe una cama de 1000×1000mm estándar — la solución prevista es montar **4 camas calefactadas de 500×500mm** en paralelo. La configuración actual es un parche temporal para poder arrancar Klipper sin cama real.

---

## Componentes

| Componente | Estado | Pin |
|-----------|--------|-----|
| Calentador cama | ❌ Pendiente instalar | PA1 (HE_BED) |
| Termistor temperatura | ❌ Pendiente instalar | PF3 (T1) |

---

## Configuración actual (parche temporal)

```ini
[heater_bed]
heater_pin: PA1
sensor_type: EPCOS 100K B57560G104F   # placeholder — no hay sensor real
sensor_pin: PF3
control: watermark
min_temp: -110    # muy bajo para que no falle sin sensor
max_temp: 130
```

`min_temp: -110` evita que Klipper bloquee el inicio cuando el pin PF3 lee un valor fuera de rango (porque no hay termistor conectado).

---

## Instalación pendiente del termistor

Para completar la cama calefactada:

1. **Comprar:** 4 camas calefactadas estándar de **500×500mm** y montarlas cubriendo el área 1000×1000mm
2. **Comprar:** Termistor NTC 100K tipo EPCOS B57560G104F (o genérico 100K B3950) — uno por zona o uno central
3. **Montar:** Pegar termistor en la parte inferior de la placa calefactora con cinta de kapton
4. **Conectar:** Al pin PF3 (conector T1 de la Octopus Pro)
5. **Actualizar config:**

```ini
[heater_bed]
heater_pin: PA1
sensor_type: EPCOS 100K B57560G104F
sensor_pin: PF3
control: pid              # cambiar de watermark a pid
min_temp: 0               # restaurar a valor normal
max_temp: 130
```

6. **Calibrar PID de cama:**
```
PID_CALIBRATE HEATER=heater_bed TARGET=60
SAVE_CONFIG
```

---

## Cableado del calentador

La cama de 1m² necesita un calentador de alta potencia (típicamente 1000-2000W). Los cables van directamente al conector BED-OUT de la Octopus Pro, que alimenta el MOSFET que controla la cama.

```
Cama calefactora:
  Cable 1 (positivo) → BED-OUT +
  Cable 2 (negativo) → BED-OUT -

Alimentación BED-POWER separada:
  BED-POWER + → fuente 24V (rail dedicado)
  BED-POWER - → GND fuente
```

> La Octopus Pro tiene entradas de alimentación **separadas** para la cama (BED-POWER) y para el resto de la placa (POWER). Es importante mantenerlas separadas para evitar ruido en las señales de los motores.
