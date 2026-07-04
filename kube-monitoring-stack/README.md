
<div align="center">
<h3 align="center">SIS Monitoring & Observability Platform</h3>

  <p align="center">
    This repository holds the core infra-as-code configuration for the SIS Monitoring & Observability platform.
    <br />
    <br />
    <a href="https://grafana.devops.core.sis.tv">Grafana Portal</a>
    ·
    <a href="#">Report Bug [placeholder]</a>
    ·
    <a href="#">Request Feature [placeholder]</a>
  </p>
</div>


<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#directory-structure">Directory Structure</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#personal-learning-path-aks--flux--lgtm">Personal Learning Path (AKS + Flux + LGTM)</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>


<!-- ABOUT THE PROJECT -->
## About The Project

The SIS Monitoring and Observability platform is made of a number of industry standard observability components, based primarily on the Grafana "LGTM" stack.

- Loki is responsible for container and workload **logs**
- Mimir is responsible for platform and application **metrics**
- Tempo (on roadmap) is responsible for applications **spans** and **traces**

These components are deployed via Helm chart using FluxCD - meaning releases and configuration changes follow a GitOps process.  



### Built With

* Grafana 
* Prometheus
* Loki
* Mimir
* Tempo (on roadmap)



<!-- GETTING STARTED -->
## Getting Started

To gain insight into the platform in order to help understand design decisions, please check out the Monitoring space in Confluence. 

This repository is watched by several Flux agents for changes. A flux agent exists on each of the internal & external AKS clusters, as well as in the `devops-core-uks-aks-1` cluster where the main SIS Monitoring platform lives.

### Directory Structure

This repository is structured in typical Flux / Kustomize monorepo fashion - using bases and overlays. More infomation [here](https://fluxcd.io/flux/guides/repository-structure/#monorepo)

* **apps** - Contain the application offerings split by environment.
* **infrastructure** - Contains any accompanying manifests which get applied *before* the applications in the Apps folder.



<!-- USAGE EXAMPLES -->
## Usage

- Follow Flux reconciliation health using:
  - `flux get sources git`
  - `flux get kustomizations`
- Validate monitoring namespace resources:
  - `kubectl get ns monitoring`
  - `kubectl get pods -n monitoring`

## Personal Learning Path (AKS + Flux + LGTM)

This repository now includes a learning scaffold for personal AKS environments:

- `/home/runner/work/kube-monitoring-stack/kube-monitoring-stack/kube-monitoring-stack/clusters/personal/dev`
- `/home/runner/work/kube-monitoring-stack/kube-monitoring-stack/kube-monitoring-stack/infrastructure/personal/dev`
- `/home/runner/work/kube-monitoring-stack/kube-monitoring-stack/kube-monitoring-stack/apps/personal/dev`

### Phase 1: AKS + Flux + repository structure (no LGTM yet)
- **What:** bootstrap Flux and reconcile infrastructure first, then apps.
- **Why:** GitOps reconciliation must be stable before observability components are added.
- **How to verify:**
  - `flux get sources git` shows Ready=True
  - `flux get kustomizations` shows Ready=True for infrastructure and apps

### Phase 2: Start with metrics first (Prometheus + Grafana)
- **What:** deploy only `prometheus-stack` from `apps/personal/dev`.
- **Why first:** metrics are the quickest signal for platform health and capacity.
- **How to verify:**
  - Prometheus targets are mostly `UP`
  - Grafana dashboard panels return live node/pod metrics
  - Alertmanager pod is healthy

### Phase 3+: Expand gradually
- Add **Loki** next for logs (root-cause analysis).
- Add **Tempo** after app instrumentation for traces.
- Add **Mimir** for longer retention and scale-out metrics backend.

### Healthy monitoring checklist
- Flux sources and kustomizations are Ready=True
- Monitoring namespace and secrets exist
- HelmRelease objects are Ready=True
- No CrashLoopBackOff in monitoring pods
- Grafana datasources are healthy
- Prometheus targets remain stable over time




<!-- ROADMAP -->
## Roadmap  [TODO]

- [ ] Feature 1
- [ ] Feature 2
- [ ] Feature 3




<!-- CONTRIBUTING -->
## Contributing


1. Clone the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

[TODO] - add prometheus scrape configuration example 





<!-- CONTACT -->
## Contact

For any further information about the platform, please contact the Cloud Platform Team.





<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [Prometheus configuration reference](https://prometheus.io/docs/prometheus/latest/configuration/configuration)
* [Grafana Azure Auth configuration](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/configure-authentication/azuread/)
* [kube-prometheus-stack helm chart](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack)
* [Loki helm chart](https://github.com/grafana/loki/tree/main/production/helm/loki)
* [Mimir helm chart](https://github.com/grafana/mimir/tree/main/operations/helm/charts/mimir-distributed)

