# Day 3

## Lab - Declaratively deploying application into Openshift
```
# Delete your existing project
oc delete project jegan

# Create a new project
oc new-project jegan

# Auto-generate declarative code to deploy nginx declaratively
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.25 --replicas=3 --dry-run=client -o yaml

# Redirect the yaml output to a yaml file
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.25 --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml

cat nginx-deploy.yml

# Declaratively deploy the nginx into Openshift
oc create -f nginx-deploy.yml --save-config=true

# List and check
oc get deploy,rs,po

```

## Lab - Declaratively create a ClusterIP Internal Service
```
oc project jegan
oc get deploy

# Declaratively create ClusterIP Internal service
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml > nginx-clusterip-svc.yml

# Create internal service declaratively
oc create -f nginx-clusterip-svc.yml --save-config=true

oc get svc
oc describe svc/nginx
```

## Lab - Using Openshift Webconsole
```
cat ~/openshift.txt

oc whoami --show-console


# Open the below url in firefox browser on your Ubuntu Cloud machine
https://console-openshift-console.apps.ocp4.palmeto.org
```
