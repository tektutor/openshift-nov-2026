# Day 5

## Info - Rolling Update
<pre>
- Rolling update is a Kubernetes feature that also works in Openshift
- It helps us upgrade our already deployed application from one version to other without any downtime
- If required we can even rollback to previous stable version on need basis
</pre>

## Lab - Rolling update
Let's deploy nginx version 1.25 initially
```
oc delete project jegan1
oc new-project jegan1

oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.25 --replicas=3 --dry-run=client -o yaml
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.25 --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml
oc apply -f nginx-deploy.yml

oc expose deploy/nginx --port=8080 --dry-run=client -o yaml
oc expose deploy/nginx --port=8080 --dry-run=client -o yaml > nginx-svc.yml
oc apply -f nginx-svc.yml

oc expose svc/nginx --dry-run=client -o yaml 
oc expose svc/nginx --dry-run=client -o yaml > nginx-route.yml
oc apply -f nginx-route.yml

oc get pods -o yaml | grep image
```

Let's upgrade the nginx from v1.25 to v1.26 using rolling update
```
cat nginx-deploy | grep image
sed -i "s/1\.25/1\.26/g" nginx-deploy.yml
cat nginx-deploy | grep image

oc apply -f nginx-deploy.yml
oc get rs
oc get pods
oc get pods -o yaml | grep image

oc rollout status deploy/nginx
oc rollout history deploy/nginx
oc rollout undo deploy/nginx
oc rollout status deploy/nginx

oc get pods -o yaml | grep image
```

## Info - External Secrets Operator (ESO)
<pre>
- In order to extract confidential data from Vault to Openshift secrets, Red Hat
  Openshift cluster must have ESO Operator installed by your Openshift Administrators
- It also must be configured, only then the ESO control will be able to convert
  Vault data into an Openshift secret
- We also have to install HashiCorp Vault ( may be an opensource ) to store the credentials
  or any confidential data, that can be extracted into an Openshift secrets
</pre>

## Demo - Configuring the ESO Operator cluster wide

externalsecretsconfig.yaml
<pre>
apiVersion: operator.openshift.io/v1alpha1
kind: ExternalSecretsConfig
metadata:
  name: cluster
  labels:
    app: external-secrets-operator
    app.kubernetes.io/name: cluster
spec:
  controllerConfig:
    networkPolicies:
      - name: allow-egress-to-vault
        componentName: ExternalSecretsCoreController
        egress:
          - ports:
              - port: 8200
                protocol: TCP
</pre>

```
oc create -f externalsecretsconfig.yaml
oc get pods -n external-secrets
oc api-resources | grep -i externalsecrets
```

Install vault
```
helm repo add hashicorp https://helm.releases.hashicorp.com
helm repo update

helm install vault hashicorp/vault \
  --namespace vault --create-namespace \
  --set global.openshift=true \
  --set server.dev.enabled=true \
  --set server.route.enabled=true \
  --set server.route.host="" \
  --set server.image.repository=public.ecr.aws/hashicorp/vault \
  --set injector.enabled=false

# Check it
oc get pods -n vault -w
oc get pods -n vault          # vault-0 should be Running 1/1
oc get svc -n vault           # service "vault" on port 8200
oc get route -n vault         # UI URL

# Fix our route
oc patch route vault -n vault --type=merge \
  -p '{"spec":{"tls":{"termination":"edge","insecureEdgeTerminationPolicy":"Redirect"}}}'

# There should be service in this pattern
http://vault.vault.svc:8200
The format is <service>.<namespace>.svc:<port>

oc exec -n vault vault-0 -- sh -c '
export VAULT_TOKEN=root

vault kv put secret/shop/db username=shopuser password="S3cr3t!2026"

vault policy write shop-read - <<EOF
path "secret/data/shop/*" {
  capabilities = ["read"]
}
EOF

vault auth enable -path=ocp kubernetes

vault write auth/ocp/config \
    kubernetes_host="https://kubernetes.default.svc:443"

vault write auth/ocp/role/shop-app \
    bound_service_account_names=shop-eso \
    bound_service_account_namespaces=shop \
    audience=vault \
    policies=shop-read \
    ttl=20m
'
```

## Info - Ingress Overview
<pre>
- It is not a service
- It is a set of forwarding rules
</pre>

## Info - Ingress vs Service
<pre>
- Behind an Ingress, there will be multiple services as backend
- each service behind ingress can point to group of pods of totally different types of applications
- Behind a Service, there will be multiple pods as backend, every Pod belongs to the same deployment
</pre>

## Info - Ingress vs Route
<pre>
- Ingress provides a user-friendly url just like Route
- Route is an Openshift feature, developed on top of Kubernetes Ingress
- Behind a Route, normally there will a single Service ( i.e ClusterIP, NodePort or LoadBalancer Service )
- Behind an Ingress, normally there can be any number of Services which points to pods from different deployment
</pre>
