---
name: panel-de-analiticas
description: >
  Convierte una exportación de LinkedIn Analytics en un dashboard interactivo de React con tema oscuro, más un análisis estratégico escrito con 5 recomendaciones de contenido respaldadas por datos. Lee cada hoja de la exportación, construye gráficos de tendencia de engagement, crecimiento de seguidores, dispersión de rendimiento de publicaciones, mapa de calor por día de la semana y desglose de audiencia. Usa esta skill cada vez que el usuario diga "analiza mi linkedin", "analíticas de linkedin", "crea mi dashboard", "revisa mi rendimiento", o suba un archivo de exportación de LinkedIn Analytics. Requiere como entrada la exportación de LinkedIn Analytics del usuario (xlsx).
---

# Dashboard de Analíticas

## CRÍTICO: Auto-inicio al cargar

Cuando esta skill se active, ve directo al Paso 1.

## Paso 1. Obtén el archivo de exportación

Pregunta:

> Sube tu archivo de exportación de LinkedIn Analytics (xlsx).
>
> ¿No estás seguro de cómo obtenerlo? Ve a LinkedIn Analytics, define tu rango de fechas (30, 60 o 90 días funciona bien) y haz clic en Export en la esquina superior derecha.

Espera a que se suba el archivo.

## Paso 2. Parsea los datos

Lee cada hoja del archivo. Espera estas hojas:

- **DISCOVERY**: impresiones y alcance generales
- **ENGAGEMENT**: impresiones y engagements diarios a lo largo del tiempo
- **TOP POSTS**: las 50 mejores publicaciones, ordenadas por engagements y por impresiones (dos tablas para combinar)
- **FOLLOWERS**: nuevos seguidores diarios más el conteo total
- **DEMOGRAPHICS**: cargos, ubicaciones, industrias, antigüedad, tamaño de empresa, principales empresas

Limpia cualquier encabezado desordenado. Combina las dos tablas de TOP POSTS (por engagements y por impresiones) en un único conjunto de datos unificado por publicación. Elimina duplicados.

## Paso 3. Construye el dashboard interactivo

Crea un único artefacto de React. Tema oscuro (fondo `#0f1117`), colores de acento para los gráficos. Usa Recharts para todas las visualizaciones.

Incluye estos paneles en este orden:

### Métricas destacadas (tarjetas de la fila superior)
- Impresiones totales
- Alcance total
- Nuevos seguidores totales
- Promedio diario de impresiones
- Promedio diario de engagements
- Tasa de engagement promedio (engagements / impresiones)
- Total de publicaciones registradas

### Tendencia de engagement (gráfico de líneas)
- Impresiones diarias (eje y izquierdo) y engagements (eje y derecho) en todo el rango de fechas
- Resalta los 3 días pico principales con marcadores

### Crecimiento de seguidores (gráfico de área)
- Nuevos seguidores diarios
- Línea de tendencia de media móvil de 7 días superpuesta
- Ganancia acumulada de seguidores

### Dispersión de rendimiento de publicaciones
- Eje X: impresiones. Eje Y: engagements
- Codifica las publicaciones por color en cuatro cuadrantes:
  - **Estrellas**: alto alcance + alto engagement
  - **Virales pero superficiales**: alto alcance + bajo engagement
  - **Oro de nicho**: bajo alcance + alto engagement
  - **De bajo rendimiento**: bajo alcance + bajo engagement
- Puntos con hover que muestren la URL y la fecha de la publicación

### Mapa de calor por día de la semana
- Promedio de impresiones y engagements por día de la semana
- Resalta los días más fuertes

### Desglose de audiencia (gráficos de barras)
- Cargos
- Industrias
- Antigüedad
- Tamaño de empresa
- Principales ubicaciones

### Reglas de formato
- Formatea los números: `67K` no `67000`, `1.2M` no `1200000`
- Conteo total de seguidores destacado en la parte superior
- Diseño responsivo (funciona en laptop y en pantalla grande)
- Fondo oscuro, colores de gráficos de alto contraste

## Paso 4. Análisis estratégico escrito

Debajo del dashboard, escribe un análisis conciso con estas secciones:

### Resumen de rendimiento
- Trayectoria: en crecimiento, estancada o en declive (usa las líneas de tendencia)
- Tasa de engagement actual y cómo se compara con los benchmarks de LinkedIn para cuentas de este tamaño

### Patrones de las mejores publicaciones
- Analiza el top 10 por impresiones y el top 10 por engagements
- Patrones: día de publicación, momento del mes, temas de contenido
- Altas impresiones + bajo engagement: ¿qué señala eso?
- Bajas impresiones + alto engagement: ¿qué señala eso?

### Ajuste audiencia-contenido
- Quién es la audiencia central, según las demografías
- Qué temas y formatos de contenido resonarían
- Segmentos en los que apoyarse o de los que alejarse

### Velocidad de crecimiento
- Crecimiento promedio diario de seguidores
- Proyecciones a 30, 60 y 90 días al ritmo actual
- Tendencias de aceleración o desaceleración

### Estrategia de días y horarios
- Mejores días para impresiones
- Mejores días para engagement
- Calendario óptimo de publicación según los datos

### 5 recomendaciones de contenido específicas
Cada una incluye:
- Ángulo o tema de contenido
- Por qué los datos lo respaldan
- A qué segmento de audiencia apunta
- Impacto esperado según los patrones en los datos

## Paso 5. Ofrece el siguiente paso

Después del análisis:

> ¿Quieres que redacte una de estas 5 recomendaciones como una publicación completa? Llama a la skill redactor-de-publicaciones o formateador-de-publicaciones con el número de la recomendación.

## Reglas

- Usa números, no adjetivos. "La tasa de engagement es 2.3%" le gana a "el engagement es saludable".
- Mantén el análisis directo. Sin relleno, sin paja.
- Nunca inventes métricas que no estén presentes en la exportación.
- Señala los problemas de calidad de datos (columnas faltantes, rangos de fechas extraños) en lugar de trabajarlos silenciosamente.
- Nunca uses rayas (em dashes).
- Español (Latinoamérica) a menos que voice.md especifique lo contrario.
- Recomienda ejecutar esto mensualmente. Los patrones solo surgen con el tiempo.
