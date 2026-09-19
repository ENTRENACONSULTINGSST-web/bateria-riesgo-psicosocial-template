# Estructura esperada del PDF

Este documento describe el layout del PDF que se usará como **único input** del
informe en Power BI. No contiene datos reales; es una referencia de estructura.

## Origen

PDF único por cliente, con todos los componentes del instrumento:

- Ficha sociodemográfica (24 campos).
- Cuestionario intralaboral, Forma A o Forma B (dimensiones + dominios + total).
- Cuestionario extralaboral (dimensiones + total).
- Evaluación de estrés (puntaje total).
- Estrategias de afrontamiento (si aplica).
- Notas metodológicas y trazabilidad.

## Estructura ancha esperada

El PDF replica la hoja reporte del archivo de baremos:

- 1 fila por persona evaluada.
- 136 columnas:
  - 24 columnas sociodemográficas.
  - 112 columnas del instrumento (56 pares puntaje/nivel de riesgo).

## Encabezados del instrumento

Los nombres de las 112 columnas del instrumento coinciden exactamente con la
columna Encabezado_Original de data/diccionario/diccionario_campos.csv.

Ejemplos:

- Dimensión: Características del liderazgo - Forma A (puntaje transformado)
- DOMINIO: Liderazgo y relaciones sociales en el trabajo - Forma A (nivel de riesgo)
- PUNTAJE TOTAL del cuestionario de factores de riesgo psicosocial intralaboral - Forma A (puntaje transformado)

## Reglas

- La identidad de cada dimensión/dominio/total depende del texto del encabezado.
- Forma (A/B) y Nivel_Ocupacional (Jefes/Auxiliares) se derivan del parseo.
- La codificación debe ser UTF-8.
- No se almacenan PDFs reales en este repositorio.
