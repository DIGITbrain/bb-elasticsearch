=============
Elasticsearch
=============

About
=====
**Elasticsearch** [1]_ is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases.

Version
-------
Elasticsearch version **8.1.2** deployed based on the official Docker Hub image [2]_ and single node documentation [4]_.

License
-------
**Elastic License Vesrion 2.0** [3]_

Pre-requisites
==============
* *docker* installed
* access to DIGITbrain private docker repo (username, password) to pull the image:

  - ``docker login dbs-container-repo.emgora.eu``
  - ``docker pull dbs-container-repo.emgora.eu/elastic:8.1.2``

Usage
=====
The following steps are based on the original description for a single node instance [4]_.

1. Create a docker network for Elasticsearch:

  .. code-block:: bash

    docker network create elastic

2. Start Elasticsearch:

  .. code-block:: bash

    docker network create elastic

    docker run -d --rm \
        --name elastic \
        --net elastic \
        -e ES_JAVA_OPTS="-Xms1g -Xmx1g" \
        -e "node.name=elastic" \
        -e "discovery.type=single-node" \
        -e "ELASTIC_PASSWORD=<PASSWORD>" \
        -e "xpack.security.enabled=true" \
        -e "xpack.security.http.ssl.enabled=true" \
        -e "xpack.security.http.ssl.key=certs/server-key.pem" \
        -e "xpack.security.http.ssl.certificate=certs/server-cert.pem" \
        -e "xpack.security.http.ssl.certificate_authorities=certs/ca.pem" \
        -e "xpack.security.http.ssl.verification_mode=certificate" \
        -e "xpack.security.transport.ssl.enabled=true" \
        -e "xpack.security.transport.ssl.key=certs/server-key.pem" \
        -e "xpack.security.transport.ssl.certificate=certs/server-cert.pem" \
        -e "xpack.security.transport.ssl.certificate_authorities=certs/ca.pem" \
        -e "xpack.security.transport.ssl.verification_mode=certificate" \
        -e "xpack.license.self_generated.type=basic" \
        -p 9300:9300 \
        -p 9200:9200 \
        dbs-container-repo.emgora.eu/elasticsearch:8.1.2

3. If the `ELASTIC_PASSWORD` environment variable is not used, then a password is generated for the elastic user and output to the terminal. (Additionally, an enrollment for Kibana is also created.)

   If you need you can reset the password using the following command:

  .. code-block:: bash

    docker exec -it elastic /usr/share/elasticsearch/bin/elasticsearch-reset-password

4. Open a terminal and verify connection to your instance (replace `http_ca.crt` with your ca cert):

  .. code-block:: bash

    curl --cacert http_ca.crt -u elastic https://localhost:9200


The standard Elasticsearch port 9300 is opened on the host, and SSL is turned on.


Security
========
The image uses **SSL/TLS traffic encryption** and **password authentication**, by
default using a DIGITbrain server certificate signed by DIGITbrain CA. You can override these certificates with your own,
see *volumes* parameters below.


Configuration
=============


Environment variables
---------------------
.. list-table::
   :header-rows: 1

   * - Name
     - Example
     - Comment
   * - *Disovery type*
     - ``-e discovery.type=single-node``
     - Single node discovery for single node instance.
   * - *Setting Java heap*
     - ``-e ES_JAVA_OPTS="-Xms1g -Xmx1g"``
     - Set Java heap to 1g (replace ``1g`` with desired value).
   * - *Setting Elastic password*
     - ``-e "ELASTIC_PASSWORD=<PASSWORD>"``
     - Replace <PASSWORD> with the desired password.
   * - *Using auto-generated certificates*
     - Remove the``xpack.*`` enviroment variables from the docker run command.
     - In this case elastic will generate the certificates and keys.

Ports
-----
.. list-table::
  :header-rows: 1

  * - Container port
    - Host port bind example
    - Comment
  * - *9300*
    - ``-p 19300:9300``
    - Default Elasticsearch container port 9300 is opened as port 19300 on the host

Volumes
-------
.. list-table::
  :header-rows: 1

  * - Name
    - Volume mount example
    - Comment
  * - *Data*
    - ``-v $PWD/data:?``
    - Elasticsearch data will be persisted in host directory: ``./data``.
  * - *CA certificate*
    - ``-v $PWD/certificates/ca.pem:/usr/share/elasticsearch/config/certs/ca.pem``
    - Overrides Certificate Authority (CA) certificate
  * - *Server key*
    - ``-v $PWD/certificates/server-key.pem:/usr/share/elasticsearch/config/certs/server-key.pem``
    - Overrides server key
  * - *Server certificate*
    - ``-v $PWD/certificates/server-cert.pem:/usr/share/elasticsearch/config/certs/server-cert.pem``
    - Overrides server certificate

References
==========
.. [1] https://www.elastic.co/elasticsearch/

.. [2] https://hub.docker.com/_/elasticsearch

.. [3] https://github.com/elastic/elasticsearch/blob/master/licenses/ELASTIC-LICENSE-2.0.txt

.. [4] https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#docker-cli-run-dev-mode
