---
date:
  created: 2025-11-23
draft: true
---

# Live Server Rack Diagrams

## Demo
screenshot/gif of the rack diagram here

## Why?
* Recently needed to label/verify hardware in a server rack and wanted a "live" server rack diagram to be able to tell what systems were up/down.
* Inspired by this post: https://www.reddit.com/r/sysadmin/comments/13ox4rr/what_do_you_use_for_live_rack_diagrams/
* K8s based, GitOps support, and FOSS solution.

## Step-by-step Instructions
### Install Kubernetes Cluster
There are many options for a development Kubernetes cluster, a good overview is [here](https://docs.tilt.dev/choosing_clusters.html). For this project, I used [Rancher Desktop](https://rancherdesktop.io/) on macOS.

### Install Prometheus and Grafana
Install [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) by first adding the helm repo
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
Then install the stack to setup prometheus and grafana in the `monitoring` namespace
```
helm install prometheus-stack prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
```
Get Grafana 'admin' user password by running
```
kubectl --namespace monitoring get secrets prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo
```
Access Grafana local instance:
```
export POD_NAME=$(kubectl --namespace monitoring get pod -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=prometheus-stack" -oname)
kubectl --namespace monitoring port-forward $POD_NAME 3000
```

### Configure Prometheus and Grafana
* Blackbox exporter
* Grafana plugin
* Prometheus list of sites to ping test

### Create dashboard
* Dashboard file
* Draw.io svg tags
* Panelconfig file

### GitOps
* Example repo
* Point to it w/Argo?

## Sources
* [https://www.reddit.com/r/sysadmin/comments/13ox4rr/what_do_you_use_for_live_rack_diagrams/](https://www.reddit.com/r/sysadmin/comments/13ox4rr/what_do_you_use_for_live_rack_diagrams/)
* [https://grafana.com/grafana/plugins/andrewbmchugh-flow-panel/](https://grafana.com/grafana/plugins/andrewbmchugh-flow-panel/)
* [https://github.com/andymchugh/andrewbmchugh-flow-panel](https://github.com/andymchugh/andrewbmchugh-flow-panel)
* [https://medium.com/@platform.engineers/setting-up-a-prometheus-and-grafana-monitoring-stack-from-scratch-63667bf3e011](https://medium.com/@platform.engineers/setting-up-a-prometheus-and-grafana-monitoring-stack-from-scratch-63667bf3e011)
