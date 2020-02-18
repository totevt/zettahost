Tested in Kubernetes v1.15.6 and v1.17.2

D E P L O Y M E N T

$ git clone https://github.com/totevt/zettahost.git
# /mnt/www folder should be storage shared between kubernetes cluster nodes (S3/blob/nfs/...)

$ kubectl apply -f ~/zettahost/manifests/www-namespace.yaml -f ~/zettahost/manifests/
# This brings up "www" namespace, "www-storage", "www-claim" PVC and a single-pod nginx Deployment.
# nginx can be scaled in several ways:
#   - Updating spec.replicas count in nginx.yaml file and the re-applying the Deployment with: kubectl apply -f ~/zettahost/manifests/nginx.yaml
#   - Rescaling the Deployment on the go: kubectl scale --replicas=4 --namespace=www deployment nginx
#   - In addition I've added automatic pod autoscaling (up to 4 pods) which is being triggerred once the pod's CPU utilisation hits 80% (of the 250 miliCPU units that are set as cap limit)
