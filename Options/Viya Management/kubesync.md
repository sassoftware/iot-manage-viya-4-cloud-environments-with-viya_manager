##### [&#11013;](../../README.md) Back
## -Kubesync

Syntax:
```
Viya_Manager -kubesync -cluster <cluster name> -provider <provider>
```
Where:
>>>
- **cluster name** is the name of the cluster being created or modified
- **provider** is one among **AZURE**, **AWS**, or **GCP**
- **-v | --verbose** enables or suppresses the output of the command to the log
>>>
**-Kubesync** is used to align the client and server versions of **kubectl** and **kustomize**. Failure to do so can cause problems when issuing commands against the cluster and results in a message similar to the following:
~~~
WARNING: version difference between client (1.21) and server (1.24) exceeds the supported minor version skew of +/-1  
~~~
When a cluster is first created, Viya_Manager places the correct version of **kubectl** inside the "**Bin**" sub-folder under the cluster's home folder. That version of **kubectl** is the one Viya_Manager uses to issue Kubernetes commands against the cluster. The **-kubesync** option should be executed every time the version of Kubernetes on the cluster changes. 
This mechanism also guarantees that the correct version of the utility is used for each cluster supported through Viya_Manager to avoid any possibility of conflict due to the Kubernetes skew policy.

Example:
```
Viya_Manager -kubesync -cluster viya-iot-azure -provider azure

- Kubectl version info (current):

- Client Version    : v1.26.3
- Kustomize Version : v4.5.7
- Server Version    : v1.27.9

- Would you like to synchronize the client and server versions? (y/n) Y
- Kubectl version info (updated):

- Client Version    : v1.27.9
- Kustomize Version : v5.0.1
- Server Version    : v1.27.9
```
