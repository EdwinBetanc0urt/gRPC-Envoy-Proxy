# gRPC Envoy Proxy
Envoy proxy for gRPC as Example

## Requirements
- [Docker Compose](https://docs.docker.com/compose/)
- [Docker](https://docs.docker.com/)

## Prepare YAML configuration file
The addresses to which the servers should point in the envoy.yaml file must be changed, in the section `cluster` > `load_assignment` > `lb_endpoints` > `endpoint` > `address` > `socket_address`. There you specify the address of the host and the port where it will be accessed:

```yaml
  address:
      socket_address:
        # replace the 'localhost' address with your access host address
        address: localhost

        # it is the default port, unless the server port is changed it is recommended not to change
        port_value: 50050
```

There are 4 clusters in total to Access (`access_cluster`), Dictionary (`dictionary_cluster`), Business Data (`business_data_cluster`) and Enrollment (`enrollment_cluster`), each with different ports pointed to the localhot address by default.

### Running Envoy proxy for it:
Go to gRPC-Envoy-Proxy and run follow command (This command bluid a dockerfile from envoy.yml):

```
docker build -t adempiere-grpc-proxy -f ./envoy.Dockerfile .
```

Terminal output
```
Sending build context to Docker daemon  238.1kB
Step 1/4 : FROM envoyproxy/envoy:v1.16.0
v1.16.0: Pulling from envoyproxy/envoy
171857c49d0f: Pull complete
419640447d26: Pull complete
61e52f862619: Pull complete
568f6e4d47db: Pull complete
c03261803dd9: Pull complete
fe52d4ba2edd: Pull complete
54e5dc37f029: Pull complete
1e918d808c1e: Pull complete
68a45b97a75d: Pull complete
46f21af565d3: Pull complete
Digest: sha256:9e72bbba48041223ccf79ba81754b1bd84a67c6a1db8a9dbff77ea6fc1cb04ea
Status: Downloaded newer image for envoyproxy/envoy:v1.16.0
 ---> a438abf4c3fd
Step 2/4 : LABEL maintainer="EdwinBetanc0urt@outlook.com"
 ---> Running in a0a163baeea7
Removing intermediate container a0a163baeea7
 ---> 45e63df5fe4a
Step 3/4 : COPY "./envoy.yaml" "./cert/localhost-cert.pem" "./cert/localhost-key.pem" "/etc/envoy/"
 ---> d7f32dfb2a93
Step 4/4 : CMD /usr/local/bin/envoy -c /etc/envoy/envoy.yaml
 ---> Running in 31693017aa2b
Removing intermediate container 31693017aa2b
 ---> 8e80364d9ed1
Successfully built 8e80364d9ed1
Successfully tagged adempiere-grpc-proxy:latest
```

Run Docker for envoy proxy with follow command
```
docker run -d -it \
  --name ADempiere-gRPC-Proxy \
  --network=host \
	-v $(pwd)/envoy.yaml:/etc/envoy/envoy.yaml \
  adempiere-grpc-proxy
```

Terminal output
```
5a5b60c256a75ace66dbaea37bf855945df2d784b2525b94bbd0f38c683d0b3f
```

Verify if is Running
```
docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                    NAMES
19066ca455ce        adempiere-grpc-proxy   "/docker-entrypoint.…"   5 seconds ago      Up 4 seconds                                ADempiere-gRPC-Proxy
```

### Running Envoy proxy with docker-compose:

Run Docker for envoy proxy with follow command
```shell
docker-compose up
```

Terminal output:
```
Building envoy-proxy-grpc
Step 1/4 : FROM envoyproxy/envoy:v1.16.0
v1.16.0: Pulling from envoyproxy/envoy
e92ed755c008: Pull complete
b9fd7cb1ff8f: Pull complete
ee690f2d57a1: Pull complete
53e3366ec435: Pull complete
d7c9afc40f7e: Pull complete
047e99158cd2: Pull complete
75ee43f2e4c4: Pull complete
0546561da879: Pull complete
539a2f535067: Pull complete
Digest: sha256:47a5ea1e6edb20fee16678670c29b489ebbf1187e411662056be7a4518ee6cdd
Status: Downloaded newer image for envoyproxy/envoy:v1.15.0
 ---> 9071be9bc679
Step 2/4 : LABEL maintainer="EdwinBetanc0urt@outlook.com"
 ---> Running in bfba7aa03eb3
Removing intermediate container bfba7aa03eb3
 ---> e0ee31e66cc3
Step 3/4 : COPY "./envoy.yaml" "./cert/localhost-cert.pem" "./cert/localhost-key.pem" "/etc/envoy/"
 ---> 714cd4d50049
Step 4/4 : CMD /usr/local/bin/envoy -c /etc/envoy/envoy.yaml
 ---> Running in 4b40a80bdeb9
Removing intermediate container 4b40a80bdeb9
 ---> a7892040ee4c

Successfully built a7892040ee4c
Successfully tagged grpc-envoy-proxy_envoy-proxy-grpc:latest
WARNING: Image for service envoy-proxy-grpc was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating Envoy-Proxy-gRPC ... done
Attaching to Envoy-Proxy-gRPC
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.484][6][info][main] [source/server/server.cc:255] initializing epoch 0 (hot restart version=11.104)
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:257] statically linked extensions:
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.filters.http: envoy.buffer, envoy.cors, envoy.csrf, envoy.ext_authz, envoy.fault, envoy.filters.http.adaptive_concurrency, envoy.filters.http.aws_lambda, envoy.filters.http.aws_request_signing, envoy.filters.http.buffer, envoy.filters.http.cache, envoy.filters.http.cors, envoy.filters.http.csrf, envoy.filters.http.dynamic_forward_proxy, envoy.filters.http.dynamo, envoy.filters.http.ext_authz, envoy.filters.http.fault, envoy.filters.http.grpc_http1_bridge, envoy.filters.http.grpc_http1_reverse_bridge, envoy.filters.http.grpc_json_transcoder, envoy.filters.http.grpc_stats, envoy.filters.http.grpc_web, envoy.filters.http.gzip, envoy.filters.http.header_to_metadata, envoy.filters.http.health_check, envoy.filters.http.ip_tagging, envoy.filters.http.jwt_authn, envoy.filters.http.lua, envoy.filters.http.on_demand, envoy.filters.http.original_src, envoy.filters.http.ratelimit, envoy.filters.http.rbac, envoy.filters.http.router, envoy.filters.http.squash, envoy.filters.http.tap, envoy.grpc_http1_bridge, envoy.grpc_json_transcoder, envoy.grpc_web, envoy.gzip, envoy.health_check, envoy.http_dynamo_filter, envoy.ip_tagging, envoy.lua, envoy.rate_limit, envoy.router, envoy.squash
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.grpc_credentials: envoy.grpc_credentials.aws_iam, envoy.grpc_credentials.default, envoy.grpc_credentials.file_based_metadata
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.filters.udp_listener: envoy.filters.udp.dns_filter, envoy.filters.udp_listener.udp_proxy
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   http_cache_factory: envoy.extensions.http.cache.simple
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.access_loggers: envoy.access_loggers.file, envoy.access_loggers.http_grpc, envoy.access_loggers.tcp_grpc, envoy.file_access_log, envoy.http_grpc_access_log, envoy.tcp_grpc_access_log
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.retry_priorities: envoy.retry_priorities.previous_priorities
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.udp_listeners: raw_udp_listener
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.dubbo_proxy.filters: envoy.filters.dubbo.router
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.thrift_proxy.transports: auto, framed, header, unframed
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.transport_sockets.upstream: envoy.transport_sockets.alts, envoy.transport_sockets.raw_buffer, envoy.transport_sockets.tap, envoy.transport_sockets.tls, raw_buffer, tls
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.resource_monitors: envoy.resource_monitors.fixed_heap, envoy.resource_monitors.injected_resource
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.filters.network: envoy.client_ssl_auth, envoy.echo, envoy.ext_authz, envoy.filters.network.client_ssl_auth, envoy.filters.network.direct_response, envoy.filters.network.dubbo_proxy, envoy.filters.network.echo, envoy.filters.network.ext_authz, envoy.filters.network.http_connection_manager, envoy.filters.network.kafka_broker, envoy.filters.network.local_ratelimit, envoy.filters.network.mongo_proxy, envoy.filters.network.mysql_proxy, envoy.filters.network.ratelimit, envoy.filters.network.rbac, envoy.filters.network.redis_proxy, envoy.filters.network.sni_cluster, envoy.filters.network.tcp_proxy, envoy.filters.network.thrift_proxy, envoy.filters.network.zookeeper_proxy, envoy.http_connection_manager, envoy.mongo_proxy, envoy.ratelimit, envoy.redis_proxy, envoy.tcp_proxy
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.stats_sinks: envoy.dog_statsd, envoy.metrics_service, envoy.stat_sinks.dog_statsd, envoy.stat_sinks.hystrix, envoy.stat_sinks.metrics_service, envoy.stat_sinks.statsd, envoy.statsd
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.thrift_proxy.protocols: auto, binary, binary/non-strict, compact, twitter
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.clusters: envoy.cluster.eds, envoy.cluster.logical_dns, envoy.cluster.original_dst, envoy.cluster.static, envoy.cluster.strict_dns, envoy.clusters.aggregate, envoy.clusters.dynamic_forward_proxy, envoy.clusters.redis
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.filters.listener: envoy.filters.listener.http_inspector, envoy.filters.listener.original_dst, envoy.filters.listener.original_src, envoy.filters.listener.proxy_protocol, envoy.filters.listener.tls_inspector, envoy.listener.http_inspector, envoy.listener.original_dst, envoy.listener.original_src, envoy.listener.proxy_protocol, envoy.listener.tls_inspector
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.dubbo_proxy.route_matchers: default
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.health_checkers: envoy.health_checkers.redis
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.tracers: envoy.dynamic.ot, envoy.lightstep, envoy.tracers.datadog, envoy.tracers.dynamic_ot, envoy.tracers.lightstep, envoy.tracers.opencensus, envoy.tracers.xray, envoy.tracers.zipkin, envoy.zipkin
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.transport_sockets.downstream: envoy.transport_sockets.alts, envoy.transport_sockets.raw_buffer, envoy.transport_sockets.tap, envoy.transport_sockets.tls, raw_buffer, tls
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.dubbo_proxy.protocols: dubbo
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.retry_host_predicates: envoy.retry_host_predicates.omit_canary_hosts, envoy.retry_host_predicates.omit_host_metadata, envoy.retry_host_predicates.previous_hosts
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.resolvers: envoy.ip
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.thrift_proxy.filters: envoy.filters.thrift.rate_limit, envoy.filters.thrift.router
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.494][6][info][main] [source/server/server.cc:259]   envoy.dubbo_proxy.serializers: dubbo.hessian2
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.508][6][info][main] [source/server/server.cc:340] admin address: 0.0.0.0:9901
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.509][6][info][main] [source/server/server.cc:459] runtime: layers:
Envoy-Proxy-gRPC    |   - name: base
Envoy-Proxy-gRPC    |     static_layer:
Envoy-Proxy-gRPC    |       {}
Envoy-Proxy-gRPC    |   - name: admin
Envoy-Proxy-gRPC    |     admin_layer:
Envoy-Proxy-gRPC    |       {}
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.548][6][info][config] [source/server/configuration_impl.cc:103] loading tracing configuration
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.548][6][info][config] [source/server/configuration_impl.cc:69] loading 0 static secret(s)
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.548][6][info][config] [source/server/configuration_impl.cc:75] loading 4 cluster(s)
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.553][6][info][upstream] [source/common/upstream/cluster_manager_impl.cc:171] cm init: all clusters initialized
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.553][6][info][config] [source/server/configuration_impl.cc:79] loading 1 listener(s)
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.563][6][info][config] [source/server/configuration_impl.cc:129] loading stats sink configuration
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.563][6][info][main] [source/server/server.cc:533] all clusters initialized. initializing init manager
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.563][6][info][config] [source/server/listener_manager_impl.cc:725] all dependencies initialized. starting workers
Envoy-Proxy-gRPC    | [2020-06-30 17:04:43.563][6][info][main] [source/server/server.cc:554] starting main dispatch loop
```


## Administration interface
Envoy exposes a local administration interface that can be used to query and modify different aspects of the server:

```yaml
admin:
  access_log_path: /tmp/admin_access.log
  address:
    socket_address: {
      address: 0.0.0.0,
      port_value: 9901
    }
```

See the following link for more information: https://www.envoyproxy.io/docs/envoy/latest/operations/admin
