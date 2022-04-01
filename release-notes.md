# Release notes

This topic contains release notes for Tanzu Application Platform v1.1

## <a id='1-1'></a> v1.1

**Release Date**: MMMM DD, 2022

### <a id='1-1-new-features'></a> New features

#### Installing


* **Tanzu Application Platform profile - Iterate** : This profile is intended for iterative development versus the path to production.

* **Tanzu Application Platform profile - Build** : This profile is intended for the transformation of source revisions to workload revisions. Specifically, hosting Workloads and SupplyChains. 

* **Tanzu Application Platform profile - Run** : This profile is intended for the transformation of workload revisions to running Pods. Specifically, hosting Deliverys and Deliverables.

* **Tanzu Application Platform profile - View** : This profile is intended for instances of applications related to centralized developer experiences. Specifically, the TAP GUI and Metadata Store.

* **Tanzu Application Platform profile - Full** : This profile contains all of the Tanzu Application Platform packages.
  - New packages included in the Full profile:
    - Application Live View (Build)
    - Application Live View (Run)
    - Application Live View (GUI)
    - Default Roles
    - Telemetry


#### Default roles for Tanzu Application Platform

- Introduction of [five new default roles](authn-authz/overview.md) and related permissions that apply to **k8s resources**. These roles help operators set up common sets of permissions to limit the access that users and service accounts have on a cluster that runs Tanzu Application Platform.
  - Three roles are for users, including: app-editor, app-viewer and app-operator.
  - Two roles are for “robot” or system permissions, including: workload and deliverable.

#### Tanzu Application Platform GUI

- **Runtime Resources Visibility plug-in:** 
  -  Introduced textual and enumerated table column filters for ease of search
  - Display meaningful errors and path forward to troubleshoot issues when system fails
  - Included 'tags' in Knative revision table in details page
  - Introduced Kubernetes Service on the resources page to provide more insights to Kubernetes service details
  - Improved several user interface components for a better user experience, including accessibility
  
- **Supply Chain Choreographer plug-in:** Added a new graphical representation of the execution of a workload through an installed supply chain. This  includes CRDs in the supply chain, the source results of each stage, as well as details to facilitate the troubleshooting of workloads on their path to production. 

#### <a id="app-acc-features"></a> Application Accelerator

- Option values can now be validated using regex
- TLS for ingress are enabled using `ingress.enable_tls` flag during package install

#### Application Live View

- Enabled multiple cluster support for Application Live View
- Split Application Live View components into three packages with new package reference names
- Enabled structured JSON logging for App Live View to meet the logging requirements of Tanzu Application Platform
- Made App Live View Convention Service compatible with the cert-manager version `1.7.1`
- Updated Spring Native `0.10.5` to `0.10.6`
- Updated `Spring Boot` to `2.5.12` to address `CVE-2022-22965` and `CVE-2020-36518`

#### Tanzu CLI - Apps plug-in

- `workload create/update/apply`:
  - Accept `workload.yaml` from stdin (through `--file -`).
  - Enable providing `spec.build.env` values (through new `–build.env` flag).
  - When `--git-repo` and `--git-tag` are provided, `git-branch` is not required.
  - Add new `--annotations` flag - annotation(s) provided are propagated to the running pod for the workload. 
- `workload list`:
  - Shorthand `-A` can be passed in for `--all-namespaces`.
- `workload get`:
  - Service Claim details are returned in command output.
  - The existing STATUS value in the Pods table in the output reflects when a pod is “Terminating.” 

**Deprecation**
* The `namespace` value that can be passed in for the `--service-ref` flag is now deprecated.
  * A deprecation warning message has been added to the `workload create/update/apply...` when user specifies a namespace in the `--service-ref` object

#### Service Bindings

