# gRPC Envoy Proxy
Envoy proxy for gRPC as Example

## Requirements
- [Docker Compose](https://docs.docker.com/compose/)
- [Docker](https://docs.docker.com/)

### Running Envoy proxy for it:
Go to YouTube-Uploader-Server and run follow command (This command bluid a dockerfile from envoy.yml):

```
docker build -t adempiere-grpc-proxy -f ./envoy.Dockerfile .
```
Terminal output
```
Sending build context to Docker daemon  7.168kB
Step 1/3 : FROM envoyproxy/envoy:latest
 ---> 20b550751ccf
Step 2/3 : COPY ./envoy.yaml /etc/envoy/envoy.yaml
 ---> 58ccb0ee49bd
Step 3/3 : CMD /usr/local/bin/envoy -c /etc/envoy/envoy.yaml
 ---> Running in d28779389feb
Removing intermediate container d28779389feb
 ---> e27ea7538138
Successfully built e27ea7538138
Successfully tagged adempiere-grpc-proxy:latest
```

Run Docker for envoy proxy with follow command

```
docker run -d --network=host adempiere-grpc-proxy
```
Terminal output

```
WARNING: Published ports are discarded when using host network mode
705d1e69c6d8ee38d9e69a4f99cf19ba566bc18e0d818ab0031bd9c89c400186
```
Verify if is Running
```
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
705d1e69c6d8        envoy-proxy/envoy   "/docker-entrypoint.…"   5 seconds ago       Up 4 seconds                            reverent_dijkstra
```
