# ESTADO DEL PROYECTO — Batería de Riesgo Psicosocial (Power BI)

Última actualización: 2026-10-01
Repo: https://github.com/ENTRENACONSULTINGSST-web/bateria-riesgo-psicosocial-template
Archivo Power BI: C:\Users\chmed\pbix\bateria-riesgo-psicosocial-template.pbix

---

## 1. Objetivo del proyecto

Plantilla .pbix reutilizable que lee un archivo .xlsm por cliente, genera textos narrativos y gráficas automáticamente, y se entrega como informe de diagnóstico de riesgo psicosocial (Batería del Ministerio de Trabajo, Resolución 2764 de 2022).

Input único: .xlsm con macros (Evaluación lexmana sas (1).xlsm en desarrollo).
Output: informe visual con 29 páginas + medidas DAX narrativas + alertas por nivel de riesgo.
Modelo mental: plantilla cliente-por-cliente, sin fila por persona en el .pbix entregable (k-anonimato).

---

## 2. Estructura del repositorio

- README.md
- data/baremos/Baremos_PowerBI.xlsx
- data/diccionario/diccionario_campos.csv (154 filas)
- docs/
- pdf_source/
- theme/tema_entrena.json (naranja #F26522)
- ESTADO_PROYECTO.md

Commits relevantes: fdd9f84, 8a12303, 369c22d, ed8778a

---

## 3. Pipeline ETL (Power Query)

### Parámetros
- pArchivoFuente = C:\Users\chmed\Downloads\Evaluación lexmana sas (1).xlsm
- pNombreEmpresa = Entrena Consulting SAS

### 8 tablas finales cargadas
Baremos_Largo, Config, Diccionario_Campos, KPI_General, Resumen_Indicadores, Resumen_Sociodemografico, Umbrales_Alerta, _Medidas

### Consultas intermedias (carga deshabilitada)
Hechos_Ancho, Hechos_Indicadores, Ficha_Sociodemografica, src_Baremos_*, Totales combinados, _Diag_Baremos

### .xlsm real
Hoja T_Reporte: 30 filas × 156 columnas (29 personas).
Sin datos de Patrones de conducta, Rasgos de personalidad ni Gestión del sueño-fatiga.

---

## 4. Modelo de datos
Ninguna relación activa. Todas las tablas son independientes.

---

## 5. Medidas DAX principales
- Total Encuestados
- Promedio Puntaje
- % Riesgo Alto y Muy Alto
- Nivel Riesgo Predominante
- Alerta_Texto
- Textos narrativos: Texto_Sociodemografico, Texto_Analisis_Sociodemografico, Texto_Dimensiones_Peor, Texto_Ficha_Tecnica, Texto_Portada_Subtitulo

---

## 6. Estado de las 29 páginas
1 Inicio — Terminada
2 Ficha Técnica — Estructura básica
5 Ficha sociodemográfica — En construcción
6 Análisis sociodemográfico — Terminada
20-24 Patrones/Rasgos/Sueño — Sin fuente en T_Reporte
Resto — Pendiente

Páginas actuales en el .pbix: 7

---

## 7. Diseño
Color principal: #F26522
Fondo: #E8EEF2
Tema: theme/tema_entrena.json

---

## 8. Pendientes críticos
- Migrar orígenes a URLs raw de GitHub
- k-anonimato (Base_Suficiente >= 5)
- Confirmar fuente de Patrones / Rasgos / Sueño-fatiga
- Catálogos de definiciones y recomendaciones
- Exportar .pbit sin datos

---

## 9. Entregable final
.pbit (plantilla) + .pbix con datos agregados + PDF

---

## 10. Cómo reanudar
1. Abrir este archivo y el .pbix
2. Verificar _Medidas sin error
3. Continuar por página 2 (Ficha Técnica) o 5 (Ficha sociodemográfica)

---

## 11. Secuencia para cargar un cliente nuevo (.xlsm)

### Principio
Cada cliente tiene su propio archivo Excel con macros (`.xlsm`), nombrado con el **nombre de la empresa**.
No se usa un nombre genérico fijo tipo `MAIN.xlsm`.

- Extensión correcta: **`.xlsm`**
- Ejemplo: `Lexmana SAS.xlsm`, `Entrena Consulting SAS.xlsm`

### Estructura de carpetas recomendada

```text
C:\Users\chmed\Documents\BateriaRiesgoPsicosocial\
├── pbix\
│   └── bateria-riesgo-psicosocial-template.pbix
└── fuente\
    ├── Lexmana SAS.xlsm
    ├── Otra Empresa SA.xlsm
    └── ...