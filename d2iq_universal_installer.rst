D2IQ DC/OS Cluster Using Terraform
==================================

Prerequisites
-------------

**NOTE: If you have already setup all these tools,
just check the version and make sure it matches with
the supported version or it is latest.**

- **Install maws**

  .. code-block:: bash

    wget https://github.com/mesosphere/maws/releases/download/0.1.6/maws-linux
    chmod +x maws-linux
    sudo mv maws-linux /usr/local/bin/maws
    which maws

    echo 'source <(maws completion bash)' >> ~/.bashrc
    source ~/.bashrc

    maws ls  #It gives account name
    eval $(maws li XXXXXXXXXX_Mesosphere-PowerUser)
    maws li -r XXXXXXXXXX_Mesosphere-PowerUser &
    export AWS_PROFILE=XXXXXXXXXX_Mesosphere-PowerUser

- **Setup Terraform**

  .. code-block:: bash

    wget https://releases.hashicorp.com/terraform/0.11.14/terraform_0.11.14_linux_amd64.zip
    unzip terraform_0.11.14_linux_amd64.zip
    sudo mv terraform /usr/local/bin/terraform
    terraform version

- **Setup SSH**

  .. code-block:: bash

    sudo apt-get install openssh-server
    ssh -V

    ssh-keygen -t rsa  # Skip if already has key generated
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_rsa


DC/OS Cluster
----------------

- **Create License File**

Create a file containing license key.

  .. code-block:: bash

    vi license.txt
    # Put license content `LS0tLS1CRUdJ...`

- **Create Terraform Config File**

Create a file namely `main.tf` containing following content
as terraform configuration for the cluster.

  .. code-block:: terraform

    provider "aws" {
        # Change your default region here
        region = "us-east-1"
    }

    # Used to determine your public IP for forwarding rules
    data "http" "whatismyip" {
        url = "http://whatismyip.akamai.com/"
    }

    module "dcos" {
        source  = "dcos-terraform/dcos/aws"
        version = "~> 0.2.0"

        providers = {
            aws = "aws"
        }

        cluster_name        = "my-dcos-demo"
        ssh_public_key_file = "~/.ssh/id_rsa.pub"
        admin_ips           = ["${data.http.whatismyip.body}/32"]

        num_masters        = 3
        num_private_agents = 2
        num_public_agents  = 1

        dcos_version = "1.13.3"

        dcos_variant              = "ee"
        dcos_license_key_contents = "${file("./license.txt")}"
        # Make sure to set your credentials if you do not want the default EE
        # dcos_superuser_username      = "superuser-name"
        # dcos_superuser_password_hash = "${file("./dcos_superuser_password_hash.sha512")}"
        # dcos_variant = "open"

        dcos_instance_os             = "centos_7.5"
        bootstrap_instance_type      = "t2.medium"
        masters_instance_type        = "t2.medium"
        private_agents_instance_type = "t2.medium"
        public_agents_instance_type  = "t2.medium"
    }

    output "masters-ips" {
        value = "${module.dcos.masters-ips}"
    }

    output "cluster-address" {
        value = "${module.dcos.masters-loadbalancer}"
    }

    output "public-agents-loadbalancer" {
        value = "${module.dcos.public-agents-loadbalancer}"
    }

**NOTE: Modify values as per your requirements.**


- **Start Creating Cluster**

  .. code-block:: bash

    terraform init -upgrade
    terraform plan -out=plan.out
    terraform apply plan.out

- **Changing Cluster**

Modify `main.tf` file as per your requirement and run following commands.

  .. code-block:: bash

    # To fetch latest version of the terraform module, you can skip it
    terraform get -update

    terraform plan -out=plan.out
    terraform apply plan.out

- **Deleting Cluster**

  .. code-block:: bash

    terraform destroy
