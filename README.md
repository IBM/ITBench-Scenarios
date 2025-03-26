# ITBench scenarios

This repository provides environment setup and deployment tooling for benchmark scenarios used in [ITBench](https://github.com/IBM/ITBench), a framework for evaluating AI agents on real-world IT automation tasks.

Each scenario simulates a realistic IT incident or compliance check. Scenarios are categorized by role—Site Reliability Engineering (SRE), Financial Operations (FinOps), and Compliance/Security (CISO)—and are deployed into operational environments (e.g., Kubernetes clusters or Linux hosts) with injected issues that agents must detect and resolve.

## 🔧 Scenario categories

- **SRE:** Service outages, performance degradations, and observability-based incident response.
- **FinOps:** Cost inefficiencies, resource misallocation, and budget policy enforcement.
- **CISO:** Security misconfigurations and compliance rule violations across system configurations.

Scenarios can be used to evaluate AI agents’ ability to understand, investigate, and remediate real-world problems.

## 🚀 Getting started

> 🛠️ For local testing and development only. For hosted access, see the [main ITBench repo](https://github.com/IBM/ITBench).

### Prerequisites

- A running Kubernetes cluster (e.g., KinD, Minikube, or remote)
- Docker or Podman
- `kubectl` and common CLI tools
- Optionally: a RHEL 9 virtual machine for certain CISO scenarios

### Setup

1. **Clone this repo**

   ```bash
   git clone https://github.com/IBM/ITBench-Scenarios.git
   cd ITBench-Scenarios
   ```

2. **Deploy a scenario bundle**

   Use the Makefile to deploy all components for a given scenario category:

   ```bash
   make deploy_bundle
   ```

   This will install:
   - A sample application (e.g., microservices)
   - Monitoring tools (e.g., Prometheus, Grafana, Loki)
   - Fault injection utilities

3. **Inject a fault**

   Simulate a failure or misconfiguration for the agent to resolve:

   ```bash
   make inject_fault
   ```

4. **Get scenario goal**

   Retrieve a textual description of the issue for the agent:

   ```bash
   make get
   ```

   This can be passed as the `--goal` argument to compatible agents.

## 📁 Repository structure

```
.
├── scenarios/
│   ├── sre/
│   ├── finops/
│   └── ciso/
├── Makefile
├── scripts/
└── README.md
```

- `scenarios/`: Scenario definitions grouped by persona
- `Makefile`: Common commands for deployment and fault injection
- `scripts/`: Helper utilities for scenario operations

## 🤖 Agent usage

Once the scenario is deployed and a fault has been injected, you can run an agent to attempt remediation. See:

- [SRE agent](https://github.com/IBM/itbench-sre-agent)
- [CISO compliance agent](https://github.com/IBM/itbench-ciso-caa-agent)

Agents should be configured to access the environment (e.g., using `KUBECONFIG` or mounting volumes). Refer to each agent’s README for usage details.

## 🧪 Extending scenarios

To contribute a new scenario:

1. Create a new folder under `scenarios/sre/`, `scenarios/finops/`, or `scenarios/ciso/`.
2. Include:
   - Kubernetes manifests or host-level config
   - Fault injection logic
   - A scenario goal and description
3. Add Makefile targets if needed.
4. Test locally and submit a pull request.

For more, see the contribution guidelines in the [main ITBench repo](https://github.com/IBM/ITBench).

## 📝 License

This project is licensed under the [Apache License 2.0](../LICENSE).