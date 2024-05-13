##### [&#11013;](../../README.md) Back
## -Operator

Syntax:
```
Viya_Manager -operator <-install [ -noseccomp ] | -uninstall> -cluster \<cluster name\> -provider \<provider\>
```
Where:
>>>
- **cluster name** is the name of the cluster being created or modified
- **provider** is one among **AZURE**, **AWS**, or **GCP**
- **noseccomp** allows for the Viya Deployment Operator to be installed on a cluster that doesn't support [Secure Compute Mode](https://itnext.io/seccomp-in-kubernetes-part-i-7-things-you-should-know-before-you-even-start-97502ad6b6d6)
>>>
**-Operator** controls the installation or removal of the Viya Deployment Operator. On Kubernetes, the operator is deployed in cluster-wide mode in its own namespace, called **sas-deployment-operator**, and it is ready to be used without any modifications. As mentioned earlier, Viya_Manager provides scripts to automate the deployment of Viya through the Deployment Operator. The scripts can be found in **$HOME/\<Viya_Manager-root-folder\>/Management/Deployment/Operator**. Please refer to the README.md file in the folder for more details on the scripts.

**Example:**
```
Viya_Manager -operator -install -cluster viya-iot-azure -provider azure
```
The successful completion of the command will generate the following output:
```
- Installing the Viya Deployment Operator on viya-iot-azure...
- Creating the configuration folder...
- Creating the configuration files...
- Customizing the configuration files...
- Creating the namespace...
- Deploying the Viya Deployment Operator...
- Done!
```
