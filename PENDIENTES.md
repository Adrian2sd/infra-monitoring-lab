# Pendientes de la versión de portfolio

- Obtener desde `monitor01` la copia final real de `/etc/prometheus/prometheus.yml`; comprobar que no hay secretos y subirla a `configs/prometheus/`.
- Exportar el dashboard de Grafana como JSON a `dashboards/infrastructure-monitoring.json` y comprobar su importación.
- Revisar el origen de los desfases del reloj tras reanudar VMs; dejarlo como pendiente mientras no se compruebe.
- Revisar si conviene añadir evidencias adicionales: capturas de YAML Netplan final y de comprobación persistente de NTP.