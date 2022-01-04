# Ansible playbooks for secure multi-cloud resource access configuration

This repo contains ansible playbooks to config cloud IAM for testing multi-cloud resource secure access without explicitly configuring target cloud credentials in the applications.

## Config AWS workloads to access GCP

### Prerequisites

* Install ansible modules

  ```shell
  ansible-galaxy collection install amazon.aws community.aws google.cloud
  ```

* Config environment variables for AWS and GCP credentials

  ```shell
  # AWS
  export AWS_ACCESS_KEY_ID=<ACCESS_KEY_ID>
  export AWS_SECRET_ACCESS_KEY=<SECRET_ACCESS_KEY>
  export AWS_REGION=<REGION_NAME>
  # GCP
  export GCP_AUTH_KIND=serviceaccount
  export GCP_SERVICE_ACCOUNT_FILE=<ACCOUNT_KEY_FILE>
  export GCP_PROJECT=<PROJECT_NAME>
  ```

### Run ansible playbook

* Change the values of the variables defined in `config_for_aws.yaml`. By default, the playbook will create new resources (VM and IAM role) in AWS for testing purpose. If you already have an AWS VM running, set the `aws_vm_id` and ignore the VM settings(image, security group, instance_type, etc.) below, make sure the existing VM is accessible via SSH using the defined private SSH key and set the `aws_role` associated with the VM.
* The playbook will create a new GCP service account with roles defined in `gcp_sa_project_roles`.
* Run the ansible playbook.

  ```shell
  ansible-playbook config_for_aws.yaml
  ```

### Cleanup

* Run the ansible playbook.

  ```shell
  ansible-playbook config_for_aws_cleanup.yaml
  ```

## Config GCP workloads to access AWS

### Prerequisites

* Install ansible modules

  ```shell
  ansible-galaxy collection install amazon.aws community.aws google.cloud
  ```

* Config environment variables for AWS and GCP credentials

  ```shell
  # AWS
  export AWS_ACCESS_KEY_ID=<ACCESS_KEY_ID>
  export AWS_SECRET_ACCESS_KEY=<SECRET_ACCESS_KEY>
  export AWS_REGION=<REGION_NAME>
  # GCP
  export GCP_AUTH_KIND=serviceaccount
  export GCP_SERVICE_ACCOUNT_FILE=<ACCOUNT_KEY_FILE>
  export GCP_PROJECT=<PROJECT_NAME>
  ```

### Run ansible playbook

* Change the values of the variables defined in `config_for_gcp.yaml`. By default, the playbook assumes there is a service account available in GCP (defined in `gcp_sa_email` and `gcp_sa_id`). The playbook will create new resources (disk, IP address, VM) in GCP for testing purpose. If you already have a GCP VM running, set the `gcp_existing_vm` and make sure the VM is accessible via SSH using the defined private SSH key and associated with the existing service account.
* The playbook will create a new AWS IAM role with policies defined in `aws_role_managed_policies`.
* Run the ansible playbook.

  ```shell
  ansible-playbook config_for_gcp.yaml
  ```

### Cleanup

* Run the ansible playbook.

  ```shell
  ansible-playbook config_for_gcp_cleanup.yaml
  ```