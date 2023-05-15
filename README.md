# Kcert on Kubernetes
A easy to follow tutorial to have an automatic https cert created for your public web url. This uses excellent util by https://github.com/nabsul/kcert  

# Assumption

* You have a public DNS for your website
* Use are using ingress controller like nginx
* You have kubectl access to the cluster  

# Installation

1. **Kcert Setup**
    * > git clone https://github.com/agileguru/kcert-tutorial.git
    * > cd URL-Shortner
    * > kubectl apply -f k8s/Deployment.yml
    * > kubectl apply -f k8s/kcert.yml

1. **Kcert Check**
    * > kubectl get all -n kcert
        ```
        NAME                         READY   STATUS    RESTARTS   AGE
        pod/kcert-7554644d65-5jfkp   1/1     Running   0          31m

        NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE
        service/kcert   ClusterIP   10.100.116.87   <none>        80/TCP,8080/TCP   31m

        NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
        deployment.apps/kcert   1/1     1            1           31m

        NAME                               DESIRED   CURRENT   READY   AGE
        replicaset.apps/kcert-7554644d65   1         1         1       31m
        ```
1. **Ingress Controller setup ( if not done already )**
    * > helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
    * > helm repo update
    * > helm install nginx-ingress ingress-nginx/ingress-nginx
    * > kubectl delete -A ValidatingWebhookConfiguration nginx-ingress-ingress-nginx-admission
    
1. **Create ingress**
    * > kubectl apply -f k8s/ingress.yaml
    * > kubectl get secrets -A 
        ```
        NAME                     TYPE                                  DATA   AGE
        kcert-example-com        kubernetes.io/tls                     2      15h
        ```

1.  **Add loadbalancer-Service external Ip to your DNS record **
    * nginx-ingress-ingress-nginx-controller this service have external ip or alias add it on dns record

2.  **Check Your app**
    * Open Your App in the browser ( https://kcert.example.com )
    * Should show valid certificate :) 
    * Certificate issued by https://letsencrypt.org/


    