# Dshackle Helm Chart

Deploy [Dshackle](https://github.com/emeraldpay/dshackle) inside Kubernetes with ease

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) ![Version: 0.1.0](https://img.shields.io/badge/Version-0.1.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.12.0](https://img.shields.io/badge/AppVersion-0.12.0-informational?style=flat-square)

## Features

- Actively maintained by [GraphOps](https://graphops.xyz) and contributors
- Supports deploying an in-memory Redis LRU Cache for blocks and transactions
- Strong security defaults (non-root execution, ready-only root filesystem, drops all capabilities)
- Readiness checks to ensure traffic only hits `Pod`s that are healthy and ready to serve requests
- Support for `PodMonitor`s to configure Prometheus to scrape metrics ([prometheus-operator](https://github.com/prometheus-operator/prometheus-operator))
- Support for configuring Grafana dashboards ([grafana](https://github.com/grafana/helm-charts/tree/main/charts/grafana))

## Quickstart

To install the chart with the release name `my-release`:

```console
$ helm repo add graphops http://graphops.github.io/helm-charts
$ helm install my-release graphops/dshackle
```

## Upgrading

We recommend that you pin the version of the Chart that you deploy. You can use the `--version` flag with `helm install` and `helm upgrade` to specify a chart version constraint.

This project uses [Semantic Versioning](https://semver.org/). Changes to the version of the application (the `appVersion`) that the Chart deploys will generally result in a patch version bump for the Chart. Breaking changes to the Chart or its `values.yaml` interface will be reflected with a major version bump.

We do not recommend that you upgrade the application by overriding `image.tag`. Instead, use the version of the Chart that is built for your desired `appVersion`.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| chainAlias | string | `"alias"` |  |
| chainType | string | `"ethereum"` |  |
| extraArgs | list | `[]` | Extra arguments for DShackle |
| extraConfig | object | `{}` |  |
| fullnameOverride | string | `""` |  |
| grafana.dashboards | bool | `false` | Enable creation of Grafana dashboards. [Grafana chart](https://github.com/grafana/helm-charts/tree/main/charts/grafana#grafana-helm-chart) must be configured to search this namespace, see `sidecar.dashboards.searchNamespace` |
| grafana.dashboardsConfigMapLabel | string | `"grafana_dashboard"` | Must match `sidecar.dashboards.label` value for the [Grafana chart](https://github.com/grafana/helm-charts/tree/main/charts/grafana#grafana-helm-chart) |
| grafana.dashboardsConfigMapLabelValue | string | `""` | Must match `sidecar.dashboards.labelValue` value for the [Grafana chart](https://github.com/grafana/helm-charts/tree/main/charts/grafana#grafana-helm-chart) |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"emeraldpay/dshackle"` | Image for Dshackle |
| image.tag | string | Chart.appVersion | Overrides the image tag |
| imagePullSecrets | list | `[]` | Pull secrets required to fetch the Image |
| lruCache.cacheSizeGB | int | `1` | Maximum size of the LRU cache in gigabytes. This must fit in-memory.  |
| lruCache.enabled | bool | `true` | Enable deploying a Redis-powered LRU cache sidecar alongside Dshackle |
| lruCache.image.pullPolicy | string | `"IfNotPresent"` |  |
| lruCache.image.repository | string | `"redis"` | Image for Redis |
| lruCache.image.tag | string | `"6.2.6"` |  |
| lruCache.resources | object | `{}` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| podAnnotations | object | `{}` | Annotations for the `Pod` |
| podSecurityContext | object | `{"fsGroup":101337,"runAsGroup":101337,"runAsNonRoot":true,"runAsUser":101337}` | Pod-wide security context |
| prometheus.podMonitors.enabled | bool | `false` | Enable monitoring by creating `PodMonitor` CRDs ([prometheus-operator](https://github.com/prometheus-operator/prometheus-operator)) |
| prometheus.podMonitors.interval | string | `nil` |  |
| prometheus.podMonitors.labels | object | `{}` |  |
| prometheus.podMonitors.relabelings | list | `[]` |  |
| prometheus.podMonitors.scrapeTimeout | string | `nil` |  |
| resources | object | `{}` |  |
| service.ports.grpc-dshackle | int | `2449` | Service Port to expose the Dshackle gRPC API on |
| service.ports.http-jsonrpc | int | `8545` | Service Port to expose the JSON-RPC API on |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |
| terminationGracePeriodSeconds | int | `30` | Amount of time to wait before force-killing containers |
| tolerations | list | `[]` |  |
| upstreams[0].chain | string | `"ethereum"` |  |
| upstreams[0].connection.ethereum.rpc.url | string | `"http://eth-mainnet-erigon-rpcdaemons:8545"` |  |
| upstreams[0].connection.ethereum.ws.frameSize | string | `"32mb"` |  |
| upstreams[0].connection.ethereum.ws.msgSize | string | `"256mb"` |  |
| upstreams[0].connection.ethereum.ws.url | string | `"ws://eth-mainnet-erigon-rpcdaemons:8545"` |  |
| upstreams[0].id | string | `"local"` |  |
| upstreams[0].labels | object | `{}` |  |
| upstreams[0].methods.enabled[0].name | string | `"eth_chainId"` |  |
| upstreams[0].methods.enabled[0].static | string | `"0x1"` |  |
| upstreams[0].methods.enabled[1].name | string | `"eth_getLogs"` |  |
| upstreams[0].methods.enabled[2].name | string | `"trace_filter"` |  |
| upstreams[0].role | string | `"standard"` |  |

## Contributing

We welcome and appreciate your contributions! Please see the [Contributor Guide](/CONTRIBUTING.md), [Code Of Conduct](/CODE_OF_CONDUCT.md) and [Security Notes](/SECURITY.md) for this repository.