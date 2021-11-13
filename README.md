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

DEPLOY THE METRIC SERVER
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.5.0/components.yaml
kubectl get apiservice v1beta1.metrics.k8s.io -o json | jq '.status'
kubectl create deployment php-apache --image=us.gcr.io/k8s-artifacts-prod/hpa-example
kubectl set resources deploy php-apache --requests=cpu=200m
kubectl expose deploy php-apache --port 80

kubectl get pod -l app=php-apache
HPA
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
kubectl get hpa
kubectl get hpa -w (watch)
watch -n 1 'kubectl get hpa)
==============================
CLUSTER AUTO SCALING GROUP
==============================
one auto scaling group, multiple auto scaling group, auto-discovery, control-plane node setup
AutoDiscovery: preferred method to configure auto scaling group.
cluster autoscaler will attempt to determine the CPU, memory, gpu provide by the Auto Scaling Group.
# we need the ASG name
export ASG_NAME=$(aws autoscaling describe-auto-scaling-groups --query "AutoScalingGroups[? Tags[? (Key=='eks:cluster-name') && Value=='eksworkshop-eksctl']].AutoScalingGroupName" --output text)

# increase max capacity up to 4
aws autoscaling \
    update-auto-scaling-group \
    --auto-scaling-group-name ${ASG_NAME} \
    --min-size 3 \
    --desired-capacity 3 \
    --max-size 4

# Check new values
aws autoscaling \
    describe-auto-scaling-groups \
    --query "AutoScalingGroups[? Tags[? (Key=='eks:cluster-name') && Value=='eksworkshop-eksctl']].[AutoScalingGroupName, MinSize, MaxSize,DesiredCapacity]" \
    --output table
