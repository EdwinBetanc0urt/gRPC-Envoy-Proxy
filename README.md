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

Run Docker for envoy proxy with follow command

```
docker run -d -p 8080:8080 --network=host envoy-proxy/envoy
```
### Terminal output
