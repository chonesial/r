# Configmap Mounting in Pod using Helms.

Configmap provides a way to decouple configuration from application containers,
allowing you to modify the configuration without rebuilding or redeploying the containers.

Contents 
#### Prerequisite 
#### Helms Chart 
#### Configmap Configuration
#### Mounting of Configmap inside manifest 
#### Applying 

## Prerequisite 

1. Kubernetes
2. Helms
3. Admin access

## Helms Chart

Helm chart is a package that contains all the necessary resources to deploy an application to a Kubernetes cluster. 
This includes YAML configuration files for deployments, services, secrets, 
and config maps that define the desired state of your application
Create a Helm chart in you project directory using below command 

```
helm create mychart

```  

Navigate inside newly create directory and go inside templates directory 

delete all the default files as we will create new files as per our configuration.

## Configmap Configuration 

ConfigMap is an API object that lets you store configuration for other objects to use. 
ConfigMap has data and binaryData fields. 
These fields accept key-value pairs as their values

inside template directory 

Create a file named configmap.yaml and define your ConfigMap in it.

```

vi configmap.yml

```

inside configmap.yml define all the configuration 

```

apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  key1: value1
  key2: value2
  
```

In the same templates directory, create a file named configmap-values.yaml and define the data for your ConfigMap

```

configMapData:
  key1: value1
  key2: value2

```

Update values.yaml file (or create one if it doesn't exist) to include the following line

```

configMapData: {{ include "mychart.configmap-values" . | nindent 2 }}

```

### Mounting inside Manifest 

Inside the same Templates directory 
create pod.yml and inside pod declare the configmap


```

apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image:latest
      volumeMounts:
        - name: config-volume
          mountPath: /path/to/config
  volumes:
    - name: config-volume
      configMap:
        name: my-configmap

```

Update Helm chart's values.yaml file to include any configurable values related to the pod or ConfigMap

```
  
podName: my-pod

```

## Applying 

Now that we have set the configuration .

we will apply and install helm chart which will start our pod and mount our config map inside 

```

helm upgrade --install mychart ./mychart


```

  

          
          

  
