# eks-autoscaling-app-cluster
eks autoscaling application and cluster
install helm package 
search the helm repo in stable package
$ helm search repo kube-ops-view
$ helm install kube-ops-view stable/kube-ops-view --set service.type=LoadBalancer --set rbac.create=True
$helm list
4kubectl get svc kube-ops-view | tail -n 1 | awk '{ print "Kube-ops-view URL = http://"$4 }'
or
kubectl get svc -A (you will get the lb end point you check with it in browser)
