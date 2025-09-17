# install-operator

This role provides a standardized way to install and manage operators from the Red Hat Operators catalog in OpenShift clusters. 

It handles the complete lifecycle of operator installation, including namespace creation, operator group configuration, subscription management, and deployment verification.

## Purpose

The role is designed to:
- Simplify operator installation in OpenShift environments
- Provide consistent operator deployment patterns
- Handle operator lifecycle management (install/uninstall)
- Support both cluster-wide and namespace-scoped operator installations
- Automatically verify operator deployment status

## Parameters

### Required Parameters
- `operator_subscription_name`: The name of the subscription to install
- `operator_name`: The name of the operator package to install

### Optional Parameters
- `operator_deployment_name`: The name of the deployment to wait for (if not specified, will be automatically detected from CSV)
- `operator_namespace`: The namespace to install the operator in
- `operator_group_name`: The name of the OperatorGroup to create (no default - must be specified if `create_operator_group` is true)
- `wait_for_operator`: Whether to wait for the operator to be ready
- `create_operator_namespace`: Whether to create the operator namespace if it doesn't exist
- `create_operator_group`: Whether to create an OperatorGroup for the operator
- `manage_all_namespaces`: Whether to configure the operator to manage resources across all namespaces
- `subscription_channel`: The channel to subscribe to
- `subscription_starting_csv`: The starting CSV to subscribe to (leave empty for latest)
- `install_plan_approval`: The approval mode for the subscription (Automatic/Manual)
- `source`: The source of the subscription
- `source_namespace`: The namespace of the source

### System Parameters
- `local_kubeconfig_file`: Path to the kubeconfig file for cluster access
- `ACTION`: The action to perform (INSTALL/UPDATE/UNINSTALL) - UPDATE is treated the same as INSTALL
- `become_override`: Override the become behavior for the role
- `silent`: Silent mode flag (referenced in main task file)

## Default values

```yaml
# installation defaults
operator_namespace: "openshift-operators"
wait_for_operator: true
create_operator_namespace: true
create_operator_group: false
manage_all_namespaces: true

# subscription defaults
subscription_channel: "latest"
subscription_starting_csv: ""
install_plan_approval: "Automatic"
source: "redhat-operators"
source_namespace: "openshift-marketplace"
```

## Usage Examples

### Basic Installation
```yaml
- name: Install an operator
  include_role:
    name: install-operator
  vars:
    operator_subscription_name: "my-operator"
    operator_name: "my-operator"
    operator_namespace: "my-namespace"
```

### Advanced Installation with Custom Configuration
```yaml
- name: Install operator with custom configuration
  include_role:
    name: install-operator
  vars:
    operator_subscription_name: "my-operator"
    operator_name: "my-operator"
    operator_namespace: "my-namespace"
    create_operator_group: true
    operator_group_name: "my-operator-group"
    manage_all_namespaces: false
    subscription_channel: "stable"
    install_plan_approval: "Manual"
    wait_for_operator: true
```

### Uninstallation
```yaml
- name: Uninstall operator
  include_role:
    name: install-operator
  vars:
    ACTION: "UNINSTALL"
    operator_subscription_name: "my-operator"
    operator_namespace: "my-namespace"
```

## Notes

- The role requires cluster-admin privileges for installation in the `openshift-operators` namespace
- For namespace-scoped installations, appropriate RBAC permissions are required
- The role uses the `k8s` and `k8s_info` modules for all Kubernetes operations
- Operator uninstallation may not remove all custom resources created by the operator
- The role includes retry logic for deployment verification (60 retries with 10-second delays)
- When `create_operator_group` is true, `operator_group_name` must be specified
- The role automatically detects deployment names from the CSV if `operator_deployment_name` is not provided
- Subscription and CSV retrieval includes retry logic (30 retries with 10-second delays)
- UPDATE action is treated identically to INSTALL action
- The subscription template conditionally includes `startingCSV` only when `subscription_starting_csv` is not empty (template has been fixed to handle this properly)
