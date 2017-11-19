---
layout:     post
title:      "Heapster Metrics"
subtitle:   "Kubernetes"
date:       2017-11-16
author:     "Charles"
tags:
    - Kubernetes
    - Monitoring
---

The Heapster Model is a structured representation of metrics for Kubernetes clusters, which is exposed through a set of REST API endpoints. It allows the extraction of up to 15 minutes of historical data for any Container, Pod, Node or Namespace in the cluster, as well as the cluster itself (depending on the metric).

#### Cluster-level Metrics

/api/v1/model/metrics/: Returns a list of available cluster-level metrics.

/api/v1/model/metrics/{metric-name}?start=X&end=Y: Returns a set of (Timestamp, Value) pairs for the requested cluster-level metric, between the time range specified by start and end.

#### Node-level Metrics

/api/v1/model/nodes/: Returns a list of all available nodes.

/api/v1/model/nodes/{node-name}/metrics/: Returns a list of available node-level metrics.

/api/v1/model/nodes/{node-name}/metrics/{metric-name}?start=X&end=Y: Returns a set of (Timestamp, Value) pairs for the requested node-level metric, within the time range specified by start and end.

#### Namespace-level Metrics

/api/v1/model/namespaces/: Returns a list of all available namespaces.

/api/v1/model/namespaces/{namespace-name}/metrics/: Returns a list of available namespace-level metrics.

/api/v1/model/namespaces/{namespace-name}/metrics/{metric-name}?start=X&end=Y: Returns a set of (Timestamp, Value) pairs for the requested namespace-level metric, within the time range specified by start and end.

#### Pod-level Metrics

/api/v1/model/namespaces/{namespace-name}/pods/: Returns a list of all available pods under a given namespace.

/api/v1/model/namespaces/{namespace-name}/pods/{pod-name}/metrics/: Returns a list of available pod-level metrics

/api/v1/model/namespaces/{namespace-name}/pods/{pod-name}/metrics/{metric-name}?start=X&end=Y: Returns a set of (Timestamp, Value) pairs for the requested pod-level metric, within the time range specified by start and end.

#### Container-level Metrics

Container metrics and stats are accessible for both containers that belong to pods, as well as for free containers running in each node.

/api/v1/model/namespaces/{namespace-name}/pods/{pod-name}/containers/: Returns a list of all available containers under a given pod. Note: Not Supported only in V1.2.0.

/api/v1/model/namespaces/{namespace-name}/pods/{pod-name}/containers/{container-name}/metrics/: Returns a list of available container-level metrics

/api/v1/model/namespaces/{namespace-name}/pods/{pod-name}/containers/{container-name}/metrics/{metric-name}?start=X&end=Y: Returns a set of (Timestamp, Value) pairs for the requested container-level metric, within the time range specified by start and end.

/api/v1/model/nodes/{node-name}/freecontainers/: Returns a list of all available free containers under a given node.

/api/v1/model/nodes/{node-name}/freecontainers/{container-name}/metrics/: Returns a list of available container-level metrics

/api/v1/model/nodes/{node-name}/freecontainers/{container-name}/metrics/{metric-name}?start=X&end=Y: Returns a set of (Timestamp, Value) pairs for the requested container-level metric, within the time range specified by start and end.

<!-- https://github.com/kubernetes/heapster/blob/master/docs/model.md -->

