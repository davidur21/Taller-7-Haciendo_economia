# Taller 7 — Índices sectoriales antes y después del COVID (2015–2025)

**Curso:** Haciendo Economía · Universidad del Rosario · Prof. Paul Rodríguez Lesmes
**Cliente (simulado):** Fondo de inversión
**Base metodológica:** CORE Econ, *Doing Economics*, sección 10.2 — [books.core-econ.org/doing-economics/book/text/10-02.html](https://books.core-econ.org/doing-economics/book/text/10-02.html)
**Herramientas:** Microsoft Excel (fórmulas visibles) · Datos: Bloomberg Terminal vía BQL

---

## 1. El encargo

El fondo quiere comparar la evolución de **tres sectores entre 2015 y 2025**, con énfasis en las diferencias entre un año previo al COVID (2015) y uno posterior (2025). El equipo construyó índices sectoriales transparentes a partir de precios y volúmenes de mercado, evaluó cómo la regla de ponderación cambia la lectura de los resultados, comparó retornos y volatilidad, y formuló una **recomendación de inversión** sustentada en la evidencia.

| # | Pregunta del fondo | Dónde se responde |
|---|---|---|
| i | ¿Cómo se construyeron los tres índices y qué representan? | Puntos 1.1–1.3 y 2.1 |
| ii | ¿Qué muestran sobre desempeño y riesgo? | Puntos 2.2–2.5 |
| iii | ¿Qué cambió entre 2015 y 2025 y qué limita esa comparación? | Puntos 3.1–3.4 |
| iv | ¿Qué sectores priorizar, mantener en observación o evitar? | Punto 3.5 |

---

## 2. Entregables

| Entregable | Archivo / enlace | Peso en la nota |
|---|---|---|
| Libro de Excel con todos los cálculos formulados | `TALLER7-DOINGECON.xlsx` | 35 pts (grupal) |
| Presentación al cliente (máx. 5 min) | Canva: `[pegar enlace]` · respaldo en PDF: `Taller7_Informe_Fondo.pdf` | 50 pts (grupal) |
| Aportes individuales por rol | Hoja `PORTADA`, sección 2, del libro de Excel | 15 pts (individual) |

---

## 3. Equipo consultor

| Rol | Integrante | Responsabilidad principal |
|---|---|---|
| 1. Líder del proyecto y enlace con el fondo | David Pascagaza Rodriguez | Coordinación, alineación con las preguntas del fondo e integración de la recomendación de inversión |
| 2. Especialista en datos y reproducibilidad | Derek Santiago Gaona | Descarga Excel–Bloomberg, documentación de fuentes, estructura del libro y trazabilidad de fórmulas |
| 3. Analista cuantitativo | Santiago Martinez Reinoso | Pesos, índices, retornos, desviaciones estándar, n e intervalos de confianza |
| 4. Especialista en visualización y comunicación | Emanuel Fernando Hernández León | Gráficos, tablas y narrativa visual de la presentación |

> **Responsabilidad compartida.** Los roles definen una responsabilidad principal, no dividen el taller. El portavoz se elige al azar justo antes de exponer, así que los cuatro integrantes pueden explicar y defender cualquier parte del análisis.

---

## 4. Estructura de la entrega

```
Taller7_Indices_Sectoriales/
├── README.md                                  ← este archivo
├── TALLER7-DOINGECON.xlsx                     ← libro con datos, cálculos, gráficos y conclusiones
├── Taller7_Informe_Fondo.pdf                  ← respaldo de la presentación (el original está en Canva)
└── Taller_7_Consultoria_Fondo_Inversion.docx  ← enunciado original
```

> **El libro abre sin terminal Bloomberg.** Las hojas `RETAIL`, `HIDROCARBUROS` y `TECNOLOGY` guardan las consultas BQL originales. Las hojas con sufijo `1` son copias estáticas de esos datos, y todas las fórmulas del libro están conectadas a esas copias.

---

## 5. Mapa del libro de Excel

| Hoja | Contenido | Puntos del taller |
|---|---|---|
| `PORTADA` | Equipo y roles, aportes individuales verificables, documentación de datos, mapa del libro y lista de cumplimiento | Organización · 1.1 |
| `RETAIL` · `HIDROCARBUROS` · `TECNOLOGY` | Consultas BQL originales (`px_last`, `px_volume`). Requieren terminal y ya no alimentan cálculos | 1.1 |
| `RETAIL1` · `HIDROCARBUROS1` · `TECNOLOGY1` | Copia estática de fechas, precios y volúmenes. Base de todas las fórmulas | 1.1 |
| `INDICES RETAIL` · `INDICES HIDROCARBUROS` · `INDICES TECNOLOGY` | Pesos, retornos diarios, retornos ponderados, índices base 100, estadísticas, IC 95 %, tablas de histograma y gráficos | 1.2 · 1.3 · 2.2–2.5 · 3.1–3.3 |
| `COMPARATIVO` | Métricas clave de los tres índices y gráfico de líneas base 100 conjunto | 2.5 · 3.4 · 3.5 |
| `CONCLUSIONES` | Tabla de evidencia ligada por fórmula, respuestas escritas, recomendación de inversión y cautelas | 1.3 · 2.1 · 2.3–2.5 · 3.4 · 3.5 |

---

## 6. Datos y metodología

### 6.1 Universo y datos (1.1)

| Índice | Acciones (tickers Bloomberg `US Equity`) |
|---|---|
| Retail | AMZN, WMT, COST, TGT, HD, LOW, BBY, ULTA, DG, DLTR |
| Hidrocarburos | XOM, CVX, BP, SHEL, E, HAL, PBR, EC, OXY, COP |
| Tecnología | AAPL, MSFT, NVDA, GOOGL, META, ORCL, AMD, INTC, QCOM, CSCO |

- **Fuente:** Bloomberg (BQL), campos `px_last` (precio de cierre) y `px_volume` (volumen), diarios.
- **Periodo:** 2-ene-2015 a 31-dic-2025. Son 2.766 días por acción y 2.765 retornos diarios por índice, con las fechas alineadas en las 10 acciones de cada sector.

### 6.2 Pesos (1.2 · 1.3)
Los pesos son fijos y se calculan con los datos del **primer día hábil, 2-ene-2015**:

| Regla | Fórmula | Lectura |
|---|---|---|
| Por volumen | peso = volumen inicial de la acción ÷ volumen inicial total del índice | Pesa más la acción más transada (liquidez) |
| Por precio | peso = precio inicial de la acción ÷ suma de precios iniciales | Pesa más la acción de mayor precio nominal |

Cada juego de pesos tiene una celda de control que verifica que sume 1 (`INDICES *!D17:E17`).

### 6.3 Retornos e índices (2.2 · 2.5)
- **Retorno diario por acción:** P(t) ÷ P(t−1) − 1
- **Retorno ponderado del índice:** Σ peso × retorno
- **Índice base 100:** I(0) = 100; I(t) = I(t−1) × (1 + retorno ponderado)

### 6.4 Comparación 2015 vs. 2025 (3.1 · 3.2)
Para cada índice y cada año, sobre los retornos ponderados: `COUNTIFS` (n), `AVERAGEIFS` (promedio), `STDEV.S` (desviación estándar) y `CONFIDENCE.T(0,05; s; n)` (margen del IC 95 %). Límites del intervalo = promedio ± margen.

---

## 7. Resultados principales (ponderación por volumen)

| Métrica | Tecnología | Retail | Hidrocarburos |
|---|---|---|---|
| Índice final (base 100) | 2.623 | 968 | 179 |
| Rendimiento acumulado (volumen / precio) | 2.523 % / 554 % | 868 % / 331 % | 79 % / 25 % |
| Desviación estándar diaria | 1,75 % | 1,52 % | 2,33 % |
| Curtosis (exceso) | 7,2 | 5,7 | 14,6 |
| Retorno / riesgo | 0,076 | 0,062 | 0,021 |
| ¿Se traslapan los IC de 2015 y 2025? | Sí | Sí | Sí |
| **Recomendación** | **Priorizar** | **Mantener bajo observación** | **Evitar por ahora** |

- **La regla de ponderación importa.** Por volumen, las dos acciones más transadas suman 76 % de Retail, 69 % de Tecnología y 59 % de Hidrocarburos.
- **2015 vs. 2025.** No hay evidencia de un cambio en el retorno promedio diario en ningún sector. Lo que sí cambió es la dispersión: −39 % en Hidrocarburos y +35 % en Tecnología.
- **Ciclos.** Las variaciones anuales de Retail y Tecnología se mueven juntas (correlación 0,65), mientras Hidrocarburos va a contraciclo (−0,59 con Retail).

El detalle y la redacción completa están en la hoja `CONCLUSIONES`.

---

## 8. Cautelas metodológicas

1. **Muestra:** 10 acciones por sector que siguen listadas hoy (sesgo de supervivencia) y con el peso concentrado en pocas acciones.
2. **Construcción:** los pesos fijos de enero de 2015 equivalen a rebalancear a diario, sin costos de transacción ni impuestos. Los retornos son de precio, sin dividendos.
3. **Inferencia:** `CONFIDENCE.T` supone normalidad aproximada, pero los retornos tienen colas pesadas. Los intervalos son una guía, no una prueba definitiva.
4. **Causalidad:** comparar 2015 y 2025 es descriptivo y no permite atribuir las diferencias al COVID.
5. **Desempeño pasado:** no garantiza rendimientos futuros. Este es un análisis académico, no asesoría financiera.

---

## 9. Cumplimiento y aportes individuales

Todos los puntos del taller (1.1 a 3.5) están cumplidos. La hoja `PORTADA` (sección 5) indica dónde se verifica cada uno.

| Integrante | Rol | Aportes concretos | Dónde verificarlo |
|---|---|---|---|
| David Pascagaza Rodriguez | Líder del proyecto | Resumen comparativo de métricas; verificación de cobertura de los puntos 1.1–3.5; interpretación 2015 vs. 2025 y recomendación con cautelas | `COMPARATIVO!A5:D20`; `PORTADA!A43:D56`; `CONCLUSIONES` secciones I, J y K |
| Derek Santiago Gaona | Datos y reproducibilidad | Descarga BQL de 30 acciones; copias estáticas y reconexión de fórmulas; documentación de fuentes y estructura del libro | Hojas `RETAIL`, `HIDROCARBUROS`, `TECNOLOGY` y sufijo `1`; `PORTADA` secciones 3 y 4 |
| Santiago Martinez Reinoso | Analista cuantitativo | Pesos por volumen y precio; retornos e índices base 100; n, promedio, desviación e IC 95 %; estadísticas descriptivas | `INDICES *!A5:F17`, `A34:P2800`, `I5:M13`, `I15:K30`; `CONCLUSIONES` sección A |
| Emanuel Fernando Hernández León | Visualización y comunicación | Gráficos de línea base 100; histogramas y cajas y bigotes; barras 2015 vs. 2025 con IC | Gráficos `Linea_*`, `Histograma_*`, `Caja_*` e `IC_*` en `INDICES *` y `COMPARATIVO` |

---

## Referencias

- CORE Econ. *Doing Economics*, Empirical Project 10, sección 10.2. [books.core-econ.org/doing-economics/book/text/10-02.html](https://books.core-econ.org/doing-economics/book/text/10-02.html)
- Bloomberg L.P. *Bloomberg Query Language (BQL)*: campos `px_last` y `px_volume`.
- Microsoft. *Función CONFIDENCE.T (INTERVALO.CONFIANZA.T)*.
