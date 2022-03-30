# The Catalyst Squad Minimum Viable Kubernetes Platform

On top of the [Infrastructure](infrastructure.md) layer of the Catalyst Squad Platform sits Kubernetes. There is at least one cluster in each cloud environment, nonprod, prod, and common.

All logs and metrics are sent to common. Common will also have services that nonprod and prod need that will be unique to every use case. However, aside from this, all clusters will at a minimum provide the following services (either from the cloud provider or our base configuration):

- [Cluster Autoscaler](https://github.com/kubernetes/autoscaler)
- [Metrics Server](https://github.com/kubernetes-sigs/metrics-server)
- [External-DNS](https://github.com/kubernetes-sigs/external-dns)
- [Cert Manager](https://github.com/cert-manager/cert-manager)
- [Sentry (in Common)](https://github.com/getsentry/sentry)
- [Prometheus](https://github.com/prometheus/prometheus)
- [Loki](https://github.com/grafana/loki)
- [Grafana](https://github.com/grafana/grafana)
- [ArgoCD](https://github.com/argoproj/argo-cd)
- [An Ingress Controller (currently Contour)](https://github.com/projectcontour/contour)
- [LinkerD](https://github.com/linkerd/linkerd2)
- [PVC Autoresizer](https://github.com/topolvm/pvc-autoresizer)
- [FusionAuth if Needed](https://github.com/FusionAuth)

## How it is organized

The infrastructure layer is created through Infrastucture as Code. Currently we're using Pulumi to do this.

Pulumi is also used to install ArgoCD. After that, everything else is installed with an [ArgoCD App of Apps](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/) we maintain a [helm chart](https://github.com/catalystsquad/chart-platform-services/) for.

Once deployed, the helm chart installs all the ArgoCD Applications for each of the core platform services, hence the name.

That's it, that's all there is to the kubernetes platform. Create a cluster with whatever means needed, install ArgoCD, and install the services chart. ArgoCD will pull the needed charts from their sources and install things to the cluster.

We now have a strong basis to deploy to in a modern development workflow.
