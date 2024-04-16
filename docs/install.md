# Installation and configuration instructions
## Mirroring images
### Terminology
> - bastion01 : indicates the bastion server that has access to the internet, access to the online repositories
> - bastion02 : indicates the bastion server, airgapped with access to the private repositories

### Pre-requisites

podman or docker installed

cpd-cli needs to be downloaded and installed, see [Installing the IBM Cloud Pak for Data command-line interface](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.8.x?topic=workstation-installing-cloud-pak-data-cli).

Download from github:

From the following link (https://github.com/IBM/cpd-cli/releases) select the correct version and open asset , download platform and release.
For 4.8.2 enterprise edition: (https://github.com/IBM/cpd-cli/releases/download/v13.1.3/cpd-cli-linux-EE-13.1.3.tgz)

Example for linux bastion01:

>curl -L https://github.com/IBM/cpd-cli/releases/download/v13.1.3/cpd-cli-linux-EE-13.1.3.tgz > /tmp/cpd-cli-linux-EE-13.1.3.tgz
>
>tar -xvf /tmp/cpd-cli-linux-EE-13.1.3.tgz
>
>export PATH=/home/admin/cpd-cli-linux-EE-13.1.3-98:$PATH
>
>cpd-cli manage restart-container
>
>cpd-cli manage save-image --from=icr.io/cpopen/cpd/olm-utils-v2:latest

By doing the above the olm-utils-v2 latest client will be in the offline folder


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
