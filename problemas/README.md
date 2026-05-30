# Problemas encontrados y soluciones

> Esta sección documenta todos los problemas reales que encontramos durante la construcción y cómo los resolvimos. Está ordenada cronológicamente.

---

## Índice

| # | Problema | Impacto | Estado |
|---|----------|---------|--------|
| 1 | [Slot MOTOR 3 defectuoso](motor-z-slot-defectuoso.md) | Z derecho no funcionaba | ✅ Resuelto |
| 2 | [Cable motor Y mal conectado](eje-y-dual.md) | Motor Y no se movía | ✅ Resuelto |
| 3 | [Ventilador hotend no se apagaba](ventilador-hotend.md) | Ventilador siempre encendido | ✅ Resuelto |
| 4 | [Endstop Z — configuración incorrecta](endstop-z.md) | Z no paraba en tope superior | ✅ Resuelto |
| 5 | [CR Touch — z_offset obligatorio](crtouch-z-offset.md) | Klipper no arrancaba | ✅ Resuelto |
| 6 | [Termistor cama no instalado](termistor-cama.md) | Temperatura cama incorrecta | ⚠️ Pendiente |
| 7 | [Husillo de 1.2m difícil de encontrar](husillo-trapezoidal.md) | Cuello de botella en construcción | ✅ Resuelto |
| 8 | [Calentador subía solo a 220°C](calentador-runaway.md) | Riesgo de thermal runaway | ✅ Resuelto |

---

## Lecciones aprendidas

1. **Probar el hardware antes de montarlo** — el slot MOTOR 3 estaba defectuoso de fábrica. Siempre testar los slots con un motor y un driver antes de asumir que funciona.

2. **El cable siempre es sospechoso** — cuando un motor no se mueve, antes de cambiar la configuración, comprueba el cableado físico.

3. **UART vs SPI no es intercambiable** — TMC2209 y TMC5160 usan protocolos completamente diferentes. Mezclarlos en la configuración causa errores silenciosos difíciles de depurar.

4. **Klipper exige `z_offset` en bltouch antes de arrancar** — añade `z_offset: 0` temporalmente y calibra después con `PROBE_CALIBRATE`.

5. **La cama calefactada necesita termistor real** — usar `min_temp: -110` como parche sirve para probar, pero no da información real de temperatura.

6. **`dir_pin: !PF0`** — la `!` antes del pin invierte la dirección. Cuando un motor gira al revés, añade o quita la `!` en lugar de mover los cables.
