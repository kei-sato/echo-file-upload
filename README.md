https://echo.labstack.com/recipes/file-upload

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
