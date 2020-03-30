

# helm-chart-dependency

"helm-chart-dependency" is a sample project showing dependency between helm charts. Project has layered architecture.

  - Database layer
  - Service layer, depends on 'database' layer
  - Application layer, depends on 'service' layer

# Declaration of dependency

Declaration of dependent chart is made in '***requirements.yaml***'. Layers 'service' & 'application' have requirements.yaml. In this example, the dependant charts are available locally and is referred over 'file://'. Now that requirements are defined, dependant chart must be updated with its requirements.  This is done by *'***helm dependency update'**** command.  Without the dependency updated, requirements are not satisfied. This can be understood with the following series of commands.

```sh
helm dependency list ./application/
# Above command reports that the dependency is 'missing'
helm dependency update ./application/
# Required chart archive & lock gets crea to 'charts/' directory
helm dependency list ./application/
# Now, dependency is reported 'ok'
```
>***Dependency is not updated transitively. Every chart must be updated with its requirement individually.*** 

Updating the dependency for 'application', does not update the dependency of 'service'.  Notice the requirements.lock has the digest. This way the integrity of dependency is maintained. 

# Commands
### Build dependency
```sh
helm dependency update ./service/
helm dependency update ./application/
```
### Deploy chart
Once the dependency is updated for every chart, only the base or root chart needs to be installed and the requirements are provided. 
```sh
helm install --namespace myorg --name myapp ./application/
#Install 'application' chart with its requirements
helm ls 
#Only lists 'myapp' as the dependency is now managed by helm
```
