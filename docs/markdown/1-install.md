## Docker/Podman/Mac/Linux/WSL/Vagrant/

## Prerequisites

Access to `Spring Enterprise` repository

```bash
mvn dependency:get -Dartifact=com.vmware.tanzu.spring:application-advisor-cli-macos-arm64:1.1.2 -Dpackaging=tar
mvn dependency:get -Dartifact=com.vmware.tanzu.spring:application-advisor-server:1.1.2 -Dpackaging=jar
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