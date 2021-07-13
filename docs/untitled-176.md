# Install the Snyk controller with OpenShift 4 and OperatorHub

* [ Kubernetes integration overview](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360003916138-Kubernetes-integration-overview/README.md)
* [ Install the Snyk controller with Helm](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360003916158-Install-the-Snyk-controller-with-Helm/README.md)
* [ Automatic import/deletion of Kubernetes workloads projects](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360020835037-Automatic-import-deletion-of-Kubernetes-workloads-projects/README.md)
* [ Install the Snyk controller with OpenShift 4 and OperatorHub](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360006548317-Install-the-Snyk-controller-with-OpenShift-4-and-OperatorHub/README.md)
* [ Install the Snyk controller on Amazon Elastic Kubernetes Service \(Amazon EKS\)](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360011128137-Install-the-Snyk-controller-on-Amazon-Elastic-Kubernetes-Service-Amazon-EKS-/README.md)
* [ Adding Kubernetes workloads for security scanning](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360003947117-Adding-Kubernetes-workloads-for-security-scanning/README.md)
* [ Viewing project details and test results](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360003916178-Viewing-project-details-and-test-results/README.md)
* [ Snyk Priority Score and Kubernetes](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360010906897-Snyk-Priority-Score-and-Kubernetes/README.md)
* [ Viewing your Kubernetes integration settings](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360006368657-Viewing-your-Kubernetes-integration-settings/README.md)
* [ Disable the Kubernetes integration](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360003947137-Disable-the-Kubernetes-integration/README.md)

## Install the Snyk controller with OpenShift 4 and OperatorHub

To get vulnerability details about your Kubernetes workloads, a Snyk admin must first install the Snyk controller onto your cluster. For most Kubernetes platforms, the Snyk monitor requires only some minimal configuration items in order to work correctly.

As with any Kubernetes deployment, the Snyk controller runs within a single namespace.

### Prerequisites

**Feature availability**  
This feature is available with all paid plans. See [Pricing plans](https://snyk.io/plans/) for more details.

* The Snyk controller is installed using OperatorHub through RedHat OpenShift 4.
* Set up your Snyk account before getting started.
* To configure the integration from Snyk, you must be an administrator for the account.
* A minimum 50 GB of storage must be available in the form of an [emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) on the cluster and the person configuring the cluster must be an administrator.
* External internet access must be available from the Kubernetes cluster.
* Ensure that your Kubernetes command-line tool \(`kubectl`\) points to the relevant cluster.
* Create a unique namespace for the Snyk controller with Cluster scope to enable the controller to monitor all of its deployments:

  ```text
  kubectl create namespace snyk-monitor
  ```

  **Tip**

  Use a unique namespace to isolate the controller resources more easily. This is generally good practice for Kubernetes applications. Notice our namespace is called snyk-monitor, you’ll need this later when configuring other resources.

  **Tip**

  Cluster scope is the default scope and we recommend you use this when installing the namespace so that we can scan the entire cluster. You can choose Namespaced scope, in which case the Snyk controller will watch for workloads only in the namespace in which it is deployed!

* Now, log in to your Snyk account and navigate to Integrations.
* Search for and click Kubernetes.
* Click Connect and from the page that loads, copy the Integration ID. The Snyk Integration ID is a UUID, similar to this format: abcd1234-abcd-1234-abcd-1234abcd123
* Save it for use from your Kubernetes environment in the next step.
* The Snyk monitor runs by using your Snyk Integration ID, and using a dockercfg file. If you are not using any private registries, create a Kubernetes secret called `snyk-monitor` containing the Snyk Integration ID from the previous step and run the following command:

  ```text
  kubectl create secret generic snyk-monitor -n snyk-monitor --from-literal=dockercfg.json={} --from-literal=integrationId=abcd1234-abcd-1234-abcd-1234abcd1234
  ```

  Now, skip the next step if you're not working with any private registries.

  **Note**

  The secret must be called `snyk-monitor` in order for the integration to work.

* If any of the images you need to scan are located in private registries, provide credentials to access those registries by creating a secret \(which must be called `snyk-monitor`\) using both the Snyk Integration ID as well as a `dockercfg` file.

  The `dockercfg` file is necessary to allow the monitor to look up images in private registries. Usually a copy of the `dockercfg` resides in `$HOME/.docker/config.json`.

  1. Create a `dockercfg` configuration file:

     ```text
     {
       "auths": {
         "gcr.io": {
           "auth": "BASE64-ENCODED-AUTH-DETAILS"
         }
         // Add other registries as necessary
       }
     }
     ```

  2. Create a secret with the file added:

     ```text
     kubectl create secret generic snyk-monitor -n snyk-monitor --from-file=dockercfg.json --from-literal=integrationId=abcd1234-abcd-1234-abcd-1234abcd1234
     ```

* If your registry is using self-signed or other additional certificates you must make those available to Snyk monitor. First place the `.crt`, `.cert`, and/or `.key` files in a directory and create a ConfigMap:

  ```text
  kubectl create configmap snyk-monitor-certs \
          -n snyk-monitor --from-file=<path_to_certs_directory>
  ```

* If you are using an insecure registry or your registry is using unqualified images, you can provide a `registries.conf` file.

  ```text
  [[registry]]
  location = "internal-registry-for-example.net/bar"
  insecure = true
  ```

  See the [documentation](https://github.com/containers/image/blob/master/docs/containers-registries.conf.5.md) for information on the format and further examples. Once you've created the file, you can use it to create the following ConfigMap:

  ```text
  kubectl create configmap snyk-monitor-registries-conf \
          -n snyk-monitor \
          --from-file=<path_to_registries_conf_file>
  ```

* Log in to your OpenShift Container Platform \(OCP\) web console, navigate to OperatorHub and then search for Snyk to install the Snyk Operator.
* Double-check installation was successful from the Installed Operators area:
* Now, from the Subscription tab, create an Operator Subscription for the Snyk controller. 1. Select "A specfic namespace on the cluster" or leave the default “All namespaces on the cluster” based on your needs.

  **Tip**

  Cluster scope is the default scope and we recommend you use this when installing the namespace so that we can scan the entire cluster. You can choose Namespaced scope, in which case the Snyk controller will watch for workloads only in the namespace in which it is deployed!

  **Note**

  Snyk always uses a stable channel when scanning your workloads

  1. Leave the remaining default configurations as they are.
  2. Click Subscribe to make the Operator available to the namespaces on this OpenShift Container Platform cluster.

* Now, create an instance of the Snyk Monitor. From the Snyk Monitor custom resource, click Create instance.
* Double-check successful installation from the cluster:
* After successfully installing the Snyk Operator and the instance of a Snyk Monitor, you can also view your cluster in Snyk.
