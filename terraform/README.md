# Terraform for EKS in learner lab

## Setup

1. Initialize terraform if running for the first time  
   `terraform init`
2. Create EKS cluster with sample node group  
   `terraform apply -auto-approve`
3. Check if `kubectl` has been successfully set up (this is done after node group has been created)  
   `kubectl get nodes`

## Clean up

1. Delete EKS cluster and related resources
   `terraform destroy -auto-approve`
