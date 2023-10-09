## Follow along:

- **Install the Keptn Lifecycle Toolkit**

    ```
    helm repo add klt https://charts.lifecycle.keptn.sh
    helm repo update
    helm upgrade --install keptn klt/klt -n keptn-lifecycle-toolkit-system --create-namespace --wait
    ```
    or if you wish to install using YAML manifests
    ```
    kubectl apply -f https://github.com/keptn/lifecycle-toolkit/releases/download/v0.7.1/manifest.yaml
    ```

- **Setup Prometheus on your cluster**
    For this demo, we'll be using prometheus to monitor our application, you can use any provider of your choice if you wish too.
    ```
    kubectl create namespace monitoring
    kubectl apply --server-side -f config/prometheus/setup
    kubectl apply -f config/prometheus/
    ```

- **Deploy the Demo application**  
    This would deploy a `Deployment` and a `Service` on you Cluster.
    ```
    kubectl create ns podtato-kubectl
    kubectl apply -f deployment.yaml
    ```
    Wait for the workload to be ready, you can see the status using `kubectl get all -n podtato-kubectl`

- **Deploy the `KeptnMetricsProivder`**
    ```
    kubectl apply -f metricprovider.yaml
    ```

- **Deploy the `KeptnMetric`**
    ```
    kubectl apply -f keptnmetric.yaml
    ```
- Now you can check the metric value for you application using
    ```
    kubectl get KeptnMetric -n podtato-kubectl
    ```
  Check the no. of pods running for your application using `kubectl get pods -n podtato-kubectl `, should be 1.

- **Apply `HorizontalPodAutoscaler`**
    ```
    kubectl apply -f hpa.yaml
    ```

- Check for the no. of pods for your application again, should be 3 now, if no, wait for some time, the pods will start getting created in some time.