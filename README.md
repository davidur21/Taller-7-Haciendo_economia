# Taller 7 — Índices sectoriales antes y después del COVID (2015–2025)

**Curso:** Haciendo Economía · Universidad del Rosario · Prof. Paul Rodríguez Lesmes
**Cliente (simulado):** Fondo de inversión
**Base metodológica:** CORE Econ, *Doing Economics*, sección 10.2 — [books.core-econ.org/doing-economics/book/text/10-02.html](https://books.core-econ.org/doing-economics/book/text/10-02.html)
**Herramienta obligatoria:** Microsoft Excel (fórmulas visibles) · Fuente de datos: Bloomberg (conexión Excel–Bloomberg) · Visualización complementaria opcional: Power BI

---

## 1. El encargo

El fondo quiere comparar la evolución de **tres sectores de la economía global entre 2015 y 2025**, con énfasis en las diferencias entre un periodo previo al COVID (2015) y uno posterior (2025). Para ello, el equipo consultor construye índices sectoriales transparentes a partir de precios y volúmenes de mercado, evalúa cómo la regla de ponderación cambia la lectura de los resultados, compara retornos y volatilidad, y entrega una **recomendación de inversión** sustentada en la evidencia.

El fondo necesita respuesta a cuatro preguntas:

| # | Pregunta del fondo | Dónde se responde |
|---|---|---|
| i | ¿Cómo se construyeron los tres índices y qué representan? | Secciones 1 y 2.1 del taller |
| ii | ¿Qué muestran sobre desempeño y riesgo? | Secciones 2.2–2.5 |
| iii | ¿Qué cambió entre 2015 y 2025 y qué limita esa comparación? | Secciones 3.1–3.4 |
| iv | ¿Qué sectores priorizar, mantener en observación o evitar? | Sección 3.5 |

---

## 2. Equipo consultor

**Integrantes:** Derek Santiago Gaona · Emanuel Fernando Hernández León · Santiago Martinez Reinoso · David Pascagaza Rodriguez

| Rol | Integrante | Responsabilidad principal |
|---|---|---|
| 1. Líder del proyecto y enlace con el fondo | `[Por asignar]` | Coordinación, alineación con las preguntas del fondo e integración de la recomendación de inversión |
| 2. Especialista en datos y reproducibilidad | `[Por asignar]` | Descarga Excel–Bloomberg, documentación de fuentes, estructura del libro y trazabilidad de fórmulas |
| 3. Analista cuantitativo | `[Por asignar]` | Pesos, índices, retornos, desviaciones estándar, n e intervalos de confianza |
| 4. Especialista en visualización y comunicación | `[Por asignar]` | Gráficos, tablas, narrativa visual y (opcional) tablero en Power BI |

> **Responsabilidad compartida.** Los roles definen una responsabilidad principal, no dividen el taller. El portavoz de la presentación se elige al azar justo antes de exponer, por lo que los cuatro integrantes deben poder explicar y defender cualquier parte del análisis.

Los aportes individuales (2 a 4 por integrante, con la hoja, rango, tabla o gráfico donde se verifican) se registran en la hoja `00_Equipo` del libro y se resumen en la sección 9 de este README.

---

## 3. Evaluación

| Componente | Puntos | Tipo de nota |
|---|---|---|
| Presentación al cliente (máx. 5 min) | 50 | Grupal |
| Archivo de Excel | 35 | Grupal |
| Aportes asociados al rol | 15 | Individual |
| **Total** | **100** | |

---

## 4. Estructura del repositorio

```
Taller7_Indices_Sectoriales/
│
├── README.md                      ← este archivo
│
├── Excel/
│   ├── README.md                  ← mapa de hojas y convenciones de fórmulas
│   └── Taller7_Indices_Sectoriales.xlsx
│
├── RawData/
│   ├── README.md                  ← tickers, campos Bloomberg, fechas y fecha de descarga
│   └── (exportaciones Bloomberg de respaldo en .csv / .xlsx)
│
├── PowerBI/                       ← opcional
│   ├── README.md
│   └── Taller7_Dashboard.pbix
│
├── Presentacion/
│   ├── README.md
│   └── Taller7_Informe_Fondo.pptx
│
└── Docs/
    └── Taller_7_Consultoria_Fondo_Inversion.docx   ← enunciado original
```

> **¿Por qué `RawData/` si los datos viven en Excel?** Las fórmulas BDH solo se recalculan en un terminal con Bloomberg. Guardar una copia estática de la descarga permite abrir y verificar el libro en cualquier computador sin perder los datos.

---

## 5. Mapa del libro de Excel

El libro sigue el orden del enunciado: cada hoja alimenta a la siguiente por referencia, sin valores pegados.

| Hoja | Contenido | Punto del taller |
|---|---|---|
| `00_Equipo` | Integrantes, roles y registro de aportes verificables | Organización |
| `01_Universo` | 3 sectores × 10 acciones: ticker Bloomberg, empresa, industria, país, fuente | 1.1 · 2.1 |
| `02_Precios` | Precio de cierre diario (`PX_LAST`), 2015–2025 | 1.1 |
| `03_Volumen` | Volumen diario (`PX_VOLUME`), 2015–2025 | 1.1 |
| `04_Pesos` | Pesos por volumen y por precio inicial (enero 2015), control de suma = 1 y comparación | 1.2 · 1.3 |
| `05_Retornos` | Retornos aritméticos diarios por acción | 2.2 |
| `06_Indices` | Retornos ponderados diarios de cada índice y niveles base 100 | 2.2 · 2.5 |
| `07_Distribuciones` | Cajas y bigotes e histogramas de los retornos ponderados | 2.3 · 2.4 |
| `08_Comparacion_2015_2025` | Promedio, desviación estándar, n e IC 95 % por índice y año; gráficos de barras con IC | 3.1 · 3.2 · 3.3 |
| `09_Interpretacion` | Lectura de resultados, cautelas metodológicas y recomendación de inversión | 2.1 · 2.5 · 3.4 · 3.5 |

---

## 6. Metodología

### 6.1 Datos (1.1)
- **Periodo:** 1 de enero de 2015 – 31 de diciembre de 2025, frecuencia diaria.
- **Universo:** 3 sectores × 10 acciones = 30 series de precio y 30 de volumen.
- **Descarga con Bloomberg Excel Add-in**, por ejemplo:

  ```
  =BDH("XOM US Equity"; "PX_LAST";   "01/01/2015"; "31/12/2025")
  =BDH("XOM US Equity"; "PX_VOLUME"; "01/01/2015"; "31/12/2025")
  ```
  *(el separador `;` o `,` depende de la configuración regional del Excel)*

- **Sectores seleccionados:** `[Sector A]`, `[Sector B]`, `[Sector C]` — el detalle de tickers se documenta en `01_Universo` y en `RawData/README.md`.

### 6.2 Pesos (1.2 · 1.3)
Para cada acción *i* dentro del índice *k*, con valores de **enero de 2015** (*t = 0*):

| Regla | Fórmula | Lectura |
|---|---|---|
| Por volumen | w<sub>i</sub> = V<sub>i,0</sub> / Σ<sub>j</sub> V<sub>j,0</sub> | Pesa más la acción más transada (liquidez) |
| Por precio | w<sub>i</sub> = P<sub>i,0</sub> / Σ<sub>j</sub> P<sub>j,0</sub> | Pesa más la acción de mayor precio nominal (lógica tipo Dow Jones) |

Control obligatorio en `04_Pesos`: `=SUMA(rango_pesos)` = 1 para cada índice y cada regla.

> **Decisión del equipo a documentar:** qué se entiende por "enero de 2015" — primer día hábil del mes o promedio de enero. El promedio suaviza días atípicos; el primer día es más simple de trazar.

### 6.3 Retornos e índices (2.2 · 2.5)
- **Retorno aritmético diario por acción:** r<sub>i,t</sub> = (P<sub>i,t</sub> − P<sub>i,t−1</sub>) / P<sub>i,t−1</sub>
- **Retorno ponderado del índice:** r<sub>k,t</sub> = Σ<sub>i</sub> w<sub>i</sub> · r<sub>i,t</sub> → en Excel, `=SUMAPRODUCTO(pesos; retornos_fila)`
- **Nivel del índice, base 100 en enero de 2015:** I<sub>k,0</sub> = 100; I<sub>k,t</sub> = I<sub>k,t−1</sub> · (1 + r<sub>k,t</sub>)

### 6.4 Comparación 2015 vs. 2025 (3.1 · 3.2)
Para cada índice y cada año, sobre los retornos ponderados diarios:

| Estadístico | Excel (español) | Excel (inglés) |
|---|---|---|
| Promedio | `PROMEDIO` | `AVERAGE` |
| Desviación estándar muestral | `DESVEST.M` | `STDEV.S` |
| Número de observaciones | `CONTAR` | `COUNT` |
| Margen del IC 95 % | `INTERVALO.CONFIANZA.T(0,05; s; n)` | `CONFIDENCE.T(0.05, s, n)` |
| Límite inferior / superior | promedio − margen / promedio + margen | |

---

## 7. Reglas de reproducibilidad

1. **Cero valores hardcodeados** cuando el resultado pueda obtenerse con fórmula o referencia.
2. **Fórmulas visibles:** nada de "pegar como valores" en hojas de cálculo; la única excepción es la copia de respaldo en `RawData/`.
3. **Rangos con nombre** (p. ej. `Pesos_Vol_A`, `Ret_Indice_A_2015`) para que las fórmulas se lean solas.
4. **Celdas de control** visibles: suma de pesos = 1, conteo de observaciones por año, fechas faltantes.
5. **Actualización:** cambiar las fechas o los tickers en `01_Universo` debe propagarse a todo el libro sin rehacer cálculos a mano.
6. **Fechas alineadas:** las 10 acciones de un índice comparten el mismo calendario; los días sin cotización se tratan de forma explícita y documentada.

---

## 8. Cautelas metodológicas

- **Tamaño del universo:** 10 acciones no representan un sector completo; los resultados describen *este* índice, no el sector global.
- **Sesgo de supervivencia:** elegir hoy empresas que existían en 2015 excluye a las que quebraron o salieron de bolsa en el periodo.
- **Pesos fijos:** ponderar con datos de enero de 2015 ignora cómo cambió la importancia relativa de cada empresa en diez años.
- **Ponderación por precio:** el precio nominal no mide el tamaño de la empresa; un *split* altera el peso implícito.
- **Retornos aritméticos:** no incluyen dividendos (salvo que se use un campo de retorno total).
- **Comparación 2015 vs. 2025:** son dos años puntuales; una diferencia entre ellos **no identifica el efecto causal del COVID**. Tasas de interés, inflación, precios de materias primas y choques sectoriales ocurren en el mismo intervalo.

---

## 9. Estado del proyecto y aportes

| Punto | Descripción | Estado |
|---|---|---|
| 1.1 | Selección de sectores, universo y descarga de datos | ⬜ Pendiente |
| 1.2 | Pesos por volumen | ⬜ Pendiente |
| 1.3 | Pesos por precio y comparación | ⬜ Pendiente |
| 2.1 | Descripción de los índices y limitaciones | ⬜ Pendiente |
| 2.2 | Retornos diarios y ponderados | ⬜ Pendiente |
| 2.3 | Cajas y bigotes | ⬜ Pendiente |
| 2.4 | Histogramas | ⬜ Pendiente |
| 2.5 | Índices base 100 | ⬜ Pendiente |
| 3.1 | Desviación estándar y n (2015 vs. 2025) | ⬜ Pendiente |
| 3.2 | IC 95 % con `CONFIDENCE.T` | ⬜ Pendiente |
| 3.3 | Barras con IC | ⬜ Pendiente |
| 3.4 | Interpretación | ⬜ Pendiente |
| 3.5 | Recomendación de inversión | ⬜ Pendiente |

**Aportes individuales verificables** (se completan cuando cada integrante los confirme):

| Integrante | Rol | Aporte | Dónde verificarlo |
|---|---|---|---|
| Derek Santiago Gaona | | | |
| Emanuel Fernando Hernández León | | | |
| Santiago Martinez Reinoso | | | |
| David Pascagaza Rodriguez | | | |

---

## 10. Cronograma

| Sesión | Actividad |
|---|---|
| 1 | Entrenamiento en Bloomberg (club de inversiones) y socialización del encargo |
| 2 | Conformación del equipo, selección de sectores y acciones, conexión Excel–Bloomberg y construcción del libro |
| 3 | Presentación al fondo: informe ejecutivo de máximo 5 minutos, orden y portavoz aleatorios |

---

## Referencias

- CORE Econ. *Doing Economics*, Empirical Project 10, sección 10.2. [books.core-econ.org/doing-economics/book/text/10-02.html](https://books.core-econ.org/doing-economics/book/text/10-02.html)
- Bloomberg L.P. *Bloomberg Excel Add-in — BDH (Bloomberg Data History)*.
- Microsoft. *Función INTERVALO.CONFIANZA.T / CONFIDENCE.T*.
