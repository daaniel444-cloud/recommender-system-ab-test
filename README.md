# Prueba A/B — Sistema de Recomendaciones (`recommender_system_test`)

Proyecto final de análisis de datos — TripleTen (Analista de Datos)

## Objetivo

Evaluar los resultados de una prueba A/B lanzada por una tienda en línea internacional, diseñada para probar un nuevo embudo de pago con sistema de recomendaciones mejorado (Grupo B) frente al embudo actual (Grupo A). El objetivo original era determinar si el Grupo B lograba un aumento de al menos **10% en la conversión** de cada etapa del embudo `product_page → product_cart → purchase`, dentro de los 14 días posteriores al registro del usuario.

La prueba fue heredada sin documentación adicional de sus creadores originales, por lo que el análisis también incluyó una **auditoría del diseño experimental**.

## Metodología

1. **Carga, limpieza y exploración** de los 4 datasets de la prueba (participantes, eventos, calendario de marketing).
2. **Validación del diseño experimental**: verificación de tamaño de muestra, balance entre grupos, segmentación geográfica y estructura del embudo.
3. **Prueba de hipótesis**: prueba z de proporciones por etapa del embudo, con corrección de Bonferroni para comparaciones múltiples.
4. **Análisis complementario**: comparación de actividad por usuario (prueba de Mann-Whitney U) y análisis de patrones temporales.
5. **Visualizaciones**: embudo comparativo y gráficos de conversión con significancia estadística marcada.

## Herramientas

Python · pandas · matplotlib/seaborn · SciPy (pruebas z, Mann-Whitney U, corrección de Bonferroni)

## Hallazgos clave: fallas en la ejecución de la prueba

Antes de evaluar resultados, el análisis detectó que la prueba **no se ejecutó según lo diseñado**:
- Tamaño de muestra real de 3,675 participantes (39% menor al esperado de ~6,000).
- Desbalance entre grupos: 74.8% (A) vs. 25.2% (B), en vez de una asignación equilibrada.
- 5.3% de los participantes no pertenecían a la región UE especificada en el diseño (se filtraron para el análisis).
- El embudo no resultó estrictamente secuencial: 68.9% de los compradores no registraron un evento `product_cart` previo, consistente con un patrón de compra directa, no con un fallo de tracking.

## Resultados de la prueba de hipótesis

Usando una prueba z de proporciones con corrección de Bonferroni (α ajustado = 0.0167):

- **product_page:** el Grupo B tuvo una conversión significativamente **menor** que A (56.21% vs. 64.71%, p<0.001) — resultado robusto.
- **product_cart:** sin diferencia estadísticamente significativa entre A y B (p=0.21).
- **purchase:** la diferencia no resistió la corrección por comparaciones múltiples.

**En ningún caso se observó la mejora de +10% esperada para el Grupo B.**

## Recomendación

No se recomienda implementar el nuevo embudo de pago (Grupo B) en su forma actual: no logró el objetivo de mejora planteado y tuvo un desempeño significativamente peor que el embudo actual en la etapa más temprana. Se recomienda repetir la prueba corrigiendo las fallas de ejecución identificadas (tamaño de muestra, balance entre grupos y segmentación geográfica) antes de sacar conclusiones definitivas sobre el sistema de recomendaciones en sí.

## Limitaciones

El desbalance muestral entre grupos reduce el poder estadístico de las pruebas, especialmente para el Grupo B (más pequeño). Los resultados reflejan una ejecución con fallas de diseño y no representan necesariamente el rendimiento potencial del sistema de recomendaciones bajo condiciones experimentales correctas.
