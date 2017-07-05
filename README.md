# Realtime VPG Metric Monitor for SORAOM Junction

![SCREENSHOT](https://github.com/kosho/soracom-realtime-vpg-metrics/raw/master/soracom-realtime-vpg-metrics-screenshot.png)

## Prerequirements

- A proper account and [setup of SORACOM Junction](https://dev.soracom.io/jp/start/junction_inspection/) 
- A valid [Elastic Cloud](https://cloud.elastic.co) account or equivalent setup of Elasticsearch and Kibana

## Setup and Run

### Elastticsearch cluster

1. Create a new cluster from the Elastic Cloud admin console with Elasticsearch version 5.4.3 or above. The trial cluster or the minimal size cluster should work for most of cases but choosing "High Availability" is strongly recommended to avoid possible data loss.

2. Enable a Kibana endpoint from the cluster configuration and open it with a web browser.
3. Go to **Management** > **Security** > **Roles**, then create a new role `ingest_only` with `index` and `create_index` privilleges for `soracom-*` indices.
4. Go to **Users**, and create a new user `ingest` with the `ingest_only` role.
5. Go to **Console** from the **Dev Tools** or open up the terminal, type in the following command to register the template.

```
$ curl -u elastic:ES_PASSWORD https://ES_ENDPOINT:9243/_template/soracom --data-binary @soracom-realtime-vpg-metrics-template.json
```

### SORACOM Junction

Set up the SORACOM junction by refering [the guide](https://dev.soracom.io/jp/start/junction_inspection/). The user `ingest` will be used for supplying the metrics.

### Kibana

Once you are sure the metrics is properly stored on your Elasticsearch cluster:

1. Add `soracom-*` to the index patterns from **Management** > **Index Patterns**.
2. Move to **Saved Objects** and import the `soracom-realtime-vpg-metrics-dashboard.json`
3. Go to **Dashboard** and choose **Soracom Realtime VPG Metrics**.

