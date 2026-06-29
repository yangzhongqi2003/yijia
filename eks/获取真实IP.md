 kubectl get cm -n ingress-nginx 
NAME                       DATA   AGE
ingress-nginx-controller   7      146d
kube-root-ca.crt           1      146d
~ $ kubectl get cm ingress-nginx-controller -n ingress-nginx -oyaml
apiVersion: v1
data:
  compute-full-forwarded-for: "true"
  enable-underscores-in-headers: "true"
  error-log-level: warn
  forwarded-for-header: X-Forwarded-For
  generate-request-id: "true"
  log-format-upstream: |
    [$time_iso8601] "$request" $status $request_time $upstream_response_time $remote_addr $host $request_id "$http_user_agent" True-Client-Ip=$remote_addr
  use-forwarded-headers: "true"
kind: ConfigMap
metadata:
  annotations:
    meta.helm.sh/release-name: ingress-nginx
    meta.helm.sh/release-namespace: ingress-nginx
  creationTimestamp: "2025-12-23T06:18:13Z"
  labels:
    app.kubernetes.io/component: controller
    app.kubernetes.io/instance: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/version: 1.13.3
    helm.sh/chart: ingress-nginx-4.13.3
  name: ingress-nginx-controller
  namespace: ingress-nginx
  resourceVersion: "66011472"
  uid: 5582e717-c47e-4a95-82d0-48eacb525c42
~ $ 
