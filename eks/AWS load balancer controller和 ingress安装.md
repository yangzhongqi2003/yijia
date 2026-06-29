
第一步：关联 IAM OIDC 提供商
eksctl utils associate-iam-oidc-provider --region=ap-southeast-1 --cluster=php-eks --approve

Run in CloudShell
第二步：创建 IAM 服务账户
等第一步完成后，再执行：
eksctl create iamserviceaccount \
  --cluster=php-eks \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::aws:policy/ElasticLoadBalancingFullAccess \
  --approve \
  --region=ap-southeast-1

第三步：安装 AWS Load Balancer Controller
# 添加 EKS Helm 仓库
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

helm repo add eks https://aws.github.io/eks-charts
helm repo update

# 获取 VPC ID
VPC_ID=$(aws eks describe-cluster --name php-eks --region ap-southeast-1 --query "cluster.resourcesVpcConfig.vpcId" --output text)

# 安装 AWS Load Balancer Controller
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=php-eks \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=ap-southeast-1 \
  --set vpcId=$VPC_ID

Run in CloudShell
第四步：验证安装
# 检查部署状态
kubectl get deployment -n kube-system aws-load-balancer-controller

# 检查 Pod 状态
kubectl get pods -n kube-system -l app.kubernetes.io/name=aws-load-balancer-controller

# 检查服务账户
kubectl get sa aws-load-balancer-controller -n kube-system -o yaml

Run in CloudShell
第五步：检查您的 LoadBalancer 服务
kubectl get svc -n ingress-nginx-public ingress-nginx-public-controller

Run in CloudShell
执行完这些步骤后，您的 LoadBalancer 服务应该会从 <pending> 状态变为显示实际的外部 IP 地址。整个过程可能需要几分钟时间来创建 AWS Network Load Balancer。

如果在任何步骤中遇到问题，请告诉我具体的错误信息，我会帮您进一步排查


ingress安装


helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

kubectl create namespace ingress-nginx-public

helm install ingress-nginx-public ingress-nginx/ingress-nginx \
  -n ingress-nginx-public \
  -f values-public.yaml



helm upgrade ingress-nginx-public ingress-nginx/ingress-nginx \
  -n ingress-nginx-public \
  -f values-public.yaml
