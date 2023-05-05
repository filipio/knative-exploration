## Setup:

- `kubectl apply -f https://github.com/knative/operator/releases/download/knative-v1.10.0/operator.yaml`
- `kubectl get deployment knative-operator`
- `kubectl apply -f knative-serving-cr.yml`
- `kubectl get deployment -n knative-serving`
- `kubectl get KnativeServing knative-serving -n knative-serving`
- `kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.10.0/serving-default-domain.yaml`
- `kubectl apply -f knative-eventing-cr.yaml`
- `kubectl get deployment -n knative-eventing`
- `kubectl get KnativeEventing knative-eventing -n knative-eventing`
- `kubectl apply -f hello-world.yml`

## Run:

Run  
`kubectl get ksvc`  
and follow the url (for now it is unknown where to see it in EKS console)

## Clean up:

- `kubectl delete KnativeServing knative-serving -n knative-serving`
- `kubectl delete KnativeEventing knative-eventing -n knative-eventing`
- `kubectl delete -f https://github.com/knative/operator/releases/download/knative-v1.10.0/operator.yaml`

probably deleting resources via terraform should be enough, above executions  
are a "cleaner" way
