# Monitor Kubernetes Cluster using Prometheus & Grafana

## Aim :
To install prometheus & grafana to our newly created kubernetes cluster. After istallation we have to monitor our cluster and pods by dashboard.

## Prerequisite :
- [Minikube](https://minikube.sigs.k8s.io/docs/start/), [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/), [Helm](https://helm.sh/docs/intro/install/) installed in our local laptop/desktop.
- Create a cluster and deploy [nginx](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) web server.

## Step 1. Install Prometheus and Grafana
- Before installing Prometheus let's check how many pods and services are running
```
kubectl get all
```
![PodsBeforeInstallation](https://drive.google.com/uc?export=view&id=1UUhBRNtyXPVTPvOZBvc7AJSkG7I7j4u-)
- Now add a repo using helm
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
![AddRepo](https://drive.google.com/uc?export=view&id=1l_8hR1aU1wG46HJVe85TQJT5EzVHWjVI)
- After adding a repo update it using this command
```
helm repo update
```
![RepoUpdate](https://drive.google.com/uc?export=view&id=1HKk2NRxoKQxoR5W5fyX3XWEZXpcKvOF1)
- Search for repo prometheus-community
```
helm search repo prometheus-community
```
![SearchRepo](https://drive.google.com/uc?export=view&id=1sP3aM4j3k9ZCQPd38q0g-QiXkOHMCUbH)
Here we have to install ```kube-prometheus-stack``` from ```prometheus-community``` repo which is the source for prometheus.

![kube-prometheus-repo](https://drive.google.com/uc?export=view&id=1Iz0qmIg0kXVCsdfkRlwqRyA_tbzhY1Nu)

- Install Prometheus using this command. 
```
helm install prometheus prometheus-community/kube-prometheus-stack
```
![InstallPrometheus](https://drive.google.com/uc?export=view&id=1jUTFUuzDNOKfB7lYVDt4g1Zqg-1YsVUE)
Grafana also installed within this package.

- Installed and check pods running based on prometheus using this command.
```
kubectl --namespace default get pods -l "release=prometheus"
```
![PrometheusBasedPods](https://drive.google.com/uc?export=view&id=1HEfkVg1auvCXaFb7wsfPFkpvH0fwZdx7)
- Now Check every pods running in our cluster
```
kubectl get pods
```
![PodsAfterInstallation](https://drive.google.com/uc?export=view&id=1UDn7scepY0PhOTtMQlLaacOgiIHEx4Dt)

It also installed Grafana through ```kube-prometheus-stack```

## Step 2. Prometheus Dashboard
- Prometheus's default port is 9090. Let's port forward to 8000 to view the dashboad.
```
kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 8000:9090
```
![PrometheusPortForward](https://drive.google.com/uc?export=view&id=1AfHIl_p_Z8MsaRn_bCF9jOn48Fjc7G68)

- I'm doing this on local so i'll visit ```localhost:8000``` or ```127.0.0.1:8000```. Now we can view our prometheus dashboard.

![PrometheusDashboard](https://drive.google.com/uc?export=view&id=1JwUXCVcumHE1uPmoYXE6oUlkWRE6-UMy)

- Go to Status-> target to view all target details of our cluster.

![Target](https://drive.google.com/uc?export=view&id=1qooSepEQ6yMthAqkHf9ZaLXdKEDkmGd-)

![ClusterDetails](https://drive.google.com/uc?export=view&id=13NwgawetKoozKFRYzMJCLpEuoQp0_dcx)

## Step 3. Grafana Dashboard
- To view grafana dashboard we have to port forward it. Default port number is 3000. I'll forward it to 8000 using this command
```
kubectl port-forward prometheus-grafana-7995c49697-mwd2c 8000:3000
```
![GrafanaPortForward](https://drive.google.com/uc?export=view&id=1wHW9QaigjNHihRKfeawrbQ-iNFVc4jwm)

- Check the login dashboard on ```localhost:3000``` or ```127.0.0.1:3000```

![GrafanaDashboard](https://drive.google.com/uc?export=view&id=1J6RLOld38hVBhESEyV1cQPnlQpkfFgTT)

- Default Username & Password of Grafana is ```admin``` & ```prom-operator```

- This is the dashboard after login. Go to ```Home->Connections->Data Sources```

![DataSources](https://drive.google.com/uc?export=view&id=1pqAs62uFA5ZBNpkplAji4ThIHMBMjSbv)

It automatically detects our prometheus data source and displays it as default. Select and view default prometheus data source details under this section.

![SelectDataSources](https://drive.google.com/uc?export=view&id=1v3Eq1f-l-z9Wn8EEMPzDuXIQ07xvUTu0)

- Let's import a import and view our data and monitor it. I used this dashboard for cluster monitoring.
```
https://grafana.com/grafana/dashboards/10000-kubernetes-cluster-monitoring-via-prometheus/
```
- To import this dashboard, go to Home->Dashboard->Import . Provide the Id number ```10000``` load it, then select data source ```Prometheus``` and click import.

![Dash_ID](https://drive.google.com/uc?export=view&id=13kgaVt06-JDE_r6dmnTP5xiL2zdOeL79)

![SelectDS](https://drive.google.com/uc?export=view&id=1I5PvnYnBZSDMM9ncSA_TCP_7M40cytl-)

- Let's view our cluster monitoring dashboard. We can star it for our later use easily.

![ClusterMonitorDashboard](https://drive.google.com/uc?export=view&id=1taA-NDZUzTQnOWPvUUIZbeboPfjr_sUU)

- Now let's import a dashboard to view/monitor pods. I used this dashboard for Pod monitoring,
```
https://grafana.com/grafana/dashboards/6781-kubernetes-pod-overview/
```
Provide the Id number ```6781``` and select data source ```Prometheus``` and click import.

![PodDashImport](https://drive.google.com/uc?export=view&id=1mF04kfLJJtZSdY_TQgXHQ2PCVsaTyDeH)

- Let's view our pod monitoring dashboard. We can star it for our later use easily. We can select any namespace, any pod and monitor it easily.

![PodMonitorDashboard](https://drive.google.com/uc?export=view&id=1sI-ehqauA5-jb_TD_BjOOmrIYHymvTYm)

- We can check our previously imported dashboards under this section.

![Dashboards](https://drive.google.com/uc?export=view&id=1dbqNBA7kiW6ov7OYSsXIZTzEQnSZOVnx)


