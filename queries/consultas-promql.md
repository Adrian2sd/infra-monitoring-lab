# Consultas PromQL utilizadas

Todas las expresiones se ejecutan en **Prometheus Graph** o en **Grafana (Code)**, nunca directamente en Bash.

## Disponibilidad

```promql
up{job="node"}
```

`1` indica que el último scrape fue correcto; `0`, que falló. Para ver la serie temporal de `servidor01`:

```promql
up{job="node",instance="192.168.56.20:9100"}
```

## CPU utilizada (%)

```promql
100 * (1 - avg by (instance) (rate(node_cpu_seconds_total{job="node",mode="idle"}[5m])))
```

Un `rate` a 5 minutos suaviza picos; en pruebas breves se puede usar `[1m]` con suficiente frecuencia de muestreo. Para filtrar series históricas, usar las instancias activas:

```promql
100 * (1 - avg by (instance) (rate(node_cpu_seconds_total{job="node",mode="idle",instance=~"192\.168\.56\.(10|20):9100"}[5m])))
```

## RAM utilizada (%)

```promql
100 * (1 - node_memory_MemAvailable_bytes{job="node"} / node_memory_MemTotal_bytes{job="node"})
```

`MemAvailable` refleja memoria estimada disponible incluyendo caché recuperable.

## Disco raíz utilizado (%)

```promql
100 * (1 - node_filesystem_avail_bytes{job="node",mountpoint="/"} / node_filesystem_size_bytes{job="node",mountpoint="/"})
```

Esta métrica toma el espacio disponible para usuarios no privilegiados; puede diferir del porcentaje mostrado por `df` según reservas del sistema de archivos.

## Seleccionar el entorno

Se comprobó la etiqueta `environment="lab"` en los targets nuevos. Si un filtro no devuelve datos, ejecutar primero `up` y revisar las etiquetas reales de la serie en la respuesta de Prometheus antes de añadir restricciones.
