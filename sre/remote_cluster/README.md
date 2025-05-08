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

2. Create `variables.yaml` from the example.
```bash
cp -n playbooks/variables.yaml.example playbooks/variables.yaml
```

5. Create `secret.yaml` (copy `secret.yaml.example`) and update the values. The `s3name` should be the cluster status s3 store (currently `sre-bench` for the `us-east-2` cluster and `sre-bench-de` for the `eu-central-1` cluster) as well as your AWS credentials. If you do not need access to the cluster nodes, then you can leave the ssh key field blank. Otherwise, create an ssh key and provide the absolute path to the public key.
```bash
cp -n playbooks/secret.yaml.example playbooks/secret.yaml
```
```yaml
ssh_key_for_cluster: "/home/<user>/.ssh/<key-name>.pub"
```

6. Set up AWS credentials by running the following command. Enter the AWS access key ID and security access key when requested.
```bash
aws configure
```

## Cluster Management

1. Run the following command to create a cluster using EC2 resources. If you already have a cluster, you can skip this step. When prompted, supply the number of control nodes, worker nodes, name of the cluster, and type of instance required for the cluster. Recommended: 1 control node, 3 worker nodes, and instance type of m4.xlarge.
```bash
make create
```

2. Update the value of the `kubeconfig` key in the `../group_vars/all.yaml`, with the absolute path that the kubeconfig should be downloaded to
```shell
vim ../group_vars/all.yaml
```

```yaml
kubeconfig: "<path to kubeconfig>"
```

3. Run the following command to download the kubeconfig of a selected cluster to the path specified by the `kubeconfig` in `all.yaml`. A cluster can be selected by typing the name of cluster into the prompt or pressing the `Return` key if the first entry in the table is what is desired.
```bash
make get_kcfg
```

4. Access remote k8s cluster
```bash
export KUBECONFIG=<path to downloaded yaml>
kubectl get pod --all-namespaces
```

5. Now let's head back to the [parent README](../README.md) to deploy the incidents.

6. Once done with the experiment runs, to destroy the cluster, run the following command:
```bash
make delete
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
