DC/OS Spark-Build Testing Instructions
======================================

Pre-Requisits
-------------

- Refer `Spark-Build Instructions Pre-Requsits`_

  .. _Spark-Build Instructions Pre-Requsits: https://github.com/farhan5900/docs/blob/master/dcos_spark_build_instructions.rst#pre-requisits


Build Spark Tests
-----------------

Run the tests:

  .. code-block:: bash

    git clone https://github.com/mesosphere/spark-build.git
    git clone https://github.com/mesosphere/spark.git
    cd spark-build
    sudo -E ./publish_local_spark.sh --spark-dist-dir spark --docker-dist-image <docker-hub-user>/spark-dev:<image-tag> #To generate universe stub
    export STUB_UNIVERSE_URL=<stub-url>
    export DOCKER_DIST_IMAGE=<docker-hub-user>/spark-dev:<image-tag>
    export CLUSTER_URL=<cluster_url>   # To point to an existing cluster
    export PYTEST_ARGS="-k <test_name>"       # Select the specific test to run
    export TEST_SH_DCOS_SPARK_TEST_JAR_URL=<s3_test_jar_url>
    sudo -E ./test.sh
