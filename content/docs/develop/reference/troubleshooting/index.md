---
title: Troubleshooting
description: Find tips and tricks to deal with troubles that may occur when using Keptn. 
weight: 100
keywords: [troubleshooting]
---

In this section, instructions have been summarized that help to troubleshoot known issues that may occur when using Keptn.

## Generating a Support Archive

The Keptn CLI allows to generate a support archive, which can be used as data source for debugging a Keptn installation.
For generating a support archive, please checkout the CLI command [keptn generate support-archive](../../cli/commands/keptn_generate_support-archive).


## Verifying a Keptn installation

Especially for troubleshooting purposes it is necessary to verify that all parts of the Keptn installation are running as intended (e.g., no crashed pods, all distributors running).

<details><summary>Expand instructions</summary>
<p>

- To verify your Keptn installation, retrieve the pods running in the `keptn` namespace.

```console
kubectl get pods -n keptn
```

```console
NAME                                                              READY     STATUS    RESTARTS   AGE
api-5cfd44687-b2sqr                                               1/1       Running   0          34m
bridge-54d65cd4c5-9hwsl                                           1/1       Running   0          34m
configuration-service-75df569979-qvg8t                            1/1       Running   0          34m
eventbroker-go-f44576fcb-z2ddv                                    1/1       Running   0          34m
gatekeeper-service-6d5d798ccd-d442x                               1/1       Running   0          34m
gatekeeper-service-evaluation-done-distributor-7556c87d9b-xbffs   1/1       Running   0          34m
helm-service-596b4855b4-zkb77                                     1/1       Running   0          34m
helm-service-configuration-change-distributor-58d97df957-2msfs    1/1       Running   0          34m
helm-service-service-create-distributor-58584b6f7-4l9rr           1/1       Running   0          34m
jmeter-service-7d9c654c9c-xgz7s                                   1/1       Running   0          34m
jmeter-service-deployment-distributor-6dbd4858bf-v2stj            1/1       Running   0          34m
keptn-nats-cluster-1                                              1/1       Running   0          34m
lighthouse-service-6497f48947-vvs5g                               1/1       Running   0          34m
lighthouse-service-get-sli-done-distributor-56896bb59c-d6tlp      1/1       Running   0          34m
lighthouse-service-start-evaluation-distributor-5fb47dcfd-mklxx   1/1       Running   0          34m
lighthouse-service-tests-finished-distributor-5dfc978bd4-7hl44    1/1       Running   0          34m
nats-operator-7dcd546854-nhpm5                                    1/1       Running   0          34m
prometheus-service-6db877499c-vvvg5                               1/1       Running   0          33m
prometheus-service-monitoring-configure-distributor-5f789fvn69f   1/1       Running   0          33m
prometheus-sli-service-66f8b8d86f-stgzr                           1/1       Running   0          33m
prometheus-sli-service-monitoring-configure-distributor-675x5kp   1/1       Running   0          33m
remediation-service-f6bbc48b5-g47kt                               1/1       Running   0          34m
remediation-service-problem-distributor-79885bd957-nz74j          1/1       Running   0          34m
servicenow-service-7cd9b8784-xxj7z                                1/1       Running   0          33m
servicenow-service-problem-distributor-666fbf4b6-l62dj            1/1       Running   0          33m
shipyard-service-565b96cb9c-mz2cl                                 1/1       Running   0          34m
shipyard-service-create-project-distributor-c65b7c677-nkmnk       1/1       Running   0          34m
shipyard-service-delete-project-distributor-55b86db7b-kd28z       1/1       Running   0          34m
wait-service-7b4d74b4d9-b4lk7                                     1/1       Running   0          34m
wait-service-deployment-distributor-55cd8fc655-n5px7              1/1       Running   0          34m
openshift-route-service-57b45c4dfc-4x5lm                          1/1       Running   0          32s (OpenShift only)
openshift-route-service-create-project-distributor-7d4454cs44xp   1/1       Running   0          33s (OpenShift only)
```

- In the `keptn-datastore` namespace, you should see the following pods:

```console
kubectl get pods -n keptn-datastore
```

```console
NAME                                             READY   STATUS    RESTARTS   AGE
mongodb-7d956d5775-mkxv5                         1/1     Running   0          5m16s
mongodb-datastore-d65b468d7-tmwfm                1/1     Running   0          5m14s
mongodb-datastore-distributor-6cc947d554-tn6kr   1/1     Running   0          5m7s
```

- To verify the Istio installation, retrieve all pods within the `istio-system` namespace and check whether they are running:

