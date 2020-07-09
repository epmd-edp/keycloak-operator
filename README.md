# How to Install Operator

EDP installation can be applied on two container orchestration platforms: OpenShift and Kubernetes.

_**NOTE:** Installation of operators is platform-independent, that is why there is a unified instruction for deploying._


### Prerequisites
1. Linux machine or Windows Subsystem for Linux instance with [Helm 3](https://helm.sh/docs/intro/install/) installed;
2. Cluster admin access to the cluster;
3. EDP project/namespace is deployed by following one of the instructions: [edp-install-openshift](https://github.com/epmd-edp/edp-install/blob/master/documentation/openshift_install_edp.md#edp-project) or [edp-install-kubernetes](https://github.com/epmd-edp/edp-install/blob/master/documentation/kubernetes_install_edp.md#edp-namespace).

### Installation
* Go to the [releases](https://github.com/epmd-edp/keycloak-operator/releases) page of this repository, choose a version, download an archive and unzip it;

_**NOTE:** It is highly recommended to use the latest released version._

* Copy Keycloak secret with admin credentials to your namespace

Openshift:
```bash
oc -n <edp_main_keycloak_project> get secret <edp_main_keycloak_secret> --export -o yaml | oc -n <edp_cicd_project> apply -f -
```

Kubernetes: 
```bash
kubectl -n <edp_main_keycloak_project> get secret <edp_main_keycloak_secret> --export -o yaml | kubectl -n <edp_cicd_project> apply -f -
```

 ```
    - <edp_main_keycloak_project>                   # namespace with deployed Keycloak;
    - <edp_main_keycloak_secret>                    # name of Keycloak secret;
 ```

* Go to the unzipped directory and deploy operator:

Parameters:
 ```
    - global.edpName                                # a namespace or a project name (in case of OpenShift);
    - global.platform                               # OpenShift or Kubernetes;
    - global.admins                                 # Administrators of your tenant separated by comma (,) (eg --set 'global.admins={test@example.com}');
    - global.developers                             # Developers of your tenant separated by comma (,) (eg --set 'global.developers={test@example.com}');
    - image.name                                    # EDP image. The released image can be found on [Dockerhub](https://hub.docker.com/repository/docker/epamedp/keycloak-operator);
    - image.version                                 # EDP tag. The released image can be found on [Dockerhub](https://hub.docker.com/repository/docker/epamedp/keycloak-operator/tags);
    - keycloak.url                                  # URL to Keycloak;
 ```

_**NOTE:** Follow instruction to create namespace [edp-install-openshift](https://github.com/epmd-edp/edp-install/blob/master/documentation/openshift_install_edp.md#install-edp) or [edp-install-kubernetes](https://github.com/epmd-edp/edp-install/blob/master/documentation/kubernetes_install_edp.md#install-edp)._

Inspect the sample of launching a Helm template for Keycloak operator installation:
```bash
helm install keycloak-operator --namespace <edp_cicd_project> --set name=keycloak-operator --set global.edpName=<edp_cicd_project> --set global.platform=<platform_type> deploy-templates
```

* Check the <edp_cicd_project> namespace that should contain Deployment with your operator in a running status

### Local Development
In order to develop the operator, first set up a local environment. For details, please refer to the [Local Development](documentation/local-development.md) page.
