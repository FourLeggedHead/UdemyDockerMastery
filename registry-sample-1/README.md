# Commands for setting up a local registry

- enable insecure registries in Daemon

## Linux

- Enable SSL
mkdir -p certs 
openssl req -newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key -x509 -days 365 -out certs/domain.crt

docker run --rm -e COMMON_NAME=127.0.0.1 -e KEY_NAME=registry -v $(pwd)/certs:/certs centurylink/openssl

## Windows

- Create self-signed certificate
mkdir certs
openssl req -newkey rsa:4096 -nodes -sha256 -keyout certs/fourleggedhead.key -x509 -days 365 -out certs/fourleggedhead.crt

- Spin up secured registry container
mkdir registry
docker run -d -p 5000:5000 --name local.registry.fourleggedhead --restart unless-stopped -v D:/localhub/certs:/certs -v D:/localhub/registry:/var/lib/registry -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/fourleggedhead.crt -e REGISTRY_HTTP_TLS_KEY=/certs/fourleggedhead.key registry

docker push hub.docker.fourleggedhead:5000/hello-world

- Setup basic authentication
mkdir auth
docker run --entrypoint htpasswd registry:latest -Bbn moby gordon > auth/htpasswd
docker run -d -p 5000:5000 --name local.registry.fourleggedhead --restart unless-stopped -v D:/localhub/registry-data:/var/lib/registry -v D:/localhub/certs:/certs  -v D:/localhub/auth:/auth -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key -e REGISTRY_AUTH=htpasswd -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" -e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" registry

## References

<https://www.codeproject.com/Articles/1263831/How-to-secure-your-private-Docker-Registry>
