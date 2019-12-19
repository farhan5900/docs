DC/OS Spark-Build Instructions
==============================

Build Spark
-----------

Run following command to build spark repo:

  .. code-block:: bash

    git clone https://github.com/mesosphere/spark-build.git
    cd spark-build
    git clone -b custom-branch-2.4.x https://github.com/mesosphere/spark.git
    sudo ./publish_local_spark.sh --spark-dist-dir spark --docker-dist-image <docker-hub-user>/spark-dev:<image-tag>

Sometime it gives ``unable to fetch aws credential`` because of sudo use,
then use following command:

  .. code-block:: bash

    sudo -E ./publish_local_spark.sh --spark-dist-dir spark --docker-dist-image <docker-hub-user>/spark-dev:<image-tag>

``publish_local_spark.sh`` will generate stub url.

Now we can install dcos cli and setup the cluster.
After that, run following command to install spark
service and subcommand cli as well.

  .. code-block:: bash

    dcos package install spark
    dcos package install spark --cli

