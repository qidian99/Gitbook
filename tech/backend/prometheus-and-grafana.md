# Prometheus and Grafana

{% embed url="https://www.scaleway.com/en/docs/configure-prometheus-monitoring-with-grafana/" %}

## Prometheus

### Installation and configuration

#### Volumes & bind-mount <a id="volumes-bind-mount"></a>

Bind-mount your `prometheus.yml` from the host by running:

```text
docker run \
    -p 9090:9090 \
    -v /tmp/prometheus.yml:/etc/prometheus/prometheus.yml \
    prom/prometheus
```

Or use an additional volume for the config:

```text
docker run \
    -p 9090:9090 \
    -v /path/to/config:/etc/prometheus \
    prom/prometheus
```

#### Custom image <a id="custom-image"></a>

To avoid managing a file on the host and bind-mount it, the configuration can be baked into the image. This works well if the configuration itself is rather static and the same across all environments.

For this, create a new directory with a Prometheus configuration and a `Dockerfile` like this:

```text
FROM prom/prometheus
ADD prometheus.yml /etc/prometheus/
```

Now build and run it:

```text
docker build -t my-prometheus .
docker run -p 9090:9090 my-prometheus
```

### Prometheus API

{% embed url="https://prometheus.io/docs/prometheus/latest/querying/api/" %}

## Node Exporter

```text
docker run -d \
  --net="host" \
  --pid="host" \
  -v "/:/host:ro,rslave" \
  quay.io/prometheus/node-exporter \
  --path.rootfs=/host
```

{% embed url="https://prometheus.io/docs/guides/node-exporter/" %}

### Exploring Node Exporter metrics through the Prometheus expression browser <a id="exploring-node-exporter-metrics-through-the-prometheus-expression-browser"></a>

Now that Prometheus is scraping metrics from a running Node Exporter instance, you can explore those metrics using the Prometheus UI \(aka the [expression browser](https://prometheus.io/docs/visualization/browser)\). Navigate to `localhost:9090/graph` in your browser and use the main expression bar at the top of the page to enter expressions. The expression bar looks like this:

![Prometheus expressions browser](https://prometheus.io/assets/prometheus-expression-bar.png)

Metrics specific to the Node Exporter are prefixed with `node_` and include metrics like `node_cpu_seconds_total` and `node_exporter_build_info`.

Click on the links below to see some example metrics:

| Metric | Meaning |
| :--- | :--- |
| [`rate(node_cpu_seconds_total{mode="system"}[1m])`](http://localhost:9090/graph?g0.range_input=1h&g0.expr=rate%28node_cpu_seconds_total%7Bmode%3D%22system%22%7D%5B1m%5D%29&g0.tab=1) | The average amount of CPU time spent in system mode, per second, over the last minute \(in seconds\) |
| [`node_filesystem_avail_bytes`](http://localhost:9090/graph?g0.range_input=1h&g0.expr=node_filesystem_avail_bytes&g0.tab=1) | The filesystem space available to non-root users \(in bytes\) |
| [`rate(node_network_receive_bytes_total[1m])`](http://localhost:9090/graph?g0.range_input=1h&g0.expr=rate%28node_network_receive_bytes_total%5B1m%5D%29&g0.tab=1) | The average network traffic received, per second, over the last minute \(in bytes\) |

## Install Grafana

{% embed url="https://grafana.com/docs/grafana/latest/installation/mac/" %}



