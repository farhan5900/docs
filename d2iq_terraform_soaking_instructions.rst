D2IQ Soaking Instructions
=========================

Prerequisites
-------------

- **Install Go**

  .. code-block:: bash

    wget https://dl.google.com/go/go1.13.linux-amd64.tar.gz
    sudo tar -C /usr/local -xzf go1.13.linux-amd64.tar.gz
    sudo ln -s /usr/local/go/bin/go /usr/bin/go
    go version

- **Install git**

  .. code-block:: bash

    sudo apt-get install git
    git --version

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

- **Setup DCOS CLI**

  .. code-block:: bash

    # Visit https://github.com/dcos/dcos-cli/releases for the latest link
    wget https://downloads.dcos.io/binaries/cli/linux/x86-64/1.0.0/dcos
    chmod +x dcos
    sudo mv dcos /usr/local/bin/dcos

- **Setup Terraform**

  .. code-block:: bash

    wget https://releases.hashicorp.com/terraform/0.11.14/terraform_0.11.14_linux_amd64.zip
    unzip terraform_0.11.14_linux_amd64.zip
    sudo mv terraform /usr/local/bin/terraform

- **Setup Terraform DCOS Provider**

  .. code-block:: bash

    # Visit https://github.com/mesosphere/terraform-provider-dcos/releases for the latest release
    git clone https://github.com/mesosphere/terraform-provider-dcos.git
    cd terraform-provider-dcos
    git checkout <release-tag> # Use latest release tag or ignore if building from master
    make build_all # Only If building for all type of OS
    make build # If not work run below commands

    go get ./... # Only If getting dependency error
    export GO111MODULE=on
    go build -v -o terraform.d/plugins/linux_amd64/terraform-provider-dcos_0.0.1 # Replace 0.0.1 it with latest version
    cp terraform.d/plugins/linux_amd64/terraform-provider-dcos_0.0.1 ~/.terraform.d/plugins/linux_amd64/ # Create folder if not exists

- **Setup Terraform KDC Provider**

  .. code-block:: bash

    # Visit https://github.com/mesosphere/data-services-terraform/releases for the latest release
    git clone https://github.com/mesosphere/data-services-terraform.git
    cd data-services-terraform/providers/terraform-provider-kdc
    git checkout <release-tag> # Use latest release tag or ignore if building from master

    go get ./... # Only If getting dependency error
    go build -v -o terraform.d/plugins/linux_amd64/terraform-provider-kdc_0.0.1 # Replace 0.0.1 it with latest version, if no version remove _0.0.1
    cp terraform.d/plugins/linux_amd64/terraform-provider-kdc_0.0.1 ~/.terraform.d/plugins/linux_amd64/


Soaking
-------

- **Clone Repo and Make Changes**

  .. code-block:: bash

    git clone https://github.com/mesosphere/soak-cluster-configurations.git
    cd soak-cluster-configurations
    git checkout -b <new-working-branch>
    # According to the framework (suppose Cassandra) need to choose a file to make changes
    vi infinity/workload/cassandra.tf
    # Put Permanent Stub (suppose stub is: https://infinity-artifacts.s3.amazonaws.com/permanent/cassandra/assets/2.7.0-3.11.5-rc1/stub-universe-cassandra.json) in the file, changes should look as follows:
    ## resource "dcos_package_repo" "cassandra" {
    ##   name = "cassandra"
    ##   url  = "https://infinity-artifacts.s3.amazonaws.com/permanent/cassandra/assets/2.7.0-3.11.5-rc1/stub-universe-cassandra.json"
    ##   index = 0
    ## }
    ## data "dcos_package_version" "cassandra" {
    ##   repo_url = "${dcos_package_repo.cassandra.url}"
    ##   name = "cassandra"
    ##   ...
    ## }

    # Only If want to run `smallsoak112s`, do the same as above for this file as well:
    vi smallsoak112s/infinity/cassandra.tf

- **Start Soaking**

  .. code-block:: bash

    # Suppose running for soak cluster Soak200s
    dcos cluster setup https://soak200s.testing.mesosphe.re/
    cd infinity/workload
    terraform init
    terraform plan -out=plan.out
    # There will be some prompts for which values are as follows:
    # cluster_name     : soak200s
    # cluster_password : <password>
    # cluster_user     : centos
    # dcos_version     : 2.0.0
    # cusom_domain     : <cluster_cryptic_id>
    # For latest value of password/user_name/cluster_name visit this: soak.mesosphere.com
    # For cluster cryptic id visit the cluster and check cluster info
    terraform apply plan.out
