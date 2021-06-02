---
layout: default
title: Logstash
nav_order: 55
has_children: false
has_toc: false
---

# Logstash

Logstash compatibility is complicated.


## Logstash with OpenSearch

Logstash contains a version check that causes it to fail to connect to OpenSearch clusters. Download an OpenSearch-compatible binary here and use the following output configuration for the `opensearch` output plugin:

```conf
output {
  opensearch {
    hosts => ["localhost:9200"]
    index => "logstash-index-test"
    user => "admin"
    password => "admin"
    ssl => true
    cacert => "/full/path/to/root-ca.pem"
  }
}
```


## Logstash with Elasticsearch OSS

Logstash versions beyond 7.12.1 contain a license check that causes it to fail to connect to Elasticsearch OSS (7.10.2 and earlier) and Open Distro for Elasticsearch (1.13.2 and earlier).

To connect Logstash 7.12.1 OSS to Elasticsearch OSS, use the following output configuration:

```conf
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "logstash-index-test"
    ilm_enabled => false
  }
}
```

To connect Logstash 7.12.1 OSS to Open Distro for Elasticsearch, use the following output configuration:

```conf
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "logstash-index-test"
    user => "admin"
    password => "admin"
    ssl => true
    cacert => "/full/path/to/root-ca.pem"
    ilm_enabled => false
  }
}
```
