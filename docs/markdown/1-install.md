## Docker/Podman/Mac/Linux/WSL/Vagrant/

## Prerequisites

Access to `Spring Enterprise` repository

```bash
mvn dependency:get -Dartifact=com.vmware.tanzu.spring:application-advisor-cli-macos-arm64:1.1.2 -Dpackaging=tar
mvn dependency:get -Dartifact=com.vmware.tanzu.spring:application-advisor-server:1.1.2 -Dpackaging=jar
```

## Install Kubernetes

```bash
kubectl get nodes -o wide
```
> You just need a Kubernetes dial-tone

## Create Config

```text
export ROOT_NAMESPACE="cf"
export KORIFI_NAMESPACE="korifi"
export ADMIN_USERNAME="cf-admin"
export BASE_DOMAIN="apps-127-0-0-1.nip.io"
export GATEWAY_CLASS_NAME="contour"
```

## Install Local Registry

```bash
helm repo add twuni https://helm.twun.io
# the htpasswd value below is username: user, password: password encoded using `htpasswd` binary
# e.g. `docker run --entrypoint htpasswd httpd:2 -Bbn user password`
helm upgrade --install localregistry twuni/docker-registry \
--set service.type=NodePort,service.nodePort=30050,service.port=30050 \
--set persistence.enabled=true \
--set persistence.deleteEnabled=true \
--set secrets.htpasswd='user:$2y$05$Ue5dboOfmqk6Say31Sin9uVbHWTl8J1Sgq9QyAEmFQRnq1TPfP1n2'
```

## Install Cert Manager

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.16.3/cert-manager.yaml
```

## Install Kpack

```bash
kubectl apply -f https://github.com/buildpacks-community/kpack/releases/download/v0.16.1/release-0.16.1.yaml
```

## (Contingent) Install binfmt

```bash

```

## [Concourse Quick Start](https://concourse-ci.org/quick-start.html)

## [Korifi Install](https://tutorials.cloudfoundry.org/korifi/local-install/)

```bash
git clone https://github.com/cloudfoundry/korifi
cd korifi
git checkout v0.14.0
./scripts/deploy-on-kind.sh korifi
```

### Authenticate, create, and target

```bash
cf api localhost --skip-ssl-validation
cf auth cf-admin
cf create-org workshop
cf create-space -o workshop saa
cf target -o workshop -s saa
```

### Deploy Spring Application Advisor

```bash
cf push saa -p ~/.m2/repository/com/vmware/tanzu/spring/application-advisor-server/1.1.2/application-advisor-server-1.1.2.jar
```

### Undeploy

```bash
kind delete cluster -n korifi
```