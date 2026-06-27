参考文档 https://docs.aws.amazon.com/zh_cn/eks/latest/best-practices/ipvs.html#

1、可以通过更新 E kube-proxy KS 插件来发出 AWS CLI 命令来启用 IPVS。
aws eks update-addon --cluster-name sg-eks --addon-name kube-proxy \
  --configuration-values '{"ipvs": {"scheduler": "rr"}, "mode": "ipvs"}' \
  --resolve-conflicts OVERWRITE


2、如果您的工作节点在进行这些更改之前已加入集群，则需要重新启动 kube DaemonSet-proxy。


kubectl -n kube-system rollout restart ds kube-proxy

3、验证
[cloudshell-user@ip-10-135-94-160 ~]$ kubectl -n kube-system get cm kube-proxy-config -o yaml | grep mode
    mode: "ipvs"
[cloudshell-user@ip-10-135-94-160 ~]$ 