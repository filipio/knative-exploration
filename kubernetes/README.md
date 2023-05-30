# Knative on kubernetes cluster

## Setup

Setup is using knative-operator, based upon [knative-operator-deployment](https://knative.dev/docs/install/operator/knative-with-operators/)

1.  Install knative operator  
    `kubectl apply -f https://github.com/knative/operator/releases/download/knative-v1.10.0/operator.yaml`  
    Check if operator is ready  
     `kubectl get deployment knative-operator`

2.  Install knative-serving  
    `kubectl apply -f knative-serving-cr.yml`  
    Wait for all knative-serving deployments to be ready  
     `kubectl get deployment -n knative-serving --watch`  
    Check the status of Knative Serving Custom Resource  
    `kubectl get KnativeServing knative-serving -n knative-serving`

3.  Configure DNS using [sslip](https://sslip.io/)  
    `kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.10.0/serving-default-domain.yaml`

4.  Install knative-eventing  
     `kubectl apply -f knative-eventing-cr.yml`  
     Wait for all knative-eventing deployments to be ready  
     `kubectl get deployment -n knative-eventing --watch`  
     Check the status of Knative Eventing Custom Resource  
    `kubectl get KnativeEventing knative-eventing -n knative-eventing`

5.  Modify `image` value in `hello-world.yml` if you want to use knative function created from scratch using instructions in [../src/hello-python/README.md](../src/hello-python/README.md). Image pattern for dockerhub is  
    `docker.io/<dockerhub-username>/hello-python`

6.  Create hello-world app function:  
    `kubectl apply -f hello-world.yml`

## Access knative service

Execute  
`kubectl get ksvc`  
and use URL from output to access endpoints. Following methods are available:  
`GET <URL>` (with optional query parameters)  
`POST <URL>` (with optional json body)  
`GET <URL>/health/readiness`  
`GET <URL>/health/liveness`  
If you are using vscode, install `rest client` plugin and execute requests available in `test.http`. If not, check mentioned file to see how to execute requests with other client (curl, postman, ...)

## Sebs Benchmark

1. Install channel implementation for default knative broker  
   `kubectl apply -f https://github.com/knative/eventing/releases/download/knative-v1.10.0/in-memory-channel.yaml`

2. Create channek broker  
   `kubectl apply -f channel-broker.yml`

3. Create knative service for specific sebs benchmark. Convention for file naming is `benchmark-{type}-svc.yml`, e.g. type=pagerank  
   `kubectl apply -f benchmark-pagerank-svc.yml`

4. Check if benchmark service is ready  
   `kubectl get ksvc`

5. Create benchmark trigger to route cloud-events to knative service. Name convention is `benchmark-{type}-tr.yml`  
   `kubectl apply -f benchmark-pagerank-tr.yml`

6. Check if trigger is ready  
   `kubectl get triggers`

7. Create pod to execute curl requests to broker  
   `kubectl apply -f curler.yml`

8. Connect to pod via ssh  
   `kubectl exec -it curler -- bin/bash`

9. In another terminal, get URL of created broker  
   `kubectl get broker my-channel-broker -o jsonpath='{.status.address.url}'`

10. Exec curl request to start benchmark  
    `curl -v <broker-url> -X POST -H "Ce-Id: 1234" -H "Ce-Specversion: 1.0" -H "Ce-Source: curl" -H "Content-Type: application/json" -H "Ce-Type: graph-pagerank" -d '{"size":"large"}'`  
    e.g  
    `curl -v http://broker-ingress.knative-eventing.svc.cluster.local/default/my-channel-broker -X POST -H "Ce-Id: 1234" -H "Ce-Specversion: 1.0" -H "Ce-Source: curl" -H "Content-Type: application/json" -H "Ce-Type: graph-pagerank" -d '{"size":"large", "debug": true}'`  
    Important: value of `Ce-Type` header needs to match the one defined in [quarkus-benchmarks](https://github.com/IBM/knative-quarkus-bench/tree/main/benchmarks)

11. In another terminal, get logs of pod executing the task  
    `kubectl logs -f -l=kubectl logs -f -l app=benchmark-pagerank-00001`  
    NOTE: It needs to be done immediately after executing curl request. This is because after some timeout knative will destroy running pod.  
    For the `pagerank` benchmark the last line displays the compute_time

## Sebs Benchmark cleanup

1. `kubectl delete pod/curler`

2. `kubectl delete triggers --all`

3. `kubectl delete ksvc --all`

4. `kubectl delete broker --all`

5. `kubectl delete -f https://github.com/knative/eventing/releases/download/knative-v1.10.0/in-memory-channel.yaml`

## Clean up

1. `kubectl delete KnativeServing knative-serving -n knative-serving`

2. `kubectl delete KnativeEventing knative-eventing -n knative-eventing`

3. `kubectl delete -f https://github.com/knative/operator/releases/download/knative-v1.10.0/operator.yaml`
