# Jira
Jira is a proprietary issue tracking product developed by Atlassian that allows bug tracking and agile project management. The product name is a truncation of Gojira, the Japanese word for Godzilla, which is a reference to a competitor, Bugzilla


To create Jira deployment follow, this steps.

Create the namespace 
```
kubectl create -f namespace.yml
```
 
Next command will create pvc,  it’s  will make dada persistent. In case if pod goes down, dada will be stable.

```
kubectl create -f pvc_jira.yml -n jira
``` 

Next  command will create actual deployment, mount it to pvc created earlier. Also you have option to specify, how mach resource do you want to give. 
```
kubectl create -f jira.yml -n jira
```

The next step will be create the service Jira, which will expose actually deployment over load balancer. 
```
kubectl create -f service-jira.yml -n jira
```

To check if it works, copy IP address and specify target port over url.

![]([jira_on_kubernetes.yml/Screen Shot 2019-03-28 at 12.28.05 PM.png at master · rameca231190/jira_on_kubernetes.yml · GitHub](https://github.com/rameca231190/jira_on_kubernetes.yml/blob/master/photo/Screen%20Shot%202019-03-28%20at%2012.28.05%20PM.png))



#Company_fscoding