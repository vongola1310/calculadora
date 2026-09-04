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
| Impresión (por placa) | horas y minutos, gramos de filamento, tipo de filamento, purga multicolor, piezas por placa, tamaño en cm |
| Extras (por pieza) | imanes $2, argollas $3, vela LED $25, pintura a mano $150/h, empaque $20 |
| Tu precio | por margen, precio manual, o precio de lista por tamaño |
| Ajustes | energía (kW × $/kWh), depreciación (precio de máquina ÷ vida útil), margen, precios por tamaño, umbrales del semáforo |

La app trae un manual desplegable (**📖 Qué hace cada campo**) con la explicación breve de
cada parámetro.

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

precio = costo × (1 + margen)      ← margen sobre el costo (predeterminado)
precio = costo ÷ (1 − margen)      ← margen sobre el precio de venta
precio = el que tú escribes        ← modo "Yo lo pongo"
precio = tu lista según tamaño     ← modo "Por tamaño"
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

## Los tres modos de precio

- **Por margen** — el precio sale del costo. Sirve para conocer tu piso, no tu precio de venta.
- **Yo lo pongo** — escribes el precio real al que vendes y la calculadora saca el margen y la
  utilidad por hora hacia atrás. **Es el modo para decidir si vale la pena imprimir algo.**
- **Por tamaño** — usa tu lista de precios según el alto de la pieza.

En modo *Por margen* la app compara el precio por costo contra tu lista por tamaño y te avisa
cuando te estás quedando corto.

## Tamaño

El alto en cm **no cambia el costo** (eso ya está en los gramos y las horas): clasifica la pieza
en Mini (<5 cm), Chica (5–10), Mediana (10–15), Grande (15–25) o XL (>25) para compararla contra
tu lista de precios de venta. Los precios de lista se editan en Ajustes.

## Catálogo

El botón **Guardar** archiva el producto con nombre en `localStorage`, junto con todas sus
entradas y ajustes. Desde la lista se puede **Cargar** (regresa todos los datos al formulario
para recalcular) o **Borrar**. La calculadora también recuerda los últimos valores capturados
al volver a abrirla.

Los datos viven **solo en ese navegador y ese dispositivo**: no se suben a ningún lado, y se
pierden si borras los datos del sitio.

## Por qué el precio por costo se queda corto

Con margen sobre el costo, la utilidad por hora se reduce a `margen × (costo de la placa ÷ horas)`.
Una impresión normal quema $8–15 de costo por hora, así que un 70% sobre el costo deja $6–10/hora
por más que le muevas a las constantes. La utilidad por hora sube en serio con **más piezas por
placa** y con **precio de mercado**, no bajando costos. Por eso existen los modos *Yo lo pongo* y
*Por tamaño*: la calculadora deja de proponerte el precio y pasa a calificar el tuyo.

## Nota sobre el margen

En **Ajustes** se elige cómo interpretar el porcentaje:

- **Sobre el costo** (predeterminado): 70% = precio 1.70× el costo.
- **Sobre la venta**: 70% = el margen es el 70% del precio, o sea 3.33× el costo.

Son dos números muy distintos con el mismo "70%". Vale la pena revisar cuál corresponde a
como llevas tus precios.
