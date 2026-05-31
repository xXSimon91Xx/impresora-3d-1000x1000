# Diario del proyecto

> Cronología completa: desde los primeros componentes en febrero hasta la impresora de 1m³ funcionando.

---

## Fase 0 — Recepción de componentes y primeras inspecciones (Febrero 2026)

### 25 de febrero — Llega la Octopus Pro

Recibimos la BTT Octopus Pro V1.1 y la inspeccionamos antes de conectar nada.

![Trasera Octopus Pro](../fotos/00-prototipo-inicial/IMG_20260225_190124.jpg)
*Vista trasera de la placa recién recibida. Se pueden leer claramente todos los slots: MOTOR0, MOTOR1, MOTOR2_1, MOTOR2_2... hasta MOTOR7. Versión: 1.1.*

También inspeccionamos la zona del USB-C y el conector de la fuente de alimentación.

**Primer paso:** Identificar todos los slots de driver y planificar qué motor va en cada slot antes de conectar nada.

---

## Fase 1 — Montaje de la estructura (Febrero–Marzo 2026)

El primer paso real del proyecto fue cortar y montar el **marco de perfiles de aluminio 2040**.

### Materiales de la estructura
- Perfiles de aluminio 2040 cortados a medida (ver [lista de materiales](../hardware/lista-materiales-estructura.md))
- Juntas cruzadas impresas en 3D (verde) para unir los perfiles en esquinas y centros
- Tornillería M5 con tuercas T en ranura del perfil

### Piezas impresas para la estructura

Durante esta fase se imprimieron todas las piezas en 3D necesarias:

![Piezas en aula](../fotos/03-piezas-impresas/piezas-impresas-todas-aula.jpeg)
*Todas las piezas impresas listas en el aula: soportes de motor verdes y guías grises.*

![Soportes grandes NEMA23](../fotos/03-piezas-impresas/soportes-nema23-grandes.jpeg)
*Soportes para los 4 motores NEMA 23 del eje Z.*

![Juntas y tensores](../fotos/03-piezas-impresas/juntas-cruzadas-soportes.jpeg)
*Juntas cruzadas y tensores de correa GT2.*

---

## Fase 2 — Primera electrónica: testeo de la placa (Abril 2026)

### 8 de abril — Primeras conexiones

Conectamos los primeros componentes a la Octopus Pro para probar que la placa funcionaba antes de montarla en la estructura.

![CR Touch ALT04](../fotos/02-electronica/IMG_20260408_193645.jpg)
*CR Touch modelo ALT04 con sus 5 cables de colores.*

![Cable hotend etiquetado](../fotos/02-electronica/cable-thermistor-heater-etiquetado.jpeg)
*Cables del hotend identificados: termistor ATC Semitec 104NT-4 y calentador cerámico 24V 72W.*

![Octopus Pro overview](../fotos/02-electronica/IMG_20260408_193713.jpg)
*Vista general de la placa con drivers instalados: TMC5160 rojos (Z) y TMC2209 azules (X, Y, extrusor).*

### 10 de abril — Primer arranque con CR Touch

Conectamos el CR Touch a la placa y arrancamos Klipper por primera vez.

![Primer test electronica](../fotos/05-montaje-electronica/primer-test-electronica.jpeg)
*Placa conectada a la fuente de alimentación y al CR Touch para el primer test.*

**Problema encontrado:** Klipper no arrancaba porque faltaba `z_offset: 0` en la sección `[bltouch]`.  
→ Ver [crtouch-z-offset.md](../problemas/crtouch-z-offset.md)

### 10 de abril — Test de motores

![Conexion CR Touch](../fotos/02-electronica/conexion-crtouch-cables.jpeg)
*Conectando los cables del CR Touch a los pines PB6 y PB7 de la Octopus Pro.*

---

## Fase 3 — Problemas de hardware (Abril 2026)

### Slot MOTOR 3 defectuoso

Descubrimos que el slot MOTOR 3 de la placa estaba **defectuoso de fábrica**. El motor Z derecho no respondía.

**Diagnóstico:** Probamos el driver en otros slots — funcionaba. Probamos el motor en otros slots — funcionaba. El slot MOTOR 3 estaba muerto.

**Solución:** Mover el motor Z derecho al slot **MOTOR 5** y actualizar `printer.cfg`.

→ Ver [motor-z-slot-defectuoso.md](../problemas/motor-z-slot-defectuoso.md)

### Motor Y no se movía

El motor Y no respondía. Diagnóstico rápido: **el cable JST no estaba bien conectado**.

**Lección:** Antes de cambiar configuración, revisar siempre el cable físico.

