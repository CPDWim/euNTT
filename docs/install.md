# Installation and configuration instructions
## Mirroring images
Get olm-utils:  
> cpd-cli manage save-image \
> --from=icr.io/cpopen/cpd/olm-utils-v2:latest
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
