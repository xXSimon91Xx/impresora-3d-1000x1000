# Problema: Calentador subía solo a 220°C sin comando

## Descripción del problema

Al encender la impresora, el hotend empezaba a calentarse **solo** sin que se hubiese dado ningún comando. La temperatura subía hasta 220°C de forma autónoma.

> «No, no se para de calentar, ha llegado hasta los 220 grados sin hacer nada» — conversación

## Causa

El pin del calentador (`heater_pin`) estaba configurado en un pin incorrecto. El HE0 (heater element 0) de la Octopus Pro no estaba cableado correctamente:

> «Oye sigo teniendo el mismo problema... B:-99.4 /0.0 T0:74.9 /0.0» — conversación  
> «No se porque pero el HE0 no funciona» — conversación

El problema era que el calentador físico estaba conectado a un pin diferente al configurado en `printer.cfg`. Cuando el pin configurado no coincide con el real, puede quedar en estado activo por defecto.

## Solución

Verificar el cableado físico del calentador:

1. Apagar la impresora inmediatamente
2. Comprobar a qué conector HE (0, 1, 2, o 3) está conectado el calentador del hotend
3. Verificar que `heater_pin` en printer.cfg coincide:

```ini
[extruder]
heater_pin: PA3   # PA3 = HE0 en la Octopus Pro V1.1
```

Mapa de pines HE de la Octopus Pro V1.1:
- HE0 → PA3
- HE1 → PA4  
- HE2 → PA5  (verificar con el manual de la placa)
- HE3 → (verificar)

4. Si el calentador sigue subiendo tras confirmar el pin, revisar si hay cortocircuito en el termistor o en el MOSFET de la placa

## Resultado

Con el pin correcto (`PA3`) y el cableado verificado, el hotend solo calienta cuando se le ordena explícitamente con `M104` o `M109`.

## Lección

**Siempre verificar físicamente a qué pin está conectado cada componente.** La placa tiene múltiples slots similares (HE0, HE1, HE2, HE3) y es fácil conectar en el incorrecto. Una discrepancia entre el cable físico y la config puede causar comportamientos peligrosos.
