# LoadBalanceing using ingress
There are some limitations when using kube-proxy to realize loadbalance among services, i.e. obsecured remote address of users, limited service ports, etc.Here simply describe how to create a simple ingress using nginx-ingress-controller to realize loadbalance.
- Prepare tls file
 https://github.com/kubernetes/contrib/tree/master/ingress/controllers/nginx/examples/tls
 - Create nginx-ingress-controller
 https://github.com/kubernetes/ingress-nginx/tree/master/examples/deployment (uncomment hostNetwork: true)
 - Create ingress
https://kubernetes.io/docs/concepts/services-networking/ingress/#simple-fanout

an example:

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:  
  name: app-ingress
spec:
  tls:  
  - secretName: app-secret  
  rules:  
  - http:      
      paths:      
      - path: /mongotest        
        backend:          
          serviceName: app-svc          
          servicePort: 9000      
      - path: /redistest        
        backend:          
          serviceName: app-svc          
          servicePort: 9000
```
-NOTE:
(1) tls must be set if using the official nginx-ingress-controller.yaml, otherwise, code 301 will be returned.
(2) virtual host set can be ignored if you want to enable external http requests.

