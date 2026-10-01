@'
# ESTADO DEL PROYECTO — Batería de Riesgo Psicosocial (Power BI)

Última actualización: 2026-10-01
Repo: https://github.com/ENTRENACONSULTINGSST-web/bateria-riesgo-psicosocial-template
Archivo Power BI: `C:\Users\chmed\pbix\bateria-riesgo-psicosocial-template.pbix`

---

## 1. Objetivo del proyecto

Plantilla `.pbix` reutilizable que lee un archivo `.xlsm` por cliente, genera textos narrativos y gráficas automáticamente, y se entrega como informe de diagnóstico de riesgo psicosocial (Batería del Ministerio de Trabajo, Resolución 2764 de 2022).

**Input único:** `.xlsm` con macros (`Evaluación lexmana sas (1).xlsm` en desarrollo).
**Output:** informe visual con 29 páginas + medidas DAX narrativas + alertas por nivel de riesgo.
**Modelo mental:** plantilla cliente-por-cliente, sin fila por persona en el `.pbix` entregable (k-anonimato).

---

## 2. Estructura del repositorio
bateria-riesgo-psicosocial-template/
├── README.md
├── data/
│ ├── baremos/Baremos_PowerBI.xlsx (13 hojas, ~24 KB)
│ └── diccionario/diccionario_campos.csv (154 filas, 8 columnas)
├── docs/
├── pdf_source/README_estructura_pdf.md
├── theme/tema_entrena.json (naranja #F26522 principal)
└── ESTADO_PROYECTO.md (este archivo)

**Commits relevantes:**
- `fdd9f84` — diccionario_campos.csv inicial (136 filas)
- `8a12303` — reemplazo de CSVs por `Baremos_PowerBI.xlsx`
- `369c22d` — tema con naranja `#F26522`
- `ed8778a` — ESTADO_PROYECTO.md + diccionario 154 filas (archivo quedó vacío por error; corregido)

**Pendiente:** verificar diccionario regenerado desde el `.xlsm` real (154 filas).

---

## 3. Pipeline ETL (Power Query)

### 3.1 Parámetros

| Parámetro | Tipo | Valor actual |
|---|---|---|
| `pArchivoFuente` | Texto | `C:\Users\chmed\Downloads\Evaluación lexmana sas (1).xlsm` |
| `pNombreEmpresa` | Texto | `Entrena Consulting SAS` (placeholder) |

### 3.2 Consultas cargadas al modelo (8 tablas finales)

| Consulta | Filas × Cols | Rol |
|---|---|---|
| `Baremos_Largo` | 315 × 9 | Catálogo de rangos por indicador |
| `Config` | 1 × 5 | Metadatos (empresa, año, período, psicólogo, licencia) |
| `Diccionario_Campos` | 154 × 8 | Traduce encabezados del `.xlsm` |
| `KPI_General` | 1 × 1 | Denominador global (`Total_Personas`) |
| `Resumen_Indicadores` | 239 × 8 | Agregado por indicador+nivel, sin ID |
| `Resumen_Sociodemografico` | variable × 3 | `Campo / Valor / Conteo` |
| `Umbrales_Alerta` | ~63 × 6 | Umbral mínimo de "Riesgo alto" por indicador |
| `_Medidas` | 1 × 1 | Contenedor vacío de medidas DAX |

### 3.3 Consultas intermedias (carga deshabilitada)

- `Hechos_Ancho` — 29 × 154 (hoja `T_Reporte` del `.xlsm`)
- `Hechos_Indicadores` — 999+ × 10 (unpivot + merge + pivot)
- `Ficha_Sociodemografica` — 29 × 25 (con `Nivel_Ocupacional` derivado)
- `src_Baremos_*` — 11 consultas fuente de los baremos
- `Totales combinados`
- `_Diag_Baremos` (diagnóstico, puede eliminarse)
- `pArchivoFuente`, `pNombreEmpresa` (parámetros)

### 3.4 Bloques M clave

**`Resumen_Indicadores`:**
```m
let
    Origen = Hechos_Indicadores,
    ConPromNum = Table.TransformColumns(Origen, {{"Promedio", each try Number.From(_) otherwise null, type nullable number}}),
    ConPuntNum = Table.TransformColumns(ConPromNum, {{"Puntaje", each try Number.From(_) otherwise null, type nullable number}}),
    Agrupado = Table.Group(
        ConPuntNum,
        {"Instrumento", "Forma", "Nivel_Agregacion", "Nombre", "Nivel_Riesgo"},
        {
            {"Conteo", each Table.RowCount(_), Int64.Type},
            {"SumaPuntaje", each List.Sum(List.RemoveNulls(_[Puntaje])), type nullable number},
            {"SumaPromedio", each List.Sum(List.RemoveNulls(_[Promedio])), type nullable number}
        }
    )
in
    Agrupado