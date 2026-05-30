# Problema: Husillo trapezoidal métrico 12 de 1200mm — difícil de encontrar

## Descripción

Para el eje Z de 1000mm necesitábamos husillos de al menos **1200mm de longitud** (con algo de margen). La especificación:

- **Tipo:** Husillo trapezoidal / roscado
- **Diámetro:** 12mm (métrica 12)
- **Paso:** 2mm
- **Entradas:** 4 (paso efectivo = 2 × 4 = **8mm por vuelta**)
- **Longitud mínima:** 1200mm

> «Tenemos un problema para encontrar la barilla trapezoidal que soluciones nos das para conseguir una barilla de 1000mm» — conversación

## ¿Por qué este husillo específico?

### Relación con rotation_distance

En Klipper, el parámetro `rotation_distance` para Z se calcula así:

```
rotation_distance = paso × entradas = 2mm × 4 = 8mm/vuelta
```

Un husillo T8 (8mm diámetro, 2mm paso, 4 entradas) también daría 8mm/vuelta, pero con diámetro 8mm se habría doblado en 1m de longitud.

Con **métrica 12** y 1.2m de longitud, el husillo tiene suficiente rigidez para no flectar.

### Por qué 4 entradas y no 1

Un husillo de 1 entrada con paso 2mm daría `rotation_distance = 2`. Esto significa:
- Mucha resolución (bueno)
- Velocidad Z muy lenta (malo — escalar 1m llevaría mucho tiempo)

Con 4 entradas: `rotation_distance = 8`. Velocidad Z máxima de 5mm/s = 5mm/s × 60s = **300mm/min** — manejable.

## Dónde encontrarlo

Proveedores que suelen tener husillos T12 de 4 entradas en 1200mm:
- Proveedores de CNC en España (Servocity, Ooznest)
- Ali Express (envío lento pero mucho stock)
- Tiendas de automatización industrial

> Asegurarse de especificar: **D=12mm, paso=2mm, 4 entradas (4-start), longitud=1200mm**

## Configuración resultante

```ini
[stepper_z]
rotation_distance: 8   # 2mm paso × 4 entradas = 8mm/vuelta
microsteps: 16
```

Con `microsteps: 16`:
```
Resolución = 8mm / (200 pasos × 16) = 0.0025mm = 2.5 micras por paso
```
