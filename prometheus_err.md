msg="Error loading config (--config.file=prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file prometheus.yml: yaml: line 46: did not find expected key"

# my global config
global:
  scrape_interval: 5s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 5s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9091"]

  - job_name: "node_exporter" #add job node_exporter
    static_configs:
      - targets: ["localhost:9100"]

  #windows_exporter metrics
  - job_name: "windows_exporter"
    static_configs:
      - targets: ["192.168.130.10:9182"]


# snmp_exporter
  - job_name: "snmp_exporter"
    static_configs:
      - targets: ["localhost:9116"]

# snmp device config
    - job_name: "ciscodev"
        static_configs:
         - targets: 
           - 172.16.15.73 # snmp device
           - 172.16.15.72
        metrics_path: /snmp
        params:
          auth: [mht_v2]
          module: [cisco]
        relabel_configs:
         - source_labels: [__address__]
           target_label: __param_target
         - source_labels: [__param_target]
           target_label: instance
         - target_label: __address__
           replacement: localhost:9116 # the snmp exporter's real hostname:port



Error:
msg="Error loading config (--config.file=prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file prometheus.yml: yaml: line 50: found character that cannot start any token"

