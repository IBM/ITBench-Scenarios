# Remote Cluster Setup

__Note: The following setup guide has been verified and tested on MacOS, Ubuntu, and Fedora using the perscribed details.__

_Note: The following guide has been largely based on this [blog](https://aws.amazon.com/blogs/compute/kubernetes-clusters-aws-kops/)._

_Note: The following setup guide presumes that the required software listed [here](../README.md#required-software) has been installed along with [creating the virtual environment and installing the dependencies](../README.md#installing-dependencies). If it has not, please go back and do so before following this document._

## Recommended Software

### MacOS

- [Homebrew](https://brew.sh/)

## Required Software

- [awscli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) (v2)
- [kops](https://kops.sigs.k8s.io/getting_started/install/)

### Installing Required Software via Homebrew (for MacOS)

1. Install required software
```bash
brew install awscli
brew install kops
```

### Installing Required Software (for Red Hat Enterprise Linux -- RHEL)

1. Install the AWS CLI v2, curl, and jq by running:
```bash
sudo dnf install awscli
sudo dnf install curl
sudo dnf install jq
```
2. Set up kops by following the instructions [here](https://kops.sigs.k8s.io/getting_started/install/#linux)


## First Time Setup

1. Install Python dependencies. (Working directory is `remote_cluster`.)
```bash
python -m pip install -r requirements-dev.txt
```

2. Create the group variables for the development host. The `kops_cluster.yaml` file contains the configuration needed to customize the kops deployment.
```bash
make group_vars
vim group_vars/development/kops_cluster.yaml
```

3. Set up AWS credentials by running the following command. Enter the AWS access key ID and security access key when requested.
```bash
aws configure
```

## Cluster Management

1. Run the following command to create a cluster using EC2 resources. If you already have a cluster, you can skip this step. The configuration used for creating the cluster is specified in the `kops_cluster` group variables.
```bash
make create_kops_cluster
```

2. Update the value of the `kubeconfig` key in the `../group_vars/all.yaml`, with the absolute path that the kubeconfig should be downloaded to
```shell
vim ../group_vars/all.yaml
```

```yaml
kubeconfig: "<path to kubeconfig>"
```

3. Run the following command to download the kubeconfig for the created cluster. Changing the value of the `cluster.name_prefix` to the prefix of an already created cluster allows the command to export that kubeconfig instead.
```bash
make export_kops_kubeconfig
```

4. Access remote k8s cluster
```bash
export KUBECONFIG=<path to downloaded yaml>
kubectl get pod --all-namespaces
```

5. Now let's head back to the [parent README](../README.md) to deploy the incidents.

6. Once done with the experiment runs, to destroy the cluster, run the following command:
```bash
make destroy_kops_cluster
```

_Note_: For a full list of `make` commands, run the following command:
```bash
make help
```

## FAQ

### How do I access the observability stack's frontends?

Once you have deployed the observability stack, run the following command to find the Ingress address for deployed frontends:

```bash
kubectl get ingress -A
```

To access the Opencost dashboard, copy the address for the `opencost-ingress` ingress resource from the terminal and paste it into the browser.

To access the Jaeger dashboard, copy the address for the `jaeger` ingress resource from the terminal and paste it into the browser with the following prefix: `/jaeger`.

To access the Prometheus dashboard, copy the address for the `prometheus` ingress resource from the terminal and paste it into the browser with the following prefix: `/prometheus/query`.


```console
http://<jaeger address>/jaeger
http://<opencost-ingress address>
http://<prometheus address>/prometheus/query
```
