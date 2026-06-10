# Proyectos a futuro

> Ideas y mejoras planificadas para continuar el desarrollo de la máquina.  
> Algunas son sencillas de implementar; otras son proyectos completos en sí mismos.

---

## Mejoras mecánicas

### 4 cabezales de impresión con cambio de color
Implementar un sistema de múltiples extrusores (4 cabezales) para impresión multicolor o multi-material en una sola pieza. Requiere modificaciones en el firmware (Klipper admite multi-extrusor nativo) y el diseño de un cabezal intercambiable o de tipo "herramienta".

### 4 husillos Z (de 2 a 4)
Añadir dos husillos adicionales en el eje Z para mejorar la rigidez del sistema en una máquina de este tamaño. Con 4 puntos de apoyo se reduce la deflexión y se puede usar `Z_TILT_ADJUST` con más puntos de sondeo.

### Sistema de cadenas portacables
Sustituir los cables libres actuales por cadenas portacables (cable chains) en los ejes X e Y. Mejora la durabilidad del cableado y da un aspecto más profesional.

### Guías lineales con lubricación automática
Instalar un sistema automático de engrase de las guías MGN15R. Existen dispositivos comerciales con tubos y depósito que aplican aceite periódicamente. Aumenta la vida útil de las guías y reduce el mantenimiento manual.

---

## Mejoras de materiales

### Placa PEI en vez de cristal
Sustituir la superficie de impresión de cristal por una placa PEI magnética. El PEI ofrece mejor adherencia para PETG, ABS y ASA, y permite quitar las piezas con un simple flexado de la lámina.

### Enclosure para ABS/ASA/PP
Para poder imprimir con ABS, ASA o polipropileno de forma consistente, la máquina necesita una carcasa cerrada que mantenga la temperatura ambiente elevada y evite las corrientes de aire. Los paneles de policarbonato corrugado con imanes de neodimio ya están planificados (ver [piezas impresas](hardware/piezas-impresas.md)).

### Extrusor de pellets
Diseñar o adaptar un extrusor que funcione directamente con pellets de plástico en lugar de filamento bobinado. Reduce el coste del material y abre la posibilidad de usar plástico reciclado.

### Boquilla 1.5 mm para impresión rápida
Para piezas grandes donde el detalle no es crítico, una boquilla de 1.5 mm acelera enormemente los tiempos de impresión. Actualmente la máquina tiene boquilla de 0.4 mm.

---

## Mejoras electrónicas y de control

### Encoders en los motores
Añadir encoders a los motores paso a paso para pasar a control en bucle cerrado. Elimina la pérdida de pasos y permite recuperar la posición si hay un obstáculo. Especialmente relevante en una máquina tan grande donde los errores acumulados son mayores.

### Sistema de recuperación de corte de luz
Implementar y probar la macro de power-loss recovery de Klipper. Cuando se va la luz, la posición se guarda en la SD; al volver la corriente, la impresión puede continuar desde donde se quedó.

> **Nota técnica**: el eje Z no cae cuando se corta la corriente porque los husillos trapezoidales son auto-frenantes por fricción. Sin embargo, con piezas muy pesadas sobre el cabezal podría bajar ligeramente.

### LEDs de estado de la máquina
- 🟢 **Verde**: todo OK, imprimiendo normalmente
- 🔴 **Rojo fijo**: error crítico (temperatura, endstop)
- 🟡 **Amarillo intermitente**: aviso (filamento agotado, temperatura fuera de rango)

Implementable con una tira LED WS2812B controlada desde Klipper con `[neopixel]`.

### Más setas de emergencia
Añadir pulsadores de parada de emergencia adicionales en diferentes puntos de la máquina (actualmente solo hay uno). Para una máquina de 1m³ operada en un taller escolar, es importante poder detener la máquina desde cualquier posición.

### Magnetotérmico (protección de sobrecarga)
Instalar un interruptor magnetotérmico en la línea de alimentación de la máquina. Protege contra sobrecargas de corriente y cortocircuitos, especialmente importante cuando se instale la cama calefactada de alta potencia (4 × 500×500mm en paralelo).

### Ventiladores en la caja de electrónica
Añadir ventiladores de extracción en el panel verde donde están la Octopus Pro y el CB1. Con la cama calefactada en funcionamiento y todos los drivers cargados, la temperatura dentro del panel puede subir considerablemente.

---

## Mejoras de software y documentación

### Certificado CE
Para que la máquina pueda ser usada oficialmente en cualquier centro educativo o cedida a otros institutos, necesitaría obtener el marcado CE que acredita el cumplimiento de las directivas europeas de seguridad eléctrica y electromagnética.

### Versiones del repositorio en catalán e inglés
Traducir toda la documentación al catalán (para uso local) y al inglés (para que otros proyectos internacionales puedan replicar la máquina).

### Render 3D explodido de la máquina
Crear un render con vista explosionada de todos los componentes para que sea fácil entender el ensamblaje sin tener la máquina delante. Pendiente cuando se suban los archivos STEP del marco.

---

## Pendientes inmediatos

- [ ] Instalar 4 × camas calefactadas 500×500mm en paralelo
- [ ] Subir archivos STL y STEP de piezas impresas
- [ ] Subir planos del marco en PDF
- [ ] Calibración final: PID cama, Z offset definitivo con `PROBE_CALIBRATE`
- [ ] Probar y validar macro de power-loss recovery
