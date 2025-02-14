---
title: Access Kyma application logs
---

Get insights into your applications (microservices, Functions...) by viewing the respective logs.

To check out real-time logs immediately, use the Kubernetes functionalities - either with `kubectl`, or in Kyma Dashboard.

If you want to see historical logs and use additional features, view the logs as aggregated by Loki. In Grafana, you can see a visual representation of your logs, while Loki itself provides an API for programmatic access.

## Kubernetes logs in Kyma Dashboard

You can view real-time logs in Kyma Dashboard:

1. Open Kyma Dashboard and select the Namespace.
2. Access the Pod and select the container.
3. Click **View Logs**.

## Kubernetes logs using kubectl

Alternatively, to see real-time logs in your terminal, run the following command:

```bash
kubectl logs {POD_NAME} --namespace {NAMESPACE_NAME} --container {CONTAINER_NAME}
```

## Loki logs in Grafana UI

> **NOTE:** Loki is [deprecated](https://kyma-project.io/blog/2022/11/2/loki-deprecation/) and is planned to be removed. If you want to install a custom Loki stack, take a look at [Installing a custom Loki stack in Kyma](https://github.com/kyma-project/examples/tree/main/loki).

> **NOTE:** Prometheus and Grafana are [deprecated](https://kyma-project.io/blog/2022/12/9/monitoring-deprecation) and are planned to be removed. If you want to install a custom stack, take a look at [Install a custom kube-prometheus-stack in Kyma](https://github.com/kyma-project/examples/tree/main/prometheus).

To see a visual representation and search for specific logs, follow these steps:

1. In the **Cluster Overview** of Kyma Dashboard, go to **Observability** and open **Grafana**.
2. In Grafana's **Explore** section, select `Loki` as data source.
3. Enter a query following the [query language guidelines](https://grafana.com/docs/loki/latest/logql/), for example:

   ```bash
   {namespace="kyma-system"} |= "info"
   ```

## Loki logs using Loki API

> **NOTE:** Loki is [deprecated](https://kyma-project.io/blog/2022/11/2/loki-deprecation/) and is planned to be removed. If you want to install a custom Loki stack, take a look at [Installing a custom Loki stack in Kyma](https://github.com/kyma-project/examples/tree/main/loki).

To access the logs through the [Loki API](https://grafana.com/docs/loki/latest/api/) directly, follow these steps:

1. Configure port forwarding with the following command:

   ```bash
   kubectl port-forward -n kyma-system service/logging-loki 3100:3100
   ```

2. For example, to get the first 1000 lines of info logs for components in the `kyma-system` Namespace, run the following command:

   ```bash
   curl -X GET -G 'http://localhost:3100/loki/api/v1/query' --data-urlencode 'query={namespace="kyma-system"}' --data-urlencode 'limit=1000' --data-urlencode 'regexp=info'
   ```