→ Ver [eje-y-dual.md](../problemas/eje-y-dual.md)

---

## Fase 4 — Cableado completo (23 de abril 2026)

![Cableado completo en progreso](../fotos/05-montaje-electronica/cableado-completo-en-progreso.jpeg)
*Sesión de cableado: todos los motores, sensores y calefactores conectados a la placa.*

![Setup completo con Fluidd](../fotos/05-montaje-electronica/setup-octopus-cb1-fluidd.jpeg)
*Setup de trabajo: Octopus Pro + CB1 + teclado + pantalla con Fluidd. La tablet naranja es la pantalla KlipperScreen.*

### Problemas de ventilación

El ventilador del heatsink del SO3 no se apagaba nunca. La causa: estaba configurado como `[fan]` (ventilador de capa) en lugar de `[heater_fan]`.

→ Ver [ventilador-hotend.md](../problemas/ventilador-hotend.md)

---

## Fase 5 — Primer arranque de Klipper

![Fluidd configuración inicial](../fotos/05-montaje-electronica/fluidd-config-error-pantalla.jpeg)
*Primer arranque con errores de configuración. Fluidd muestra los avisos de printer.cfg que hay que corregir antes de poder mover nada. La tablet naranja (KlipperScreen) ya está activa.*

![Fluidd funcionando](../fotos/05-montaje-electronica/fluidd-funcionando-primer-arranque.jpeg)
*¡Klipper funcionando! Fluidd mostrando la interfaz completa en el monitor Dell. A la izquierda el rack de electrónica con todo conectado.*

![Setup completo con KlipperScreen](../fotos/05-montaje-electronica/fluidd-setup-klipperscreen-completo.jpeg)
*Setup de desarrollo completo: monitor Dell con Fluidd, KlipperScreen naranja en la mesa y toda la electrónica montada a la izquierda.*

En el monitor se ve la interfaz de Fluidd con:
- Temperatura del hotend y la cama
- Controles de movimiento XYZ
- Consola para comandos G-code
- Estado de la impresora

La tablet naranja en la mesa es la **BTT KlipperScreen** — pantalla táctil para controlar la impresora sin necesidad de un PC.

---

## Fase 6 — Montaje en la estructura (Mayo 2026)

### 22 de abril — Guías lineales y carro

Instalamos las guías lineales MGN15R en el eje Y:

![Guía lineal montada](../fotos/01-estructura/IMG_20260422_173948.jpg)
*Guía MGN15R con carro instalada en el perfil de aluminio 2040.*

![Soporte motor con polea](../fotos/01-estructura/soporte-motor-polea-perfil-aluminio.jpeg)
*Soporte de motor impreso (verde) montado sobre el perfil de aluminio 2040. La polea loca naranja guía la correa GT2. En la parte superior se ven los tensores impresos también en naranja.*

### 13 de mayo — Husillo y eje Z

Instalamos los husillos trapezoidales M12 de 1200mm con sus soportes impresos:

![Husillo y perfil](../fotos/01-estructura/IMG_20260513_175847.jpg)
*Husillo M12 (4 entradas, 8mm/vuelta) instalado en el perfil vertical del eje Z.*

![Soporte husillo](../fotos/01-estructura/IMG_20260513_175857.jpg)
*Soporte impreso en azul para el extremo superior del husillo.*

---

## Fase 7 — Integración final del cabezal

![Cables extrusor montados](../fotos/01-estructura/cables-extrusor-etiquetados-montados.jpeg)
*Mazo de cables del cabezal ya instalado en la impresora. Los cables están etiquetados como "EXTRUSOR" para facilitar el mantenimiento.*

---

## Estado actual

| Sistema | Estado |
|---------|--------|
| Marco estructura | ✅ Montado |
| Guías lineales MGN15R | ✅ Instaladas |
| Eje X | ✅ Funcionando |
| Eje Y dual (2 motores paralelo) | ✅ Funcionando |
| Eje Z dual (TMC5160 + NEMA23) | ✅ Funcionando |
| Extrusor SO3 | ✅ Funcionando |
| CR Touch + Bed Mesh 5×5 | ✅ Configurado |
| Z_TILT_ADJUST | ✅ Configurado |
| Klipper + Fluidd + KlipperScreen | ✅ Funcionando |
| Cama calefactada | ⚠️ Sin termistor |
| Archivos 3D de piezas | ⏳ Pendiente subir |

---

## Pendientes

- [ ] Instalar termistor en cama calefactada
- [ ] Subir archivos 3D de las piezas impresas al repositorio
- [ ] Completar calibración final (PID cama, Z offset definitivo)
- [ ] Subir planos del marco en PDF
