# Makefile to run Ansible playbooks

.PHONY: help
help: ## Display this help.
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make \033[36m<target>\033[0m\n"} /^[a-zA-Z_0-9-]+:.*?##/ { printf "  \033[36m%-24s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)

.PHONY: group_vars
group_vars: ## Generates the group variables
	mkdir group_vars/all
	echo "ansible_python_interpreter: $(shell which python)" > group_vars/all/ansible.yaml
	cp group_vars/development/awx_stack.yaml.example group_vars/development/awx_stack.yaml
	cp group_vars/development/kops_cluster.yaml.example group_vars/development/kops_cluster.yaml

.PHONY: create_kops_cluster
create_kops_cluster: ## Creates a single cluster created by kops
	ansible-playbook -i inventory.yaml playbooks/manage_kops_cluster.yaml --tags "create"

.PHONY: list_kops_clusters
list_kops_clusters: ## Lists all active clusters created by kops
	ansible-playbook -i inventory.yaml playbooks/manage_kops_cluster.yaml --tags "list"

.PHONY: export_kops_kubeconfig
export_kops_kubeconfig: ## Exports the kubeconfig of a single cluster created by kops
	ansible-playbook -i inventory.yaml playbooks/manage_kops_cluster.yaml --tags "export"

.PHONY: destroy_kops_cluster
destroy_kops_cluster: ## Deletes a single cluster created by kops
	ansible-playbook -i inventory.yaml playbooks/manage_kops_cluster.yaml --tags "delete"

.PHONY: create_awx_stack
create_awx_stack: ## Creates an AWX stack created by kops
	ansible-playbook -i inventory.yaml playbooks/manage_awx_stack.yaml --tags "create"

.PHONY: destroy_awx_stack
destroy_awx_stack: ## Destroy an AWX stack created by kops
	ansible-playbook -i inventory.yaml playbooks/manage_awx_stack.yaml --tags "delete"
