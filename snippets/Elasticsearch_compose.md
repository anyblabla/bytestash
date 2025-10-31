# Elasticsearch compose

Fichier "docker-compose.yml" à utiliser pour déployer Elasticsearch via Docker.

• docker
• compose
• yml
• elasticsearch
• search
• research

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  es:
    restart: always
    image: docker.elastic.co/elasticsearch/elasticsearch:8.18.0
    container_name: elasticsearch
    environment:
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m -Des.enforce.bootstrap.checks=true"
      - "xpack.license.self_generated.type=basic"
      - "xpack.security.enabled=false"
      - "xpack.watcher.enabled=false"
      - "xpack.graph.enabled=false"
      - "xpack.ml.enabled=false"
      #- "bootstrap.memory_lock=true"
      - "cluster.name=es-full-lxc-vm"
      - "discovery.type=single-node"
      - "thread_pool.write.queue_size=1000"
    networks:
      - external_network
      - internal_network
    healthcheck:
      test: ["CMD-SHELL", "curl --silent --fail localhost:9200/_cluster/health || exit 1"]
    volumes:
      - ./elasticsearch:/usr/share/elasticsearch/data
    #ulimits:
      #memlock:
        #soft: -1
        #hard: -1
      #nofile:
        #soft: 65536
        #hard: 65536
    ports:
      - '0.0.0.0:9200:9200'

networks:
  external_network:
  internal_network:
    internal: true
```
