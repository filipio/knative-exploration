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

5. Follow instructions in [kubernetes/README.md](./kubernetes/README.md) to setup knative in EKS cluster and run hello-world app

## Cleanup

1. Destory EKS cluster with knative resources (execute in `./terraform` directory)  
   `terraform destroy`  
   NOTE : you need to wait for completion

2. Destroy learner lab
