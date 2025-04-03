# ITBench Scenarios

This repository contains scenarios and environment setup scripts for the [ITBench](https://github.com/IBM/ITBench) project.

ITBench scenarios simulate realistic IT problems in operational environments for AI agents to solve. Examples include resolving a "High error rate on service order-management" in Kubernetes or assessing compliance posture after detecting a "new control rule for RHEL 9."

## [CISO Scenarios](./ciso)

These scenarios simulate compliance-related misconfigurations. Each scenario provides:

- A pre-configured environment with specific compliance issues
- Tools to detect misconfigurations
- Validation methods to verify successful remediation CISO scenarios are located [here](./ciso).

## [SRE Scenarios](./sre)

These scenarios focus on observability and incident response. Each scenario includes:

- A comprehensive observability stack deployment featuring:
  - Prometheus for metrics collection
  - Grafana for visualization and single mode of API interactions for agents 
  - Loki for log aggregation
  - Elasticsearch and OpenSearch for search and analytics
  - Jaeger for distributed tracing
  - Kubernetes events exporter
- Simulated faults that trigger service degradation
- Thereby leading to alerts associated with application performance issues such as increased error rates and latency spikes
  
SRE scenarios are located [here](./sre).

## [FinOps Scenarios](./sre)

Each scenario includes:
- The core SRE observability stack
- OpenCost integration for cost monitoring
- Simulated faults trigger cost overrun alerts

FinOps scenarios are located [here](./sre) (alongside SRE scenarios).
