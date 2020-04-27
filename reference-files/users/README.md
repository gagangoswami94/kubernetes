## Retrieve keys from kops
```
aws s3 sync s3://{{bucket}}/{{cluster}}/pki/private/ca/ ca-key
aws s3 sync s3://{{bucket}}/{{cluster}}/pki/issued/ca/ ca-crt
mv ca-key/*.key ca.key
mv ca-crt/*.crt ca.crt
```
## Create new user
```
sudo apt install openssl
openssl genrsa -out user.pem 2048
openssl req -new -key user.pem -out user.csr -subj "/CN=user/O=myteam/"
openssl x509 -req -in user.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out user.crt -days 10000
```

## add new context
```
kubectl config set-credentials user --client-certificate=user.crt --client-key=user.pem
kubectl config set-context user --cluster={{cluster}} --user user
```
