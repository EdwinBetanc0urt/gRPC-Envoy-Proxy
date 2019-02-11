# gRPC Envoy Proxy
Envoy proxy for gRPC as Example

## Requirements
- [Docker Compose](https://docs.docker.com/compose/)
- [Docker](https://docs.docker.com/)

### Running Envoy proxy for it:
Go to YouTube-Uploader-Server and run follow command (This command bluid a dockerfile from envoy.yml):

```
docker build -t envoy-proxy/envoy -f ./envoy.Dockerfile .
```
Terminal output
```
Sending build context to Docker daemon  113.7kB
Step 1/3 : FROM envoyproxy/envoy:latest
 ---> 1adc524bb108
Step 2/3 : COPY ./envoy.yaml /etc/envoy/envoy.yaml
 ---> 2bc1aa4f0a14
Step 3/3 : CMD /usr/local/bin/envoy -c /etc/envoy/envoy.yaml
 ---> Running in e2f6746c361f
Removing intermediate container e2f6746c361f
 ---> 3f3b15e47e32
Successfully built 3f3b15e47e32
Successfully tagged envoy-proxy/envoy:latest
```

Run Docker for envoy proxy with follow command

```
docker run -d -p 8080:8080 --network=host envoy-proxy/envoy
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
