```Service exposes an application running on a set of Pods as a network service. The set of Pods targeted by a Service is usually determined by a selector. The name of a Service object must be a valid DNS label name. A Service can map any incoming port to a targetPort. By default and for convenience, the targetPort is set to the same value as the port field.```

## TYPES OF SERVICES
```
ClusterIP: Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default ServiceType.

NodePort: Exposes the Service on each Node’s IP at a static port (the NodePort). A ClusterIP Service, to which the NodePort Service routes, is automatically created. You’ll be able to contact the NodePort Service, from outside the cluster, by requesting <NodeIP>:<NodePort>.

LoadBalancer: Exposes the Service externally using a cloud provider’s load balancer. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created.

ExternalName: Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record
with its value. No proxying of any kind is set up.
```
## In our Case we are using AWS LoadBalancer
# Defining Service Object and adding labels
```apiVersion: v1
kind: Service
metadata:
  name: aws-lb
  labels:
    app: test-1
```

## Setting ELB port and Target Port(container port)
```
spec:
  ports:
  - port: 80
    targetPort: 80

```
## Selecting pods to add in service
```
  selector:
    name: nginx-dev-pods

```
## Setting Service Type
```
type: LoadBalancer
```