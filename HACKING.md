```sh
# pretty much required for anything
oc scale -n openshift-cluster-version deployment/cluster-version-operator --replicas=0

# required/makes it easier if you want to edit deployments managed by this operator
oc scale -n openshift-machine-api deployment/machine-api-operator --replicas=0

# Building a project:
sudo buildah bud -t quay.io/username/machine-api-operator:v0.0.4 .
sudo buildah push quay.io/username/machine-api-operator:v0.0.4

```

Edit the machine-api-operator deployment to point at the new image.

```
$ token=`oc sa get-token prometheus-k8s -n openshift-monitoring`
$  oc -n openshift-monitoring exec -c prometheus prometheus-k8s-0 -- curl -k -H "Authorization: Bearer $token" 'https://prometheus-k8s.openshift-monitoring.svc:9091/api/v1/label/__name__/values' | jq | grep "mapi_instance_"
```
