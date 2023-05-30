# Knative on EKS

This project uses EKS created by terraform to setup knative in kubernetes cluster and run a sample knative hello-world function written in python.

## Requirements

- [terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli), verify by `terraform --version`
- [docker](https://docs.docker.com/engine/install/), verify by `docker --version`
- [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl), verify by `kubectl version`
- [aws cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#getting-started-install-instructions), verify by `aws --version`
- [knative cli](https://knative.dev/docs/getting-started/quickstart-install/#install-the-knative-cli), verify by `kn version`
- [knative cli func plugin](https://knative.dev/docs/getting-started/install-func/#installing-the-kn-func-cli-plugin), verify by `kn func version`
- [dockerhub free account](https://hub.docker.com/) (to store knative function images in repositories)
- [amazon aws learner lab](https://aws.amazon.com/training/awsacademy/) - terraform will work only with learner lab

## Setup

1. Create learner lab

2. Paste learner lab credientials into `~/.aws/credentials` (this needs to be done each time new lab session is started)

3. Follow instructions in [terraform/README.md](./terraform/README.md) to setup EKS with sample node group

4. Follow instructions in [src/hello-python/README.md](./src/hello-python/README.md) to build and push knative function image to dockerhub repository

5. Follow instructions in [kubernetes/README.md](./kubernetes/README.md) to setup knative in EKS cluster

6. Follow instructions in [kubernetes/README.md](./kubernetes/README.md) to either execute request on hello-world knative function or run [sebs-pagerank](https://github.com/spcl/serverless-benchmarks/tree/master/benchmarks/500.scientific/501.graph-pagerank) benchmark using [quarkus](https://github.com/IBM/knative-quarkus-bench)

## Clean up

1. Destory created knative resources. Follow instructions in [kubernetes/README.md](./kubernetes/README.md) (`Clean up` section)

2. Destory EKS cluster and its resources. Follow instructions in [terraform/README.md](./terraform/README.md) (`Clean up` section)
   You need to wait for completion

3. Destroy learner lab

NOTE: You need to follow point 1) before 2). Otherwise some resources may not be deleted properly (e.g. EC2 LoadBalancers)
