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
openssl req -new -key cert.key -out cert.csr -subj "/CN=www.example.com"
openssl x509 -req -sha256 -days 3650 -CA ca.crt -CAkey ca.key -CAcreateserial -in cert.csr -out cert.crt

: remove useless file
rm cert.csr
```
