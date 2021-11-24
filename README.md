# Splunk-otel-collector-memory
How to change allocated memory

Splunk Otel Collector - How to change allocated memory
1. Host-based
- To configure memory allocation, you might need to change the --memory parameter. Set SPLUNK_MEMORY_TOTAL_MIB by increasing a setting in "curl -sSL https://dl.signalfx.com/splunk-otel-collector.sh > /tmp/splunk-otel-collector.sh;
sudo sh /tmp/splunk-otel-collector.sh --realm SPLUNK_REALM --memory SPLUNK_MEMORY_TOTAL_MIB \
    -- SPLUNK_ACCESS_TOKEN"
This value will be used to auto-calculate and set other Memory settings for memory limiter and ballast.
- SPLUNK_MEMORY_TOTAL_MIB is actually 90% of SPLUNK_MEMORY_LIMIT_MIB  unless the env variable is already set https://github.com/signalfx/splunk-otel-collector/blob/3636a7832101f903bfb5b8f27913e8689b30ef23/cmd/otelcol/main.go#L59
- Same applies to memory ballast SPLUNK_BALLAST_SIZE_MIB, which is set to 33% of the memory limit - https://github.com/signalfx/splunk-otel-collector/blob/3636a7832101f903bfb5b8f27913e8689b30ef23/cmd/otelcol/main.go#L58

2. K8s-based
- To limit memory you can change as following,
Agent (Daemonset) - https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/values.yaml#L259
Fluentd (if enable to collect log, this will be another container in a daemonset) - https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/values.yaml#430
ClusterReceiver - https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/values.yaml#L307


Addtion useful link 
- agent code: https://github.com/signalfx/splunk-otel-collector/blob/3636a7832101f903bfb5b8f27913e8689b30ef23/cmd/otelcol/main.go
- list of environment variables: https://github.com/signalfx/splunk-otel-collector/blob/3636a7832101f903bfb5b8f27913e8689b30ef23/cmd/otelcol/main.go#L44