- Applied [RFC-3339](https://datatracker.ietf.org/doc/html/rfc3339) timestamps to service binding logs.
- Added Tanzu Application Platform aggregate roles to support Tanzu Application Platform Authentication and Authorization (new feature referenced above).
- Added support for servicebinding.io/v1beta1
- Corrected Postgres resource pluralization error.

#### Source Controller

- Enable Source Controller to connect to image registries that use self-signed or private certificate authorities - to support airgapped installs
  - This is an optional configuration
  - See [Source Controller Installation](source-controller/install-source-controller.md) for details
- Applied [RFC-3339](https://datatracker.ietf.org/doc/html/rfc3339) timestamps to source controller logs.
- Added Tanzu Application Platform aggregate roles to support Tanzu Application Platform Authentication and Authorization (new feature referenced above).

#### Spring Boot Conventions

- Applied [RFC-3339](https://datatracker.ietf.org/doc/html/rfc3339) timestamps to service binding logs.
The following new conventions are applied to spring boot apps v2.6 and later:
- Add Kubernetes liveness and readiness probes by using spring boot health endpoints.
- Change management port from 8080 to 8081 to increase security of the management port.

#### Supply Chain Security Tools - Scan

- Support for configuring Supply Chain Security Tools - Scan to remotely connect to Supply Chain Security Tools - Store in a different cluster.

#### Supply Chain Security Tools – Sign

- Support configuring Webhook resources
- Support configuring Namespace where webhook is installed
- Support for registries with self-signed certificates

#### Supply Chain Security Tools - Store

- Added Contour Ingress support with custom domain name
- Created Tanzu CLI plug-in called `insight`
  - Currently, only MacOS and Linux are supported for the `insight` plug-in.
- Upgraded golang version from `1.17.5` to `1.17.8`

#### Tanzu Application Platform GUI

- **Runtime Resources Visibility plug-in:** 
- **Supply Chain Choreographer plug-in:** Added a new graphical representation of the execution of a workload through an installed supply chain. This  includes CRDs in the supply chain, the source results of each stage, as well as details to facilitate the troublshooting of workloads on their path to production. 

### <a id='1-1-breaking-changes'></a> Breaking changes

#### <a id="app-acc-changes"></a> Application Accelerator

- When enabling ingress the TLS support must now be explicitly enabled using `ingress.tls_enable`.

#### Supply Chain Security Tools - Scan

- **Deprecated API version:** API version `scanning.apps.tanzu.vmware.com/v1alpha1` is deprecated.

#### Supply Chain Security Tools - Store

- The independent `insight` CLI is deprecated, customers can now use Tanzu CLI plugin called `insight`.
  - Renamed all instances of `create` verb to `add` for all CLI commands.
  - Currently, only MacOS and Linux are supported for the `insight` plug-in.

### <a id='1-1-security-issues'></a> Security issues

This release has the following security issues:

None.

### <a id='1-1-resolved-issues'></a> Resolved issues

#### <a id="app-acc-resolved"></a> Application Accelerator

- Accelerator engine no longer fails with "java.lang.OutOfMemoryError: Direct buffer memory" when processing very large Git repositories.

#### Services Toolkit

- Resolved an issue with the `tanzu services` CLI plug-in which meant it was not compatible with Kubernetes clusters running on GKE.
- Fixed a potential race condition during reconciliation of ResourceClaims which might cause the Services Toolkit manager to stop responding.
- Updated configuration of the Services Toolkit carvel Package to prevent an unwanted build up of ConfigMap resources.

#### Supply Chain Security Tools – Scan

- Resolved two scan jobs and two scan pods being created when reconciling `ScanTemplates` and `ScanPolicies`.

#### Supply Chain Security Tools - Store

- Fixed an issue where querying a source report with local path name would return the following error: `{ "message": "Not found" }`
- Return related packages when querying image and source vulnerabilities
- Ratings are updated when updating vulnerabilities

#### Tanzu CLI - Apps plug-in

- Apps plug-in no longer crashes when `KUBECONFIG` includes the colon (`:`) config file delimiter.
- `tanzu apps workload get`: Passing in `--output json` and `--export` flags together exports the workload in JSON rather than YAML.
- `tanzu apps workload tail`: Duplicate log entries created for init containers are removed.
- `tanzu apps workload create/update/apply`
  - When the `--wait` flag passed and the prompt "Do you want to create this workload?" 
  is declined, the command immediately exits 0 rather than hanging (continuing to "wait").
  - Workload name is now validated when the workload values are passed in through `--file workload.yaml`.
  - When creating/applying a workload from –local-path, if user answers “No” to the prompt “Are you sure you want to publish your local code to [registry name] where others may be able to access it?”, the command now exits 0 immediately rather than showing the workload diff and prompting to continue with workload creation.
  - `.spec.build.env` in workload yaml definition file is being removed when using tanzu apps workload apply command


### <a id='1-1-known-issues'></a> Known issues

#### Tanzu Application Platform

- **Deprecated profile:** Tanzu Application Platform light profile is deprecated.

#### Application Live View

- **App Live View connector sometimes does not connect to the backend**
  Workaround: Check the app live view connector pod logs to see if there are any rsocket connection issues to the backend. 
  Try deleting the connector pod so it gets recreated:
  ```
  kubectl -n app-live-view delete pods -l=name=application-live-view-connector
  ```

#### Grype scanner

- **Scanning Java source code may not reveal vulnerabilities:** Source Code Scanning only scans
files present in the source code repository. No network calls are made to fetch dependencies.
For languages using dependency lock files, such as Golang and Node.js, Grype uses the
lock files to check the dependencies for vulnerabilities.
- For Java, dependency lock files are not guaranteed, so Grype instead uses the
dependencies present in the built binaries (`.jar` or `.war` files).
- Because VMware does not recommend committing binaries to source code repositories, Grype
fails to find vulnerabilities during a Source Scan. The vulnerabilities are still found during the
Image Scan, after the binaries are built and packaged as images.

#### Supply Chain Choreographer plug-in
- **Details for ConfigMap CRD not appearing:** `Unable to retrieve conditions for ConfigMap...` error appears in details section after clicking on the ConfigMap stage in the graph view of a supply chain. This does not necessarily mean that the workload failed its execution through the supply chain.

#### Supply Chain Security Tools – Scan

- **Scan Phase indicates `Scanning` incorrectly:** Scans have an edge case that when an error
  occurs during scanning, the `Scan Phase` field is not updated to `Error` and remains in the
  `Scanning` phase. Read the scan pod logs to verify the existence of an error.
- **User see error message saying Supply Chain Security Tools - Store (Store) is not configured even though configuration values were supplied:** The Scan Controller experiences a race-condition when deploying Store in the same cluster, that shows Store as not configured, even when it is present and properly configured. This happens when the Scan Controller is deployed and reconciled before the Store is reconciled and the corresponding secrets are exported to the Scan Controller namespace. As a workaround to this, once your Store is successfully reconciled, you would need to restart your Supply Chain Security Tools - Scan deployment by running: `kubectl rollout restart deployment.apps/scan-link-controller-manager -n scan-link-system`. If you deployed Supply Chain Security Tools - Scan to a different namespace than the default one, you can replace `-n scan-link-system` with `-n <my_custom_namespace>`.

#### Supply Chain Security Tools - Store

- **`insight` CLI plug-in does not support Windows:**

    Currently, only MacOS and Linux are supported for the `insight` plug-in.

- **Existing packages with new vulnerabilities not updated:**

    There’s a known issue in Supply Chain Security Tools - Store where it does not correctly save new vulnerablities for a package that had already been submitted in a previous report. This issue will result in new vulnerabilities not being saved to the database.

- **Persistent volume retains data:**

    If Supply Chain Security Tools - Store is deployed, deleted, and then redeployed the
    `metadata-store-db` Pod fails to start if the database password changed during redeployment.
    This is caused by the persistent volume used by postgres retaining old data, even though the retention
    policy is set to `DELETE`.

    To resolve this issue, see [solution](scst-store/troubleshooting.md#persistent-volume-retains-data-solution).


- **Missing persistent volume:**

    After Supply Chain Security Tools - Store is deployed, `metadata-store-db` Pod might fail for missing
    volume while `postgres-db-pv-claim` pvc is in the `PENDING` state.
    This issue may occur if the cluster where Supply Chain Security Tools - Store is deployed does not have
    `storageclass` defined.

    The provisioner of `storageclass` is responsible for creating the persistent volume after
    `metadata-store-db` attaches `postgres-db-pv-claim`. To resolve this issue, see
    [solution](scst-store/troubleshooting.md#missing-persistent-volume-solution).

- **No support for installing in custom namespaces:**

    Supply Chain Security Tools — Store is deployed to the `metadata-store` namespace.
    There is no support for configuring the namespace.

#### Application Live View

- **App Live View connector sometimes does not connect to the back end:**

    Workaround: 
    
    - Check the app live view connector pod logs to verify if there are any rsocket connection issues to the back end. 
    
    - Try deleting the connector pod so it gets re-created:
    
        ```
        kubectl -n app-live-view delete pods -l=name=application-live-view-connector
        ```

## <a id='1-0'></a> v1.0

**Release Date**: January 11, 2022

### Known issues

This release has the following issues:

#### Installing

When you install Tanzu Application Platform on Google Kubernetes Engine (GKE), Kubernetes control
plane can be unavailable for several minutes during the installation.
Package installations can enter the `ReconcileFailed` state. When API server becomes available,
packages try to reconcile to completion.

This can happen on newly provisioned clusters that have not finished GKE API server autoscaling.
When GKE scales up an API server, the current Tanzu Application install continues, and any subsequent installs succeed without interruption.

#### Application Accelerator

- Build scripts provided as part of an accelerator do not have the execute bit set when a new
  project is generated from the accelerator.

    To resolve this issue, explicitly set the execute bit. For more information, see
    [Execute Bit Not Set for App Accelerator Build Scripts](troubleshooting.html#build-scripts-lack-execute-bit)
    in _Troubleshooting Tanzu Application Platform_.
#### Application Live View

The Live View section in Tanzu Application Platform GUI might show
"No live information for pod with ID" after deploying Tanzu Application Platform workloads.

Resolve this issue by recreating the Application Live View Connector pod. For more information, see
[No Live Information for Pod with ID Error](troubleshooting.html#no-live-information) in
_Troubleshooting Tanzu Application Platform_.

#### Convention Service

Convention Service does not currently support custom certificates for integrating with a private
registry. Support for custom certificates is planned for an upcoming release.

#### Developer Conventions

**Debug Convention might not apply:** If you upgraded from Tanzu Application Platform v0.4 then the
the debug convention might not apply to the app run image. This is because of the missing SBOM data
in the image.
To prevent this issue, delete existing app images that were built using Tanzu Application Platform
v0.4.

For more information, see [Debug Convention May Not Apply](troubleshooting.html#debug-convention) in
_Troubleshooting Tanzu Application Platform_.

#### Grype scanner

- **Scanning Java source code may not reveal vulnerabilities:** Source Code Scanning only scans
files present in the source code repository. No network calls are made to fetch dependencies.
- For languages that make use of dependency lock files, such as Golang and Node.js, Grype uses the
lock files to check the dependencies for vulnerabilities.
- In the case of Java, dependency lock files are not guaranteed, so Grype instead uses the
dependencies present in the built binaries (`.jar` or `.war` files).
- Because best practices do not include committing binaries to source code repositories, Grype
fails to find vulnerabilities during a Source Scan. The vulnerabilities are still found during the
Image Scan, after the binaries are built and packaged as images.

#### Learning Center

- **Training Portal in pending state:** Under certain circumstances, the training portal is stuck in
a pending state. To resolve this issue, see
[Training portal stays in pending state](learning-center/troubleshoot-learning-center.md#training-portal-pending).

- **image-policy-webhook-service not found:** If you are installing a Tanzu Application Platform
profile, you might see the error:

    ```
    Internal error occurred: failed calling webhook "image-policy-webhook.signing.apps.tanzu.vmware.com": failed to call webhook: Post "https://image-policy-webhook-service.image-policy-system.svc:443/signing-policy-check?timeout=10s": service "image-policy-webhook-service" not found
    ```

    This is a rare condition error among some packages. To recover from this error, redeploy the
    `trainingPortal` resource.

- **Cannot Update Parameters:**
Normally you must update some parameters provided to the Learning Center Operator. These parameters
include ingressDomain, TLS secret, ingressClass, and others.

    After updating parameters, if the Training Portals do not work or you cannot see the updated values,
    redeploy `trainingportal` in a maintenance window where Learning Center is unavailable while the
    `systemprofile` is updated.

- **Increase your cluster's resources:** Node pressure may be caused by not enough nodes or not
enough resources on nodes for deploying the workloads you have. In this case, follow your cloud
provider instructions on how to scale out or scale up your cluster.

#### Supply Chain Choreographer

Deployment from a public Git repository might require a Git SSH secret.
Workaround is to configure SSH access for the public Git repository.

#### Supply Chain Security Tools – Scan

- **Events show `SaveScanResultsSuccess` incorrectly:** `SaveScanResultsSuccess` appears in the
events when the Supply Chain Security Tools - Store is not configured. The `.status.conditions`
output, however, correctly reflects `SendingResults=False`.
- **Scan Phase indicates `Scanning` incorrectly:** Scans have an edge case where, when an error has
occurred during scanning, the Scan Phase field is not updated to `Error` and instead remains in the
`Scanning` phase. Read the scan Pod logs to verify there was an error.
- **CVE print columns are not getting populated:** After running a scan and using `kubectl get` on
the scan, the CVE print columns (CRITICAL, HIGH, MEDIUM, LOW, UNKNOWN, CVETOTAL) are not populated.
You can run `kubectl describe` on the scan and look for `Scan completed. Found x CVE(s): ...` under
`Status.Conditions` to find these severity counts and `CVETOTAL`.
- **Failing Blob source scans:** Blob source scans have an edge case where, when a compressed file
without a `.git` directory is provided, sending results to the Supply Chain Security Tools - Store
fails and the scanned revision value is not set. The current workaround is to add the `.git`
directory to the compressed file.
- **Scan controller pod fails:** If there is a misconfiguration
(i.e. secretgen-controller not running, wrong CA secret name) after enabling the
metadata store integration, the controller pod fails. The current workaround is
to update the `tap-values.yaml` file with the proper configuration and update the application.
- **Deleted resources keep reconciling:** After creating a scan CR and deleting it,
the controllers keep trying to fetch the deleted resource, resulting in a `not found`
or `unable to fetch` log entry with every reconciliation cycle.
- **Scan controller crashes when Git clone fails:** If this occurs, confirm that
the Git URL and the SSH credentials are correct.

#### Supply Chain Security Tools - Sign

- **Blocked pod creation:** If all webhook nodes or pods are evicted by the cluster or scaled down,
the admission policy blocks any pods from being created in the cluster.
To resolve the issue, delete `MutatingWebhookConfiguration` and re-apply it when the cluster is
stable.

- **MutatingWebhookConfiguration prevents pods from being admitted:** Under certain circumstances,
  if the `image-policy-controller-manager` deployment pods do not start up before the
  `MutatingWebhookConfiguration` is applied to the cluster, it can prevent the admission of all
  pods.

    To resolve this issue, delete the `MutatingWebhookConfiguration` resource, then restore the
    `MutatingWebhookConfiguration` resource to re-enable image signing enforcement. For instructions,
    see [MutatingWebhookConfiguration Prevents Pod Admission](troubleshooting.html#pod-admission-prevented)
    in _Troubleshooting Tanzu Application Platform_.

- **Terminated kube-dns prevents new pods from being admitted:**
If `kube-dns` is terminated, it prevents the admission controller from being able to reach the image policy controller. This prevents new pods from being admitted, including core services like kube-dns.

    Modify the mutating webhook configuration to exclude the `kube-system` namespace from the admission check. This allows pods in the `kube-system` to appear, which should restore `kube-dns`

- **Priority class of webhook's pods might preempt less privileged pods:**
This component uses a privileged `PriorityClass` to start up its pods in order to prevent node
pressure from preempting its pods. However, this can cause other less privileged components to have
their pods preempted or evicted instead.

    To resolve this issue, see [Priority Class of Webhook's Pods Preempts Less Privileged Pods](troubleshooting.html#priority-class-preempts)
    in _Troubleshooting Tanzu Application Platform_.

#### Supply Chain Security Tools - Store

- **Existing packages with new vulnerabilities not updated:**

    There’s a known issue in Supply Chain Security Tools - Store where it does not correctly save new vulnerablities for a package that had already been submitted in a previous report. This issue will result in new vulnerabilities not being saved to the database.

- **Persistent volume retains data:**

    If Supply Chain Security Tools - Store is deployed, deleted, and then redeployed the
    `metadata-store-db` Pod fails to start if the database password changed during redeployment.
    This is caused by the persistent volume used by postgres retaining old data, even though the retention
    policy is set to `DELETE`.

    To resolve this issue, see [solution](scst-store/troubleshooting.md#persistent-volume-retains-data-solution).


- **Missing persistent volume:**

    After Supply Chain Security Tools - Store is deployed, `metadata-store-db` Pod might fail for missing
    volume while `postgres-db-pv-claim` pvc is in the `PENDING` state.
    This issue may occur if the cluster where Supply Chain Security Tools - Store is deployed does not have
    `storageclass` defined.

    The provisioner of `storageclass` is responsible for creating the persistent volume after
    `metadata-store-db` attaches `postgres-db-pv-claim`. To resolve this issue, see
    [solution](scst-store/troubleshooting.md#missing-persistent-volume-solution).

- **No support for installing in custom namespaces:**

    Supply Chain Security Tools — Store is deployed to the `metadata-store` namespace.
    There is no support for configuring the namespace.

- **Querying local path source reports:**

    If a source report has a local path as the name -- for example,
    `/path/to/code` -- the leading `/` on the resulting repository name
    causes the querying packages and vulnerabilities to return the
    following error from the client lib and the CLI: `{ "message": "Not found" }`.

    The URL of the resulting HTTP request is properly escaped. For example,
    `/api/sources/%2Fpath%2Fto%2Fdir/vulnerabilities`.
    
    The rbac-proxy used for authentication handles this URL in a way that the response is a redirect. For example, `HTTP 301\nLocation: /api/sources/path/to/dir/vulnerabilities`. The Client Lib follows the redirect, making a request to the new URL which does not exist in the
    Supply Chain Security Tools - Store API, resulting in this error message.

#### Tanzu CLI

- `tanzu apps workload get`: Passing in `--output json` with the `--export` flag returns YAML
rather than JSON. Support for honoring the `--output json` with `--export` is planned for the next
release.
- `tanzu apps workload create/update/apply`: `--image` is not supported by the default supply chain
in Tanzu Application Platform v0.3.
`--wait` functions as expected when a workload is created for the first time but might return
prematurely on subsequent updates when passed with `workload update/apply` for existing workloads.
When the `--wait` flag is included and you decline the "Do you want to create this workload?"
prompt, the command continues to wait and must be cancelled manually.

#### Tanzu Dev Tools for VSCode

**Unable to configure task:** Launching the `Extension Host`, and configuring `tasks` in a workspace
that does not contain workload YAML files might not work.
To solve this issue, uninstall the Tanzu Dev Tools extension.

**Extension Pack for Java:** In some cases, the Extension Pack for Java (`vscjava.vscode-java-pack`)
does not automatically install. In these cases debugging does not work after Tanzu Dev Tools
installs `live-update`.
To solve this issue, manually install `vscjava.vscode-java-pack` from the extension marketplace.

#### Services Toolkit

* It is not possible for more than one application workload to consume the same service instance.
Attempting to create two or more application workloads while specifying the same `--service-ref`
value causes only one of the workloads to bind to the service instance and reconcile successfully.
This limitation is planned to be relaxed in an upcoming release.
* The `tanzu services` CLI plug-in is not compatible with Kubernetes clusters running on GKE.

### <a id='1-0-security-issues'></a> Security issue

The installation specifies that the installer's Tanzu Network credentials be exported to all
namespaces. Customers can choose to mitigate this concern using one of the following methods:

- Create a Tanzu Network account with their own credentials and use this for the installation
exclusively.
- Using [Carvel tool's imgpkg](https://carvel.dev/imgpkg/) customers can create a dedicated OCI
registry on their own infrastructure that can comply with any required security policies that might
exist.


### <a id='1-0-breaking-changes'></a> Breaking changes

This release has the following breaking change:

**Supply Chain Security Tools - Store:** Changed package name to `metadata-store.apps.tanzu.vmware.com`.

### <a id='1-0-resolved-issues'></a> Resolved issues

This release has the following fixes:

#### Tanzu Dev Tools for VSCode

- Fixed issue where the Tanzu Dev Tools extension might not support projects with multi-document
YAML files
- Modified debug to remove any leftover port-forwards from past runs

#### Supply Chain Security Tools - Store

Upgrade golang version from `1.17.1` to `1.17.5`
