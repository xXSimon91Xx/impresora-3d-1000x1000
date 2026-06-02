# Lista de componentes (BOM)

> Por qué elegimos cada pieza — el razonamiento detrás de cada elección.

---

## Electrónica principal

### BTT Octopus Pro V1.1 (STM32H723)

| Campo | Valor |
|-------|-------|
| **MCU** | STM32H723ZET6 (ARM Cortex-M7, 550 MHz) |
| **Slots de driver** | 8 (usamos 5: X, Y, Z, Z1, extrusor) |
| **Protocolo drivers** | UART (TMC2209) y SPI (TMC5160) simultáneo |
| **Entradas temperatura** | 4 termistores + 2 PT100/PT1000 |
| **Salidas calefactor** | 4 (usamos 2: hotend + cama) |
| **Salidas ventilador** | 6 PWM + 2 siempre encendidos |
| **Alimentación motores** | 12–60V independiente |
| **Precio aproximado** | ~60€ |

**¿Por qué la elegimos?**  
Necesitábamos una placa con capacidad para **8 drivers** (el formato 1×1m tiene múltiples motores), soporte simultáneo para TMC2209 (UART) y TMC5160 (SPI), y compatibilidad nativa con Klipper. La Octopus Pro cumple todo eso y tiene una comunidad enorme.

---

### BTT CB1 (ordenador de control)

| Campo | Valor |
|-------|-------|
| **CPU** | ARM Cortex-A55 (quad-core) |
| **RAM** | 1 GB DDR4 |
| **Almacenamiento** | microSD |
| **Sistema operativo** | Armbian + Klipper |
| **Interfaz web** | Fluidd |
| **Conexión con Octopus** | USB-C |

**¿Por qué no una Raspberry Pi?**  
El CB1 es más barato, monta directamente en la Octopus Pro (sin cables adicionales) y viene preconfigurado para Klipper. También es más fácil de conseguir en 2025-2026 que una RPi.

---

## Drivers de motor

### TMC2209 (UART) — para X, Y y extrusor

| Parámetro | Valor |
|-----------|-------|
| **Corriente máxima** | 2A (run), 1.2A recomendado continuo |
| **Protocolo** | UART (1 solo cable de datos) |
| **StealthChop** | Sí (modo silencioso) |
| **Detección de carga** | StallGuard 4 |
| **Color en nuestra placa** | Azul |

Usamos TMC2209 para los ejes que usan **NEMA 17**: velocidad suficiente, bajo consumo, modo silencioso perfecto.

### TMC5160-T Pro (SPI) — para Z izquierdo y Z derecho

| Parámetro | Valor |
|-----------|-------|
| **Corriente máxima** | 4.4A peak, 3A RMS |
| **Protocolo** | SPI (4 cables) |
| **StealthChop** | Sí |
| **Tensión** | 8–60V |
| **Color en nuestra placa** | Rojo |

**¿Por qué TMC5160 para Z y no TMC2209?**  
Los motores Z son **NEMA 23**, que necesitan **2A de corriente continua**. El TMC2209 solo aguanta ~1.2A de forma fiable. Si usáramos TMC2209 en Z, los motores se calentarían, perderían pasos, o el driver moriría.

---

## Motores

### NEMA 17 — Ejes X, Y (×3) y extrusor (×1)

- Paso: 1.8° (200 pasos/vuelta)
- Corriente run: 0.6A (X), 1.0A (Y), 0.85A (extrusor)
- Volumen 1m: necesitamos **2 motores en Y** en paralelo (ver [eje Y](cableado/eje-y.md))

### NEMA 23 — Eje Z (×2)

- Mayor fuerza de empuje para sostener la cama de 1m²
- Paso: 1.8°
- Corriente run: 2.0A (requiere TMC5160)
- **Dual Z independiente**: cada motor tiene su propio driver para poder hacer `Z_TILT_ADJUST`

### LDO-36STH20-1004AHG — Extrusor (dentro del SO3)

- Motor compacto integrado en el Orbiter SO3
- Corriente run: 0.85A

---

## Extrusor — LDO Smart Orbiter v3.0

| Campo | Valor |
|-------|-------|
| **Modelo** | LDO Smart Orbiter v3.0 (diseñado por Dr. Robert Lőrincz) |
| **Tipo** | Direct drive con reducción 7.5:1 |
| **rotation_distance calibrado** | 4.637 mm/rev |
| **Motor** | LDO-36STH20-1004AHG |
| **Sensor filamento** | Integrado |
| **Botón descarga** | Integrado |

**¿Por qué el Smart Orbiter v3.0?**  
Alta relación de reducción (7.5:1), muy ligero para un direct drive, sensor de filamento integrado y botón de descarga integrado. Es uno de los extrusores direct drive más recomendados para impresoras de gran formato.

---

## Sonda de nivelación — CR Touch (ALT04)

| Campo | Valor |
|-------|-------|
| **Modelo** | Creality CR Touch ALT04 |
| **Tipo** | Punta metálica servo-controlada |
| **Protocolo** | 5 hilos (compatibe BL Touch) |
| **Repetibilidad** | ±0.005 mm |

**¿Por qué CR Touch y no un final de carrera en Z?**  
Con una cama de 1m² es imposible nivelarla perfectamente a mano. El CR Touch + Klipper `BED_MESH_CALIBRATE` mapea 25 puntos (5×5) y compensa las imperfecciones de la cama automáticamente durante la impresión.

---

## Estructura mecánica

| Componente | Especificación |
|-----------|----------------|
| **Perfiles aluminio** | item 40×80mm, serie XMS, art. 0.0.669.99 |
| **Guías lineales** | MGN15R (carros R = rodamientos de bolas recirculantes) |
| **Husillo Z** | Métrico 12, paso 2mm, 4 entradas (= 8mm/rev) |
| **Correa XY** | GT2, 2mm paso, polea 20 dientes (= 40mm/rev) |

**¿Por qué MGN15R y no varillas lisas?**  
Para 1m de recorrido, las varillas lisas tienden a flectar en el centro. Las guías MGN15R son más rígidas y precisas.

**¿Por qué husillo T12 (métrico 12, 4 entradas)?**  
- `rotation_distance = paso × entradas = 2mm × 4 = 8mm/vuelta`
- Mayor avance por vuelta que T8 → menos pasos por mm, pero más velocidad Z
- Suficiente para una impresora (no necesitamos velocidad extrema en Z)

---

## Links de compra

- [BTT Octopus Pro V1.1 + CB1 (Amazon ES)](https://www.amazon.es/dp/B0BN7B6KV6)
- [Componente 2 (Amazon ES)](https://www.amazon.es/dp/B0CLLRG551)
- [Componente 3 (Amazon ES)](https://www.amazon.es/dp/B0D97P6Z64)
- [BTT Octopus Pro — Repositorio oficial](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro)
