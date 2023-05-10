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
```
