

1. (add Argo app) Once logged to the Argo CD console, click on the "New App+" button in the upper left of the Argo CD console and fill out the form with values matching the Cloud Pak of your choice, according to the table below:

    For all other fields, use the following values:

    | Field | Value |
    | ----- | ----- |
    | Application Name | argo-app |
    | Path | config/argocd |
    | Namespace | openshift-gitops |
    | Project | default |
    | Sync policy | Automatic |
    | Self Heal | true |
    | Repository URL | https://github.com/CPDWim/euNTT |
    | Revision | HEAD |
    | Cluster URL | <https://kubernetes.default.svc> |

1. (add Cloud Pak Shared app) Click on the "New App+" button again and fill out the form with values matching the Cloud Pak of your choice, according to the table below:

    For all other fields, use the following values:

    | Field | Value |
    | ----- | ----- |
    | Application Name | cp-shared-app |
    | Path | config/argocd-cloudpaks/cp-shared |
    | Namespace | ibm-cloudpaks |
    | Project | default |
    | Sync policy | Automatic |
    | Self Heal | true |
    | Repository URL | https://github.com/CPDWim/euNTT |
    | Revision | HEAD |
    | Cluster URL | <https://kubernetes.default.svc> |

2. After filling out the form details, click the "Create" button

3. (add actual Cloud Pak) Click on the "New App+" button again and fill out the form with values matching the Cloud Pak of your choice, according to the table below:

    Note that if you want to deploy a Cloud Pak to a non-default namespace, you need to make sure you pass the same namespace values used in the optional parameter values for the `cp-shared` application.

    | Cloud Pak | Application Name | Path | Namespace |
    | --------- | ---------------- | ---- | --------- |
   
    | Data | cp4d-app | config/argocd-cloudpaks/cp4d | cp4d |


    For all other fields, use the following values:

    | Field | Value |
    | ----- | ----- |
    | Project | default |
    | Sync policy | Automatic |
    | Self Heal | true |
    | Repository URL | https://github.com/CPDWim/euNTT |
    | Revision | HEAD |
    | Cluster URL | <https://kubernetes.default.svc> |

4. After filling out the form details, click the "Create" button

5. Under "Parameters," set the values for the fields `storageclass.rwo` and `storageclass.rwx` with the appropriate storage classes. For OpenShift Container Storage, the values will be `ocs-storagecluster-ceph-rbd` and `ocs-storagecluster-cephfs`, respectively.

6. After filling out the form details, click the "Create" button

7. Wait for the synchronization to complete.
