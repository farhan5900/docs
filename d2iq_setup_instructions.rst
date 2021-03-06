D2IQ Setup Instructions
=======================

- **Install Java 8**

  .. code-block:: bash

    sudo apt-get install openjdk-8-jdk
    java -version

- **Install Go**

  .. code-block:: bash

    wget https://dl.google.com/go/go1.12.linux-amd64.tar.gz
    sudo tar -C /usr/local -xzf go1.12.linux-amd64.tar.gz
    sudo ln -s /usr/local/go/bin/go /usr/bin/go
    go version

- **Install R**

  .. code-block:: bash

    sudo apt-get install r-base
    R --version

- **Install SBT**

  .. code-block:: bash

    echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823
    sudo apt-get update
    sudo apt-get install sbt

- **Install Make**

  .. code-block:: bash

    sudo apt-get install build-essential
    make --version

- **Install git**

  .. code-block:: bash

    sudo apt-get install git
    git --version

- **Install Docker**

  .. code-block:: bash

    sudo apt-get install docker.io
    docker --version

- **Install awscli**

  .. code-block:: bash

    sudo apt-get install awscli
    aws --version

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

- **Setup Docker Hub Account**

  .. code-block:: bash

    sudo docker login

- **Setup DCOS CLI**

  .. code-block:: bash

    # Visit https://github.com/dcos/dcos-cli/releases for the latest link
    wget https://downloads.dcos.io/binaries/cli/linux/x86-64/1.0.0/dcos
    chmod +x dcos
    sudo mv dcos /usr/local/bin/dcos

- **Setup kubectl**

  .. code-block:: bash

    # Visit this for more info: https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux
    curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin/kubectl
    kubectl version

- **Setup konvoy**

  .. code-block:: bash

    wget https://github.com/mesosphere/konvoy/releases/download/v1.2.2/konvoy_v1.2.2_linux.tar.bz2
    tar -xvf konvoy_v1.2.2_linux.tar.bz2
    sudo cp konvoy_v1.2.2/* /usr/local/bin/
    source <(konvoy completion bash)

- **Setup brew**

  .. code-block:: bash

    sudo apt-get install linuxbrew-wrapper

- **Setup kudo-cli**

  .. code-block:: bash

    brew tap kudobuilder/tap
    brew install kudo-cli
    # To upgrade
    brew upgrade kudo-cli

    # OR use this link to directly download: https://github.com/kudobuilder/kudo/releases
    wget https://github.com/kudobuilder/kudo/releases/download/v0.7.5/kubectl-kudo_0.7.5_linux_x86_64
    chmod +x kubectl-kudo_0.7.5_linux_x86_64
    sudo mv kubectl-kudo_0.7.5_linux_x86_64 /usr/local/bin/kubectl-kudo

    # OR
    VERSION=x.y.z # look up the current stable release at https://github.com/kudobuilder/kudo/releases/latest
    OS=$(uname | tr '[:upper:]' '[:lower:]')
    ARCH=$(uname -m)
    wget -O kubectl-kudo https://github.com/kudobuilder/kudo/releases/download/v${VERSION}/kubectl-kudo_${VERSION}_${OS}_${ARCH}
    chmod +x kubectl-kudo
    # add to your path
    sudo mv kubectl-kudo /usr/local/bin/kubectl-kudo

- **Setup Git Bash Prompt**

  .. code-block:: bash

    git clone https://github.com/magicmonty/bash-git-prompt.git ~/.bash-git-prompt --depth=1
    # Add to the ~/.bashrc
    if [ -f "$HOME/.bash-git-prompt/gitprompt.sh" ]; then
        GIT_PROMPT_ONLY_IN_REPO=1
        source $HOME/.bash-git-prompt/gitprompt.sh
    fi

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

- **Setup GitHub SSH Key**

  .. code-block:: bash

    ssh-keygen -t rsa  # Skip if already has key generated
    sudo apt-get install xclip
    xclip -sel clip < ~/.ssh/id_rsa.pub

  Go to GitHub settings and paste the key.

- **Setup Slack/VSCode**

  .. code-block:: bash

    sudo snap install --classic slack
    sudo snap install --classic code
