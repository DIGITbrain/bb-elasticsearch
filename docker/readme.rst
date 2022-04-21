=============
Elasticsearch
=============

About
=====
**Elasticsearch** [1]_ is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases. 

Version
-------
Elasticsearch version **8.1.2** deployed based on the official Docker Hub image: [2]_. 

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
.. code-block:: bash

  docker run -d --rm \
      --name elastic \
      -e "discovery.type=single-node" \
      -p 9300:9300 \
      elastic:8.1.2 

where TODO ...,
standard Elasticsearch port 9300 is opened on the host, and SSL is turned on.


Security
========
TODO: The image uses **SSL/TLS traffic encryption** and **username-password authentication**, by
default using a DIGITbrain server certificate signed by DIGITbrain CA. You can override these certificates with your own,
see *volumes* parameters below.


Configuration
=============

TODO:

Environment variables
---------------------
.. list-table:: 
   :header-rows: 1

   * - Name
     - Example
     - Comment
   * - *Disovery type*
     - ``-e discovery.type=single-node``
     - TODO 

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
    - ``-v $PWD/certificates/ca.pem:?``  
    - Overrides Certificate Authority (CA) certificate
  * - *Server key*    
    - ``-v $PWD/certificates/server-key.pem:?``  
    - Overrides server key
  * - *Server certificate*    
    - ``-v $PWD/certificates/server-cert.pem:?``  
    - Overrides server certificate

References
==========
.. [1] https://www.elastic.co/elasticsearch/

.. [2] https://hub.docker.com/_/elasticsearch

.. [3] https://github.com/elastic/elasticsearch/blob/master/licenses/ELASTIC-LICENSE-2.0.txt

