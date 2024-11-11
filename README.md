
# S3-sync kubernetes cron-job
Automate fetching data from an S3 bucket across accounts and store it in the EFS drive of an EKS cluster, while also handling deletions.

Please refer the article for the explanation : [Simplifying Cross-Account S3 Access for Your EKS Cluster
](https://dilshanw.medium.com/simplifying-cross-account-s3-access-for-your-eks-cluster-b6d77b37321b) and [Automating Cross-Account S3 Data Sync for EKS With Helm
](https://dilshanw.medium.com/automating-cross-account-s3-data-sync-for-eks-with-helm-ba5d2c3a7111)

## Pre-requirements: Kubernetes Service Account
```
kubectl apply -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
name: s3-sync-sa
namespace: <your_namespace>
annotations:
eks.amazonaws.com/role-arn: arn:aws:iam::<account_ID>:role/my-cross-account-s3-sync-role
EOF
```

## Deployment - s3-sync

```
helm upgrade --install s3-sync s3-sync \
--namespace <your_namespace> \
-f s3-sync/values.yaml
```

## Run Manually

```
kubectl create job --from=cronjob/s3-sync s3-sync-<random_id>
```
