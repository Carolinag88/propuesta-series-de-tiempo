# Propuesta de Serie de Tiempo: Pronóstico de la Demanda Comercial de Energía en Colombia

**Asignatura:** Series de Tiempo — Análisis de Datos  
**Autor:** Estudiante de Posgrado  
**Herramienta de Trabajo:** Python  

---

## 1. Descripción de la Serie de Tiempo
Para el desarrollo de los proyectos y análisis de la asignatura, se trabajará con la serie de tiempo de la **Demanda Comercial de Energía Eléctrica en Colombia**, administrada y centralizada por **XM S.A. E.S.P.** en su calidad de Operador del Mercado de Energía Mayorista (MEM).

* **Variable Principal:** Demanda comercial de energía eléctrica (medida en kWh / GWh).
* **Frecuencia Temporal:** Registro de resolución horaria con agregaciones diarias y mensuales.
* **Cobertura y Disponibilidad:** Datos históricos continuos con más de 10 años de registro hasta la fecha actual.

---

## 2. Justificación y Valor Agregado del Pronóstico
En la toma de decisiones dentro del sector energético, la imprecisión en las estimaciones del consumo genera sobrecostos financieros significativos y riesgos operativos en el suministro. Realizar un pronóstico riguroso sobre esta serie aporta un alto valor agregado en tres horizontes clave:

1. **Operativo (Corto Plazo - Horario/Diario):** Permite a comercializadores y generadores ofertar con precisión en la Bolsa de Energía, reduciendo desviaciones, penalizaciones y optimizando los costos de compra/venta.
2. **Táctico (Mediano Plazo - Mensual):** Facilita la programación de mantenimiento preventivo en plantas e infraestructura de transmisión sin poner en riesgo la atención de la demanda nacional.
3. **Estratégico (Largo Plazo - Anual):** Sirve como insumo crítico para planificar la capacidad de generación ante variaciones macroeconómicas y choques climáticos (fenómenos de El Niño o La Niña).

**Reto Técnico:** Esta serie representa un escenario analítico idóneo para la materia, ya que exhibe **estacionalidad múltiple** (intradiaria, semanal y anual), **tendencia** y sensibilidad a días festivos y variables climáticas.

---

## 3. Extracción de Datos mediante Herramientas TIC (Python)
Para garantizar la reproducibilidad y automatización del flujo de trabajo, los datos serán extraídos mediante la librería oficial de Python `pydataxm`, la cual interactúa directamente con la API REST de XM sin necesidad de descargas manuales de archivos.

El siguiente script en Python ilustra el procedimiento técnico para la ingesta de datos:

```python
import pandas as pd
from pydataxm import ReadDB

# Inicializar cliente de conexión a la API de XM
objeto_xm = ReadDB()

# Ingesta automatizada de la serie de tiempo de Demanda Comercial
df_demanda = objeto_xm.request_data(
    metric_id="DemandComercial",
    entity="Sistema",
    start_date="2023-01-01",
    end_date="2023-12-31"
)

# Estructurar la variable de tiempo como índice del DataFrame
df_demanda['Date'] = pd.to_datetime(df_demanda['Date'])
df_demanda.set_index('Date', inplace=True)

print(df_demanda.head())
