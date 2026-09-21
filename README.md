<img width="1345" height="758" alt="Captura de pantalla 2026-09-20 225539" src="https://github.com/user-attachments/assets/1542420d-9a19-434e-8bf4-191140996bd9" />
<img width="1346" height="754" alt="Captura de pantalla 2026-09-20 225815" src="https://github.com/user-attachments/assets/bdd287ab-c119-45cd-b1ab-e3fae2f78da8" />
<img width="1342" height="757" alt="Captura de pantalla 2026-09-20 225843" src="https://github.com/user-attachments/assets/a39bb5b9-6f00-4f98-a201-c261aba5bab8" />

# Financial Dashboard — Power BI Developer Project (.pbip)

Dashboard financiero corporativo y modelo analítico desarrollado en **Power BI Desktop** utilizando el formato moderno **Power BI Project (`.pbip`)** con **TMDL** (Tabular Model Definition Language) y **PBIR** (Power BI Report JSON), diseñado para control de versiones y colaboración en Git.

---

## Vistas del Reporte (Dashboards)

El reporte está diseñado en resolución **1920 × 1080 (Full HD)** con el tema corporativo **Fluent2** y comprende 3 páginas ejecutivas:

### 1. Resumen Ejecutivo (`resumen-ejecutivo-page`)
* **KPIs Clave:** Ingresos Totales, Gastos Totales, Utilidad Neta, Margen Neto %, Crecimiento Anual % y acumulados del año (Ingresos hasta la fecha, Gastos hasta la fecha, Utilidad hasta la fecha).
* **Comparativo Ingresos vs. Gastos:** Gráfico de columnas agrupadas con evolución mensual.
* **Tendencia de Utilidad:** Análisis temporal de la rentabilidad y comportamiento del margen.
* **Mix de Ingresos:** Distribución porcentual por línea de negocio.
* **Filtros:** Segmentación interactiva por Año fiscal.

### 2. Análisis de Ingresos (`analisis-ingresos-page`)
* **KPIs:** Facturación Total, Variación Interanual (% Anual) y Ticket Promedio por venta.
* **Evolución y Estacionalidad:** Histórico de facturación para detectar patrones comerciales.
* **Ranking por Categoría:** Desglose de ingresos por servicio (Software, Licencias, Consultoría, Soporte, Capacitación).
* **Desempeño Regional:** Participación y comparativo vs. año anterior por territorio (Norte, Centro, Sur).
* **Matriz Detallada:** Tabla analítica por producto/servicio con métricas clave.
* **Filtros:** Slicers dinámicos por Año, Trimestre y Región con botón de restablecimiento.

### 3. Control de Gastos y Presupuesto (`control-gastos-page`)
* **KPIs:** Gasto Real Total, Presupuesto Aprobado, Desviación Presupuestal ($) y % de Ejecución Presupuestal.
* **Real vs. Presupuesto:** Comparativa visual por rubro (Nómina, Infraestructura, Marketing, Administración, Investigación).
* **Semáforo de Desviaciones:** Monitoreo visual de ahorros y sobrecostos.
* **Estructura de Costos:** Composición porcentual del gasto operativo.
* **Tabla de Ejecución:** Matriz de seguimiento presupuestal con importes y variaciones.
* **Filtros:** Selector anual y reseteo rápido.

---

## Arquitectura del Modelo de Datos (Star Schema)

El modelo semántico implementa un **esquema estrella** con relaciones de uno a varios (1:N):

```
          ┌─────────────┐
          │  DimFecha   │
          └──────┬──────┘
                 │
       ┌─────────┴─────────┐
       │ (1:N)             │ (1:N)
       ▼                   ▼
┌──────────────┐    ┌──────────────┐
│  FactVentas  │    │  FactGastos  │
└──────────────┘    └──────────────┘
       ▲ (N:1)             ▲ (N:1)
       │                   │
       └─────────┬─────────┘
                 │
          ┌──────┴──────┐
          │DimCategoria │
          └─────────────┘
```

### Tablas del Modelo:
* **`FactVentas`**: Transacciones de ventas e ingresos por fecha, categoría, unidades y región (2023–2025).
* **`FactGastos`**: Gastos mensuales ejecutados vs. Presupuesto aprobado por categoría (2023–2025).
* **`DimCategoria`**: Clasificación de líneas de ingreso (*Software, Consultoría, Licencias, Soporte, Capacitación*) y centros de costo (*Nómina, Infraestructura, Marketing, Administración, Investigación*).
* **`DimFecha`**: Calendario dinámico (2023 a 2025) con jerarquías de Año, Mes, Nombre de Mes, Trimestre y AñoMes.
* **`Medidas`**: Tabla desacoplada con más de 15 medidas DAX organizadas.

---

## Fórmulas DAX Destacadas

* **Utilidad Neta:**
  ```dax
  Utilidad Neta = [Ingresos Totales] - [Gastos Totales]
  ```
* **Margen Neto %:**
  ```dax
  Margen Neto % = DIVIDE([Utilidad Neta], [Ingresos Totales], 0)
  ```
* **Crecimiento YoY % (Time Intelligence):**
  ```dax
  Ingresos Año Anterior = 
      CALCULATE([Ingresos Totales], SAMEPERIODLASTYEAR(DimFecha[Fecha]))

  Crecimiento % = 
      DIVIDE([Ingresos Totales] - [Ingresos Año Anterior], [Ingresos Año Anterior], BLANK())
  ```
* **Ejecución Presupuestal:**
  ```dax
  % Ejecución Presupuesto = DIVIDE([Gastos Totales], [Presupuesto Total], 0)
  ```

---

## Estructura del Repositorio

```
.
├── FinancialDashboard.pbip             # Archivo raíz del proyecto Power BI Desktop
├── FinancialDashboard.Report/          # Definición visual del reporte (PBIR JSON)
│   ├── definition/
│   │   ├── pages/                      # Páginas y contenedores visuales
│   │   └── report.json
│   └── StaticResources/                # Tema Fluent2 corporativo
├── FinancialDashboard.SemanticModel/   # Modelo de datos semántico (TMDL)
│   └── definition/
│       ├── model.tmdl                  # Configuración del modelo y cultura (es-MX)
│       ├── relationships.tmdl          # Relaciones del modelo estrella
│       └── tables/                     # Definición de tablas y medidas DAX
├── .gitignore                          # Exclusiones de Git para Power BI DevMode
└── README.md
```

---

## Como abrir y usar el proyecto?

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/ManuelFernandezD/FinancialDashboard.git
   ```
2. Asegurarse de tener instalado **Power BI Desktop** (versión compatible con *Developer Mode / PBIP*).
3. Hacer doble clic en el archivo **`FinancialDashboard.pbip`**.
4. Power BI Desktop cargará automáticamente el modelo semántico y el reporte con todas sus visualizaciones interactivas.
