# TCRB custom alert rule

Created: December 23, 2022 11:46 PM
Last Edited Time: December 23, 2022 11:58 PM
Type: TCRB
tag: tcrb

### custom prometheus alertrule

**create** `custom-highmem.yaml` 
custom from `HighOverallControlPlaneMemory` change from `60` to `70`

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: highmemmaster-overall-custom
  namespace: openshift-monitoring
spec:
  groups:
  - name: userdefine
    rules:
    - alert: HighMemOverallMasterOver70
      labels:
        severity: warning
      expr: (1 - sum(node_memory_MemFree_bytes + node_memory_Buffers_bytes + node_memory_Cached_bytes and on(instance) label_replace(kube_node_role{role="master"}, "instance", "$1", "node", "(.+)")) / sum(node_memory_MemTotal_bytes and on(instance) label_replace(kube_node_role{role="master"}, "instance", "$1", "node", "(.+)"))) * 100 > 70
```

```bash
oc apply -f custom-highmem.yaml
```

![Untitled](TCRB%20custom%20alert%20rule%209d9bec006d52495199cefd2da0825296/Untitled.png)

### **Silence default alert**

![Untitled](TCRB%20custom%20alert%20rule%209d9bec006d52495199cefd2da0825296/Untitled%201.png)

![Untitled](TCRB%20custom%20alert%20rule%209d9bec006d52495199cefd2da0825296/Untitled%202.png)