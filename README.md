# Multi Elasticsearch On One Docker-compose File

An Ansible Role that installs Elasticsearch with HOT/Warm/Cold architecture by Docker engine.

Ansible role for 7.x Elasticsearch. Installing multi elasticsearch on one host.

## Usage

Create your Ansible playbook with your own tasks, and include the role elasticsearch.

The simplest configuration therefore consists of:

```yml
- name: Simple Example
  hosts: elastic
  roles:
    - role: elasticsearch
  vars:
    elasticsearch_discovery_seed_hosts: elastic:9301,elastic:9302,elastic:9303,elastic:9304,elastic:9305,elastic:9306
    elasticsearch_cluster_initial_master_nodes: elastic:9301,elastic:9302
    elasticsearch:
        nodes:
            es-master1:
            hostname: es-master1
            roles: [ master ]
            cluster_name: oss
            http_port: 9201 
            transport_tcp_port: 9301
            elasticsearch_extra_options: |  
                node.attr.box_type: master
            jvm_options: |
                -Xms2g
                -Xmx2g

            es-master2:
            hostname: es-master2
            roles: [ master ]
            cluster_name: oss
            http_port: 9202
            transport_tcp_port: 9302
            elasticsearch_extra_options: |  
                node.attr.box_type: master
            jvm_options: |
                -Xms2g
                -Xmx2g

            es-hot1:
            hostname: es-hot1
            roles: [ data,data_hot,ingest ]
            cluster_name: oss
            http_port: 9203
            transport_tcp_port: 9303
            elasticsearch_extra_options: |
                node.attr.box_type: hot
                indices.requests.cache.size: 20%
                indices.query.bool.max_clause_count: 10240
                search.max_buckets: 250000
                indices.breaker.fielddata.limit: 60%
                indices.queries.cache.size: 20%
                indices.recovery.max_bytes_per_sec: 125mb
            jvm_options: |
                -Xms2g
                -Xmx2g


            es-hot2:
            hostname: es-hot2
            roles: [ data,data_hot,ingest ]
            cluster_name: oss
            http_port: 9204
            transport_tcp_port: 9304
            elasticsearch_extra_options: |
                node.attr.box_type: hot
                indices.requests.cache.size: 20%
                indices.query.bool.max_clause_count: 10240
                search.max_buckets: 250000
                indices.breaker.fielddata.limit: 60%
                indices.queries.cache.size: 20%
                indices.recovery.max_bytes_per_sec: 125mb
            jvm_options: |
                -Xms2g
                -Xmx2g

            es-warm1:
            hostname: es-warm1
            roles: [ data,data_warm ]
            cluster_name: oss
            http_port: 9205
            transport_tcp_port: 9305
            elasticsearch_extra_options: |
                node.attr.box_type: warm
                indices.requests.cache.size: 20%
                indices.query.bool.max_clause_count: 10240
                search.max_buckets: 250000
                indices.breaker.fielddata.limit: 60%
                indices.queries.cache.size: 20%
                indices.recovery.max_bytes_per_sec: 125mb 
            jvm_options: |
                -Xms2g
                -Xmx2g

            es-cold1:
            hostname: es-cold1
            roles: [ data,data_cold ]
            cluster_name: oss
            http_port: 9206
            transport_tcp_port: 9306
            elasticsearch_extra_options: |
                node.attr.box_type: cold
                indices.requests.cache.size: 20%
                indices.query.bool.max_clause_count: 10240
                search.max_buckets: 250000
                indices.breaker.fielddata.limit: 60%
                indices.queries.cache.size: 20%
                indices.recovery.max_bytes_per_sec: 125mb
            jvm_options: |
                -Xms2g
                -Xmx2g
```        