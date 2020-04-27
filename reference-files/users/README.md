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

## update kubeconfig file
```
kubectl --kubeconfig {{config-file}} config set-cluster {{cluster}} --server {{server}} --certificate-authority={{ca-cert-path}}

example-
kubectl --kubeconfig config config set-cluster k.gagan.tech --server https://api.k.gagan.tech --certificate-authority=ca.crt


kubectl --kubeconfig {{config-file}} config set-credentials {{user-name}} --client-certificate {{user-cert-path}} --client-key {{user-key-path}}

example-
kubectl --kubeconfig config config set-credentials user1 --client-certificate /root/.kube/user.cert --client-key /root/.kube/user.key


## add new context
```
kubectl config set-context {{context-name}} --cluster={{cluster}} --user user

example-
kubectl config set-context gagan-tech --cluster=k.gagan.tech --user gagan
```
