# Problema: Slot MOTOR 3 defectuoso en la Octopus Pro

## Descripción del problema

El eje Z usa **dos motores NEMA 23 independientes** para poder nivelar la cama automáticamente con `Z_TILT_ADJUST`. La configuración inicial asignaba:

- Z izquierdo → MOTOR 1
- Z derecho → MOTOR 3

El Z derecho (MOTOR 3) **no respondía**. El motor no se movía, no había error de Klipper, y el driver parecía funcionar.

## Diagnóstico

Proceso de eliminación:

1. **¿Es el driver?** → Conectamos el TMC5160 del MOTOR 3 al slot MOTOR 1. El motor funcionó. **El driver estaba bien.**

2. **¿Es el motor?** → Conectamos el motor del Z derecho al MOTOR 1. El motor funcionó. **El motor estaba bien.**

3. **¿Es el slot?** → Conectamos un driver diferente al MOTOR 3. El motor seguía sin moverse. **El slot MOTOR 3 de la placa estaba defectuoso.**

> «el driver no es, puesto que he puesto el driver de Y en X y se movía X, quizá es el slot del motor, mira los comentarios de placa» — conversación de proyecto

## Solución

Mover el motor Z derecho al **MOTOR 5** (que estaba libre).

### Cambios en printer.cfg

```ini
# ANTES (con slot defectuoso)
[stepper_z1]
step_pin: PG4    # MOTOR 3 — DEFECTUOSO
dir_pin: PC1
enable_pin: !PA0

# DESPUÉS (con slot funcional)
[stepper_z1]
step_pin: PC13   # MOTOR 5 — FUNCIONAL
dir_pin: !PF0    # ! invertido para girar en la misma dirección que Z
enable_pin: !PF1
```

> **Nota sobre el `!` en `dir_pin`:** Al cambiar de slot, el motor giraba en sentido contrario al Z izquierdo. Añadiendo `!` (inversión) al `dir_pin` se corrige sin necesidad de rehacer el cableado físico.

## Resultado

Ambos motores Z funcionan correctamente con `Z_TILT_ADJUST`, que nivela los dos lados de la cama de forma independiente.

## Lección

**Siempre comprobar el slot antes de asumir que el problema es el driver o el motor.**  
La BTT Octopus Pro puede tener slots defectuosos de fábrica — no es un fallo del usuario.

Si un motor no responde:
1. Prueba el driver en otro slot
2. Prueba el motor en otro slot
3. Si ambos funcionan en otros slots → el slot original está muerto
