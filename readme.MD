# Ingress Controller == aws loadbalancer controller
Ref: https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/  --> examples, guide, deployment tabs

```
1. eksctl utils associate-iam-oidc-provider \
    --region <region-code> \
    --cluster <your-cluster-name> \
    --approve

    eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster expense \
    --approve

```

```
2. curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v3.0.0/docs/install/iam_policy.json
```

```
3. aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json
```

```
4. eksctl create iamserviceaccount \
--cluster=<cluster-name> \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::<AWS_ACCOUNT_ID>:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region <region-code> \
--approve

eksctl create iamserviceaccount \
--cluster=expense \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::529088275803:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region us-east-1 \
--approve
```

```
5. helm repo add eks https://aws.github.io/eks-charts
```

```
6. helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=<cluster-name> --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller

helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=expense --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
```

# Ingress resource
1. use annotations
2. use ingress rules
Ref: https://kubernetes.io/docs/concepts/services-networking/ingress/

# Annotations
Annotations are used to select exernal resources. there is no limit like special characters, length.
Ref: https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/ingress/annotations/#group.name

flow
![alt text](k8-ingress.svg)