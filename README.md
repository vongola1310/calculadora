# 🧊 Calculadora de Costos de Impresión 3D

Calculadora de costos y precios para venta de figuras impresas en 3D, en **pesos mexicanos**.
Todo vive en un solo archivo: `index.html`. Sin dependencias, sin servidor, sin internet.

## Cómo usarla

Abre `index.html` en cualquier navegador. En el celular conviene agregarla a la pantalla de
inicio (*Compartir → Agregar a pantalla de inicio*) para abrirla como app.

Los resultados se recalculan **en vivo** conforme escribes, no hay botón de calcular.

## Qué captura

| Sección | Campos |
|---|---|
| Impresión (por placa) | horas y minutos, gramos de filamento, tipo de filamento, purga multicolor, piezas por placa |
| Extras (por pieza) | imanes $2, argollas $3, vela LED $25, pintura a mano $150/h, empaque $20 |
| Ajustes | energía (kW × $/kWh), depreciación (precio de máquina ÷ vida útil), margen, umbrales del semáforo |

Filamentos precargados: PLA $420/kg · PLA Silk $480 · PETG $500 · TPU $650 · Glow in the dark $600,
más la opción **Otro** para capturar el precio por kilo a mano.

Purga por multicolor: 1 color = 0 g · 2 colores = +3 g · 3 colores = +6 g · 4 colores = +10 g.

## Cómo calcula

```
costo placa = filamento + purga + energía + depreciación + extras + mano de obra
costo pieza = costo placa ÷ piezas por placa

filamento    = gramos ÷ 1000 × precio por kilo
purga        = gramos de purga ÷ 1000 × precio por kilo
energía      = horas × kW × precio del kWh
depreciación = horas × (precio de la máquina ÷ vida útil en horas)
extras       = (imanes + argollas + vela + empaque) × piezas
mano de obra = horas de pintura × $150 × piezas

precio sugerido = costo × (1 + margen)      ← margen sobre el costo (predeterminado)
precio sugerido = costo ÷ (1 − margen)      ← margen sobre el precio de venta
utilidad por pieza = precio − costo pieza
utilidad por hora de impresora = utilidad por pieza × piezas ÷ horas de impresión
```

**Utilidad por hora de impresora** es el número clave: mide lo que deja la máquina por cada
hora encendida y sale destacado con semáforo.

- 🔴 menos de $30/hora → mejor no gastes el filamento
- 🟡 entre $30 y $60/hora → aceptable, se puede mejorar
- 🟢 más de $60/hora → excelente, a imprimir

Los dos umbrales se editan en **Ajustes**.

La pintura a mano no descuenta horas de impresora: la utilidad por hora siempre se divide
entre el tiempo que la máquina estuvo trabajando.

## Catálogo

El botón **Guardar** archiva el producto con nombre en `localStorage`, junto con todas sus
entradas y ajustes. Desde la lista se puede **Cargar** (regresa todos los datos al formulario
para recalcular) o **Borrar**. La calculadora también recuerda los últimos valores capturados
al volver a abrirla.

Los datos viven **solo en ese navegador y ese dispositivo**: no se suben a ningún lado, y se
pierden si borras los datos del sitio.

## Nota sobre el margen

En **Ajustes** se elige cómo interpretar el porcentaje:

- **Sobre el costo** (predeterminado): 70% = precio 1.70× el costo.
- **Sobre la venta**: 70% = el margen es el 70% del precio, o sea 3.33× el costo.

Son dos números muy distintos con el mismo "70%". Vale la pena revisar cuál corresponde a
como llevas tus precios.
