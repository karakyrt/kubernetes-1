     # Install Grafana 
                                                          
  # What is Grafana ??
Grafana is an open source, feature rich, powerful, elegant and highly-extensible analytics and monitoring software that runs on Linux, Windows and MacOS. It is a de facto software for data analytics, being used at Stack Overflow, eBay, PayPal, Uber and Digital Ocean – just to mention but a few.
It supports 30+ open source as well as commercial databases/data sources including MySQL, PostgreSQL, Graphite, Elasticsearch, OpenTSDB, Prometheus and InfluxDB. It allows you to dig deeply into large volumes of real-time, operational data; visualize, query, set alerts and get insights from your metrics from different storage locations.

 # Steps for Installation:

1. You need use 'namespaces' it already created on Kubernetes Cluster 

2. This will create a 'deployment' called 'grafana-deployment' under namespaces 'tools'  It sets the replica to 1, attaches the label 'app: grafana' to deployment. And does the following thinks:

     kubectl create -f grafana-deployment.yaml:

       1.Dowload the container image(attach image pull polices----this means if you have image on your server don't go to docker hub)

       2.Sets the resources request and limits for the container(cpu,memory)

       3.Sets Up Variables(for secret.yaml without this deployment can't be created)

       4.This file has Readiness Probe(check container healts)

       5.Exposes port 3000 of the container (Grafana  uses  by defaults  port 3000)

       6.Mounts the volume to a path inside the container

3. After when you will have 'namespaces' and 'grafana-deployment'

   You need to encrypt your token(password) first before you add it to the secret.yaml file where is you will have credentials:
   
   '''
   example encryption:                       
   echo -n 'admin' | base64
   YWRtaW4=
   echo -n '1fhg9e67df' | base64
   DRhjjZDFDGTlwmRm
   '''
   
   for decryption:
     use this command: echo 'MWYyZDFlFGghHVH' | base64 --decode

       After this you need to create file:  

          kubectl create -f secret.yaml
   This 'secret' has encrypted information about log in and password

       example:
       admin-password: YWRtaW4=
       admin-username: YWRtaW4=

4. Next step create 'service'

    kubectl create -f grafana-service.yaml

      This will create a 'service' called 'grafana-service'. The type of the service is 'LoadBalancer' which publishes the port '3000' of the node through port '3000' of the service, sets the protocol to TCP and sets the target port to '80' (this is the port of our container).
      NOTE : service type 'LoadBalancer' works only on cloud providers that provider Load-Balancer service.

5. To check if everething correct run this commands:

```python
   kubectl get sectets -n tools
   kubectl get pods -n tools
   kubectl get service -n tools
   kubectl get deployment -n tools 
```
6. kubectl -n grafana get services

   The output should show a service called 'grafana-service'
   Copy the EXTERNAL-IP of your service, and paste on your browsers > IP:3000
   You should see the login page for Grafana.
   Set Up Login and Password:
   
   ```
   username = *******
   password = *******
   ```
   
7. If needed you can create:
   kubectl create -f grafana-pv.yaml
   kubectl create -f grafana-pvc.yaml
   This filels create volume for your 
   NOTE: but in this case i didn't see any files creating storage

8. After this you can work in Grafana use this link:
   http://35.226.61.13:3000/login
   




          
       
   