```console
kubectl get pods -n istio-system
```

```console
NAME                                      READY     STATUS    RESTARTS   AGE
istio-citadel-6c456d967c-bpqbd            1/1     Running     0          6m
istio-cleanup-secrets-1.2.5-22gts         0/1     Completed   0          6m
istio-ingressgateway-5d49795589-tfl4k     1/1     Running     0          6m
istio-init-crd-10-rzlf7                   0/1     Completed   0          6m
istio-init-crd-11-chvzr                   0/1     Completed   0          6m
istio-init-crd-12-8zvn4                   0/1     Completed   0          6m
istio-pilot-79b78c894b-zsz5j              2/2     Running     0          6m
istio-security-post-install-1.2.5-glswk   0/1     Completed   0          6m
istio-sidecar-injector-bcf445789-gkfjf    1/1     Running     0          6m
```
</p></details>

## Installation on Azure aborts
<details><summary>Expand instructions</summary>
<p>

**Investigation:**

The Keptn installation is aborting with the following error:

```console
Cannot obtain the cluster/pod IP CIDR
```

**Reason:** 

The root cause of this issue is that `kubenet` is not used in your AKS cluster. However, it is needed to retrieve the `podCidr` according to the official docs: https://docs.microsoft.com/en-us/rest/api/aks/managedclusters/createorupdate#containerservicenetworkprofile 

**Solution:** 

Please select the **Kubenet network plugin (basic)** when setting up your AKS cluster, instead of *Azure network plugin (advanced)* and retry the installation. You can find more information here: https://docs.microsoft.com/en-us/azure/aks/configure-kubenet 

</p></details>


## Troubleshooting the Installer

In some cases the installer is not running correctly or crashes.

<details><summary>Expand instructions</summary>
<p>

**Investigation:**

The Keptn installation is aborting with an error. Investigation needs to be conducted using the following commands:

* Show all deployed pods in the default namespace (should show the status of the installer pod): ``kubectl get pods``
* Show status of the installer job: ``kubectl get jobs``
* Get logs of the installer job: ``kubectl logs jobs/installer``
* If the installer has partially finished, [verify your Keptn installation](#verifying-a-keptn-installation)

**Possible solutions:**

* If the installer pod shows an ImagePullBackOff error, verify that your cluster can connect to the Internet to pull images (e.g., from docker.io).
* If the installer pod has started, but crashes, please create a [new bug report](https://github.com/keptn/keptn/issues/new?assignees=&labels=bug&template=bug_report.md&title=) with the output of above commands.


</p></details>


## Error: UPGRADE FAILED: timed out waiting for the condition

This error often appears when executing `keptn send event new-artifact` in case of insufficient CPU and/or memory on the Kubernetes cluster.

<details><summary>Expand instructions</summary>
<p>

**Investigation:**

The Helm upgrade runs into a time-out when deploying a new artifact of your service using

```console
keptn send event new-artifact
```

**Reason:** 

In this case Helm creates a new Kubernetes Deployment with the new artifact, but Kubernetes fails to start the pod. 
Unfortunately, there is no way to catch this error by Helm (right now). A good way to detect the error is to look at the Kubernetes events captured by the cluster:

```console
kubectl -n sockshop-dev get events  --sort-by='.metadata.creationTimestamp'
```

where `sockshop-dev` is the project and stage that you are trying to deploy to.

*Note*: This error can also occur at a later stage (e.g., when using blue-green deployments).

**Solution:** 

Increase the number of vCPUs and/or memory, or add another Kubernetes worker node.

</p></details>


## Helm upgrade runs into a time-out on EKS

Same as the error above, but this issue occurs sometimes using a _single_ worker node on EKS.

**Solution:** 

Increase the number of worker nodes. For example, you can therefore use the `eksctl` CLI:
https://eksctl.io/usage/managing-nodegroups/


## Verify Kubernetes Context with Keptn Installation

If you are performing critical operations, such as installing new Keptn services or upgrading something, please verify
that you are connected to the correct cluster.

An easy way to accomplish this is to compare the domain stored in the Kubernetes ConfigMap `keptn-domain` with the output of `keptn status`.

```console
$ kubectl get cm keptn-domain -n keptn -o=jsonpath='{.data.app_domain}'

123.45.67.89.xip.io
``` 
vs.
```console
$ keptn status
Starting to authenticate
Successfully authenticated
CLI is authenticated against the Keptn cluster https://api.keptn.123.45.67.89.xip.io
```

As you can see, the domains match (despite the `https://api.keptn.` prefix in the output of `keptn status`).