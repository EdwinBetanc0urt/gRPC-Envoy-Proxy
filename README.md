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
Sending build context to Docker daemon  120.3kB
Step 1/3 : FROM envoyproxy/envoy:latest
latest: Pulling from envoyproxy/envoy
e80174c8b43b: Pull complete
d1072db285cc: Pull complete
858453671e67: Pull complete
3d07b1124f98: Pull complete
990fbe3093cb: Pull complete
4da8686dffe3: Pull complete
b816b805e1d5: Pull complete
f3c50b037f2f: Pull complete
149d66218a49: Pull complete
Digest: sha256:80d260d17d39926e5d405713d59e2d98f0aa1b63936f54c9b471a2f39656b6e4
Status: Downloaded newer image for envoyproxy/envoy:latest
 ---> 56683ef746a8
Step 2/3 : COPY ./envoy.yaml /etc/envoy/envoy.yaml
 ---> e108a72bace1
Step 3/3 : CMD /usr/local/bin/envoy -c /etc/envoy/envoy.yaml
 ---> Running in b9e4509ab6d8
Removing intermediate container b9e4509ab6d8
 ---> e764a11e4c4a
Successfully built e764a11e4c4a
Successfully tagged adempiere-grpc-proxy:latest
```

Run Docker for envoy proxy with follow command
```
docker run -d --name ADempiere-gRPC-Proxy --network=host adempiere-grpc-proxy
```

Terminal output
```
26f674f5a8ceb86c0e26113c51e394b80fd08a4baf52ec9303aef239c2a481bf
```

Verify if is Running
```
docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                    NAMES
ce998664e7a7        adempiere-grpc-proxy   "/docker-entrypoint.…"   5 seconds ago      Up 4 seconds                                ADempiere-gRPC-Proxy
```
