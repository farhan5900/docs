DC/OS Spark-Build Instructions
==============================

Pre-Requisits
-------------

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


Build Spark
-----------

Run following command to build spark repo:

  .. code-block:: bash

    git clone https://github.com/mesosphere/spark-build.git
    cd spark-build
    git clone https://github.com/mesosphere/spark.git
    sudo ./publish_local_spark.sh --spark-dist-dir spark --docker-dist-image <docker-hub-user>/spark-dev:<image-tag>

Sometime it gives ``unable to fetch aws credential`` because of sudo use, then use following command:

  .. code-block:: bash

    sudo -E ./publish_local_spark.sh --spark-dist-dir spark --docker-dist-image <docker-hub-user>/spark-dev:<image-tag>

``publish_local_spark.sh`` will generate stub url.

Now we can install dcos cli and setup the cluster. After that, run following command to install spark service and subcommand cli as well.

  .. code-block:: bash

    dcos package install spark
    dcos package install spark --cli

