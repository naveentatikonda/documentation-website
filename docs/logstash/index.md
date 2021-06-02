---
layout: default
title: Logstash
nav_order: 55
has_children: false
has_toc: false
---

# Logstash

Logstash OSS is a popular ingestion and transformation tool. Newer versions of Logstash have compatibility issues with existing Elasticsearch OSS and Open Distro for Elasticsearch clusters. Logstash also has general compatibility issues with OpenSearch.


## Logstash with OpenSearch

Logstash OSS contains a version check that causes it to fail to connect to OpenSearch clusters.

{% comment %}
Download an OpenSearch-compatible binary [here]() and use the following output configuration for the `opensearch` output plugin:

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
{% endcomment %}


## Logstash with Elasticsearch OSS

Logstash versions beyond 7.12.1 contain a license check that causes it to fail to connect to Elasticsearch OSS (7.10.2 and earlier) and Open Distro for Elasticsearch (1.13.2 and earlier).

To connect Logstash 7.12.1 OSS to Elasticsearch OSS, try the following output configuration:

```conf
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "logstash-index-test"
    ilm_enabled => false
  }
}
```

To connect Logstash 7.12.1 OSS to Open Distro for Elasticsearch, try the following output configuration, which works with the security plugin:

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

The `cacert` field is only required if you use self-signed certificates.
{: .note }
