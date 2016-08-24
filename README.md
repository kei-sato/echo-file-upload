https://echo.labstack.com/recipes/file-upload

# certificates

https://gist.github.com/denji/12b3a568f092ab951456
http://inaz2.hatenablog.com/entry/2015/01/28/230418

```
mkdir cert && cd $_

: create ca
openssl req -x509 -sha256 -nodes -days 3650 -newkey rsa:2048 -keyout ca.key -out ca.crt -subj "/CN=my private CA"

: create certificate with a sign of the ca
openssl genrsa -out cert.key 2048
openssl req -new -key cert.key -out cert.csr -subj "/CN=localhost"
openssl x509 -req -sha256 -days 3650 -CA ca.crt -CAkey ca.key -CAcreateserial -in cert.csr -out cert.crt

: create client certificate
openssl genrsa -out client.key 2048
openssl req -new -key client.key -out client.csr -subj "/CN=localhost"
openssl x509 -req -sha256 -days 3650 -CA ca.crt -CAkey ca.key -CAcreateserial -in client.csr -out client.crt

: remove useless file
rm *.csr
```

# text connection with openssl 

```
openssl s_client -connect localhost:8080 -cert client.crt -key client.key < /dev/null
```

# build

```
: compile for multiple platforms
go get -u github.com/mitchellh/gox
gox -output "pkg/{{.OS}}_{{.Arch}}/{{.Dir}}"

: recreate build directory
rm -rf dist && mkdir $_ && cp -a public $_

: if target machine is 64-bit linux
cp pkg/linux_amd64/echo-file-upload dist
```

# run with docker

```
docker run --rm -it -p 8080:8080 -v $PWD/dist:/usr/share/app --name ubuntu ubuntu bash -c 'cd /usr/share/app && ./echo-file-upload'
curl -i $(docker-machine ip):8080/
```
