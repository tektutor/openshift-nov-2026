# Day 5

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
oc get pods -n vault          # vault-0 should be Running 1/1
oc get svc -n vault           # service "vault" on port 8200
oc get route -n vault         # UI URL

# There should be service in this pattern
http://vault.vault.svc:8200
The format is <service>.<namespace>.svc:<port>

oc exec -it vault-0 -n vault -- sh
export VAULT_TOKEN=root

vault auth enable -path=ocp kubernetes

vault write auth/ocp/config \
    kubernetes_host="https://kubernetes.default.svc:443"

vault write auth/ocp/role/shop-app \
    bound_service_account_names=shop-eso \
    bound_service_account_namespaces=shop \
    audience=vault \
    policies=shop-read \
    ttl=20m



```

