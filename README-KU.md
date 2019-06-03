# Setup 

## Repo

<https://github.com/openshift/origin>

## Cluster can not be started

Error:

~~~
I0603 08:34:12.194670   10548 interface.go:41] Finished installing "kube-proxy" "kube-dns" "openshift-service-cert-signer-operator" "openshift-apiserver"

Error: timed out waiting for the condition
~~~

### Solution

<https://github.com/openshift/origin/issues/21420>

~~~
firewall-cmd --permanent --new-zone dockerc
firewall-cmd --permanent --zone dockerc --add-source 172.17.0.0/16
firewall-cmd --permanent --zone dockerc --add-port 8443/tcp
firewall-cmd --permanent --zone dockerc --add-port 53/udp
firewall-cmd --permanent --zone dockerc --add-port 8053/udp
firewall-cmd --reload

mkdir -p "$HOME/.occluster"
oc cluster up --base-dir="$HOME/.occluster"
~~~

## Connect helm

<https://blog.openshift.com/getting-started-helm-openshift/>

~~~
oc new-project tiller
oc project tiller
export TILLER_NAMESPACE=tiller
curl -s https://storage.googleapis.com/kubernetes-helm/helm-v2.9.0-linux-amd64.tar.gz | tar xz
cd linux-amd64
./helm init --client-only

oc process -f https://github.com/openshift/origin/raw/master/examples/helm/tiller-template.yaml -p TILLER_NAMESPACE="${TILLER_NAMESPACE}" -p HELM_VERSION=v2.9.0 | oc create -f -

oc rollout status deployment tiller

./helm version

oc new-project myapp
oc policy add-role-to-user edit "system:serviceaccount:${TILLER_NAMESPACE}:tiller"
./helm install https://github.com/jim-minter/nodejs-ex/raw/helm/helm/nodejs-0.1.tgz -n nodejs-ex
~~~ 

## Docker registry

1. Get token via admin console
2. Login to registry:

    docker login  -p nh8kSncc83Dp2zaCcdzawniE3PVbACGS1ZepHrbcVAA 172.30.1.1:5000

## Docker registry

1. Get token via admin console
2. Login to registry:

    docker login  -p nh8kSncc83Dp2zaCcdzawniE3PVbACGS1ZepHrbcVAA 172.30.1.1:5000

## Docker registry

1. Get token via admin console
2. Login to registry:

    docker login  -p nh8kSncc83Dp2zaCcdzawniE3PVbACGS1ZepHrbcVAA 172.30.1.1:5000

## Docker registry

1. Get token via admin console

2. Login to registry:

    docker login  -p nh8kSncc83Dp2zaCcdzawniE3PVbACGS1ZepHrbcVAA 172.30.1.1:5000

3. Tag image

    docker tag ubuntu 172.30.1.1:5000/myproject/ubuntu

4. Push image

    docker push 172.30.1.1:5000/myproject/ubuntu

## DBAMC 18.0.2

<https://github.ibm.com/klaus-ulrich/cert-kubernetes>

~~~
root@ku17rhel76 ~/github/cert-kubernetes
scripts/loadimages.sh -p ~/DBAMC-18.0.2.tgz -r 172.30.1.1:5000/myproject
~~~
scripts/loadimages.sh -p /Downloads/PPA/ImageArchive.tgz -r docker-registry.default.svc:5000/demo-project
