# Playing with TAP-SANDBOX

Go to the https://portal.tapsandbox.com/ portal and ask for a new session

## Prerequisites

Register and create uses: only vmware.com domain accounta are enabled

## Settign up local environment

Clean your local configuration befor setting up 

```bash
rm ~/Download/tenant_kubernetes*

kubectl config unset clusters.tap-sandbox && kubectl config unset users.tap-sandbox && kubectl config unset contexts.tap-sandbox

```

Download tenant_kubernetes.yaml file and run command as explained in the session:

```bash
export KUBECONFIG_DOWNLOAD_LOCATION=~/Downloads/tenant_kubeconfig.yml #set to download location

cp ~/.kube/config ~/.kube/config.bak && KUBECONFIG=~/.kube/config:$KUBECONFIG_DOWNLOAD_LOCATION kubectl config view --flatten > /tmp/config && mv /tmp/config ~/.kube/config && kubectl config set current-context tap-sandbox
```

Test connectivity

```bash
kubectl get node
```

## Get current tap-value.yaml file from your session

You may want to recreate the tap-value.yaml file for future configuration updates

```bash
kubectl get secret -n tap-install tap-tap-install-values -o json | jq '.data["values.yaml"]' -r | base64 -d > tap-value.yaml
```

## Accessing TAP GUI

As explained in your session you can access the TAP GUI page at `https://tap-gui.tap-stirred-mongoose.tapsandbox.com` by a web browser.


## Create you developer namespace

Run the following kubectl commands

```bash
kubectl create namespace cpu
kubectl label namespaces cpu apps.tanzu.vmware.com/tap-ns=""
kubectl get secrets,serviceaccount,rolebinding,pods,workload,configmap -n cpu

NAME                            TYPE                             DATA   AGE
secret/registries-credentials   kubernetes.io/dockerconfigjson   1      1s

NAME                     SECRETS   AGE
serviceaccount/default   1         2s

NAME                                                               ROLE                      AGE
rolebinding.rbac.authorization.k8s.io/default-permit-deliverable   ClusterRole/deliverable   1s
rolebinding.rbac.authorization.k8s.io/default-permit-workload      ClusterRole/workload      1s

NAME                         DATA   AGE
configmap/kube-root-ca.crt   1      2s

```

Check the output from the last command to be sure all permissions stuff are set




