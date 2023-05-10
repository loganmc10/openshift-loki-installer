# Usage
The target cluster requires NooBaa (ODF Multicloud Object Gateway).

loki_size defaults to ```1x.small```. See https://docs.openshift.com/container-platform/latest/logging/cluster-logging-loki.html#deployment-sizing_cluster-logging-loki for possible options.
```
export KUBECONFIG=~/path/to/kubeconfig
ansible-playbook loki-playbook.yaml -e "loki_size=1x.extra-small"
```

Optional arguments:
```
-e "auditing=true"
-e "remote_logging=true"
```

## Remote Logging

When `remote_logging` is enabled, the playbook writes a file with a Secret named `<loki_route>-secret.yaml` to the playbook directory.

This Secret can be applied to the remote cluster, and then used for log forwarding, for example:
```
apiVersion: logging.openshift.io/v1
kind: ClusterLogForwarder
metadata:
  name: instance
  namespace: openshift-logging
spec:
  outputs:
    - name: loki-app
      type: loki
      url: https://logging-loki-openshift-logging.apps.cluster.example.com/api/logs/v1/application
      secret:
        name: remote-logger-secret
...
  pipelines:
    - name: send-app-logs
      inputRefs:
        - application
      outputRefs:
        - loki-app
      labels:
        clustername: <cluster_name>
...
```
