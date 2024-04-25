# Installation and configuration instructions
## Mirroring images
### Terminology
> - bastion01 : indicates the bastion server that has access to the internet, access to the online repositories
> - bastion02 : indicates the bastion server, airgapped with access to the private repositories

### Pre-requisites

parameters: 
- VERSION : 4.8.3
- COMPOMENTS : cpd_platfrom,wkc

*list of components will vary based on environment to be installed*

podman or docker installed

### Bastion 01
cpd-cli needs to be downloaded and installed, see [Installing the IBM Cloud Pak for Data command-line interface](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.8.x?topic=workstation-installing-cloud-pak-data-cli).

Download from github:

From the following link (https://github.com/IBM/cpd-cli/releases) select the correct version and open asset , download platform and release.
For 4.8.2 enterprise edition: (https://github.com/IBM/cpd-cli/releases/download/v13.1.3/cpd-cli-linux-EE-13.1.3.tgz)

Example for linux bastion01:

>`curl -L https://github.com/IBM/cpd-cli/releases/download/v13.1.3/cpd-cli-linux-EE-13.1.3.tgz > /tmp/cpd-cli-linux-EE-13.1.3.tgz`
>
>`tar -xvf /tmp/cpd-cli-linux-EE-13.1.3.tgz`
>
>`export PATH=/home/admin/cpd-cli-linux-EE-13.1.3-98:$PATH`
>
>`cpd-cli manage restart-container`
>
>`cpd-cli manage save-image --from=icr.io/cpopen/cpd/olm-utils-v2:latest`

By doing the above the olm-utils-v2 latest client will be in the offline folder

#### Download case file

>`cpd-cli manage case-download --components=${COMPONENTS} --release=${VERSION}`

#### Mirror images to intermediate container

Login to the entitled registry
>`cpd-cli manage login-entitled-registry ${IBM_ENTITLEMENT_KEY}`
Make sure component parameter is set

List images (will also download the case packages)
>`cpd-cli manage list-images --components=${COMPONENTS} --release=${VERSION} --inspect_source_registry=true`

Mirror images to intermediate container
>`cpd-cli manage mirror-images --components=${COMPONENTS} --release=${VERSION} --target_registry=127.0.0.1:12443 --arch=${IMAGE_ARCH} --case_download=false`

Copy the entire cpd-cli workspace.

### Bastion 02

Put cpd-cli and copied workspace on Bastion 02

Login to private container regisry
>`cpd-cli manage login-private-registry ${PRIVATE_REGISTRY_LOCATION} ${PRIVATE_REGISTRY_PUSH_USER} ${PRIVATE_REGISTRY_PUSH_PASSWORD}`

Mirror the images to private registry

>`cpd-cli manage mirror-images --components=${COMPONENTS} --release=${VERSION} --source_registry=127.0.0.1:12443 --target_registry {PRIVATE_REGISTRY_LOCATION} --arch=${IMAGE_ARCH} --case_download=false`



# Documentation To renew to Kustomize

## Argo specific context
1. Add Argo app
Once logged to the Argo CD console, click on the "New App+" button in the upper left of the Argo CD console and fill out the form with values matching the Cloud Pak of your choice, according to the table below:

    For all other fields, use the following values:

    | Field | Value |
    | ----- | ----- |
    | Application Name | argo-app |
    | Path | config/argocd |
    | Namespace | openshift-gitops |
    | Project | default |
    | Sync policy | Automatic |
    | Self Heal | true |
    | Repository URL | <https://github.com/CPDWim/euNTT> |
    | Revision | HEAD |
    | Cluster URL | <https://kubernetes.default.svc> |

## Cluster based shared settings
1. (add Cloud Pak Shared app) Click on the "New App+" button again and fill out the form with values matching the Cloud Pak of your choice, according to the table below:

    For all other fields, use the following values:

    | Field | Value |
    | ----- | ----- |
    | Application Name | cp-shared-app |
    | Path | config/cloudpaks/cp-shared |
    | Namespace | ibm-cloudpaks |
    | Project | default |
    | Sync policy | Automatic |
    | Self Heal | true |
    | Repository URL | <https://github.com/CPDWim/euNTT> |
    | Revision | HEAD |
    | Cluster URL | <https://kubernetes.default.svc> |

2. After filling out the form details, click the "Create" button

## Cloud pak applications

1. (add actual Cloud Pak) Click on the "New App+" button again and fill out the form with values matching the Cloud Pak of your choice, according to the table below:

    Note that if you want to deploy a Cloud Pak to a non-default namespace, you need to make sure you pass the same namespace values used in the optional parameter values for the `cp-shared` application.

    | Cloud Pak | Application Name | Path | Namespace |
    | --------- | ---------------- | ---- | --------- |
    | Data | cp4denv01 | config/cloudpaks/cp4denv01 | cp4d |

    For all other fields, use the following values:

    | Field | Value |
    | ----- | ----- |
    | Project | default |
    | Sync policy | Automatic |
    | Self Heal | true |
    | Repository URL | <https://github.com/CPDWim/euNTT> |
    | Revision | HEAD |
    | Cluster URL | <https://kubernetes.default.svc> |

2. After filling out the form details, click the "Create" button

3. After filling out the form details, click the "Create" button

4. Wait for the synchronization to complete.
