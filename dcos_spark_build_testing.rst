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
    cd spark-build
    export DOCKER_DIST_IMAGE=<docker-hub-user>/spark-dev:<image-tag>
    export CLUSTER_URL=<cluster_url>   # To point to an existing cluster
    PYTEST_ARGS="-k <test_name>"       # Select the specific test to run
    sudo -E make test
