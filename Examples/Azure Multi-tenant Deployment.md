## Azure Simple Multi-tenant Deployment

An internal project for the IoT division requires the deployment of Viya 4 on a cluster running on AZURE. The list of requirements include:

```
- The cluster name is viya-azure-5
- The cluster must be defined in the US East region
- The cluster must come with a NFS box
- The NFS boxes must be accessible via a public IP address
- The deployment is going to rely on standard storage
- TLS must be implemented throughout the environment
- Multi-tenancy should be used when installing Viya
- Future updates of the environment will be applied via the Deployment Operator
```

The order info for the deployment is the following:

```
- Order number is 9CS5LN
- The cadence is 2023.04
- The install type is stable
- The email of the user authorized to access the order is john.doe@sas.com
```

In order to deploy the cluster infrastructure, credentials must exist to create resources on Azure. The file that stores them is called **azure-credentials** and is located in the \<Viya_Manager-root-folder\>/Cloud-Providers/azure/Credentials folder:

<table align="center"><tr><td align="center" width="9999">
<img src="https://github.com/sassoftware/iot-manage-viya-4-cloud-environments-with-viya_manager/blob/main/Images/Azure_credentials.png" align="left" height="60">
</td></tr></table>

Once the Kubernetes cluster is created, the deployment of Viya requires a separate set of credentials to be able to download the order. These credentials are found in the **sas-api** file located inside the \<Viya_Manager-root-folder\>/Cloud-Providers/azure/Credentials folder:

<table align="center"><tr><td align="center" width="9999">
<img src="https://github.com/sassoftware/iot-manage-viya-4-cloud-environments-with-viya_manager/blob/main/Images/Sample_SAS_API_credentials.png" align="left" height="60">
</td></tr></table>

Since the installer’s email is already present in the sas-api file, no changes are required. For more information on credentials, please refer to the Credentials section of this document.

To following command creates the cluster infrastructure:

**Viya_Manager -apply -cluster viya-azure-5 -provider azure -v**

Output:

```
- Creating cluster infrastructure for viya-azure-5...
- Creating configuration folder for viya-azure-5...
- Creating credentials folder for viya-azure-5...
- Creating aws configuration file for viya-azure-5...
- Customizing aws configuration file for viya-azure-5...
…
```

The creation of the cluster can also be seen from the Azure Console:

<table align="center"><tr><td align="center" width="9999">
<img src="https://github.com/sassoftware/iot-manage-viya-4-cloud-environments-with-viya_manager/blob/main/Images/Azure_Console.png" align="left" height="800">
</td></tr></table>

The information below is displayed at the end of the process:

```
Apply complete! Resources: 30 added, 0 changed, 0 destroyed.

Outputs:

aks_cluster_node_username = <sensitive>
aks_cluster_password = <sensitive>
aks_host = <sensitive>
cluster_api_mode = "public"
cluster_name = "viya-azure-5-aks"
cluster_node_pool_mode = "default"
kube_config = <sensitive>
location = "eastus"
nat_ip = "20.85.254.41"
nfs_admin_username = "nfsuser"
nfs_private_ip = "192.168.2.4"
nfs_public_ip = "52.149.150.106"
prefix = "viya-azure-5"
provider = "azure"
provider_account = "iot"
rwx_filestore_endpoint = "192.168.2.4"
rwx_filestore_path = "/export"

- Task completed.
- Cluster added to KUBECONFIG file. Switched to context "viya-azure-5-aks".

- Re-setting default storage class...
- To access the Jump and NFS boxes via ssh, use the /root/Viya_Manager/Cloud-Providers/azure/Clusters/viya-azure-5/Keys/viya-azure-5 identity key.
- Done!
```

The last portion of the output shows that the KUBECONFIG file for the user running the command has been updated to add an entry for the cluster being created (viya-zure-5). That allows the user to use the **kubectl** utility from the command line to issue commands against the cluster, making it easier to administer it.

With the cluster infrastructure in place, Viya can now be deployed using the following command:

**Viya_Manager -install -cluster viya-azure-5 -provider azure -order 9CS5LN -cadence 2023.03 -type stable -email john.doe@sas.com -domain unx.sas.com -tenant_list viya-azure-5.tenants -v**

Output:

```
- Installing...2023/05/01 17:37:44 INFO: using config file: /Keys/viya4-orders-cli.yaml
OrderNumber: 9CS5LN
AssetName: certificates
AssetReqURL: https://api.sas.com/mysas/orders/9CS5LN/certificates
AssetLocation: /License-and-certificates/SASViyaV4_9CS5LN_certs.zip
Cadence:
CadenceRelease:
2023/05/01 17:37:46 INFO: using config file: /Keys/viya4-orders-cli.yaml
OrderNumber: 9CS5LN
AssetName: license
AssetReqURL: https://api.sas.com/mysas/orders/9CS5LN/cadenceNames/stable/cadenceVersions/2023.03/license
AssetLocation: /License-and-certificates/SASViyaV4_9CS5LN_stable_2023.03_license_2023-05-01T173753.jwt
Cadence: Stable 2023.03
CadenceRelease:
2023/05/01 17:37:54 INFO: using config file: /Keys/viya4-orders-cli.yaml
OrderNumber: 9CS5LN
AssetName: deploymentAssets
AssetReqURL: https://api.sas.com/mysas/orders/9CS5LN/cadenceNames/stable/cadenceVersions/2023.03/deploymentAssets
AssetLocation: /Orders/SASViyaV4_9CS5LN_stable_2023.03_20230425.1682458335994_deploymentAssets_2023-05-01T173808.tgz
Cadence: Stable 2023.03
CadenceRelease: 20230425.1682458335994

- Re-setting default storage class...
- Creating viya configuration file for viya-azure-5 from viya.cfg.template...
- Customizing viya configuration file for viya-azure-5...
…
```

The following info is displayed at the end of the installation:

```
Monday 01 May 2023  17:54:16 +0000 (0:00:06.910)       0:16:05.168 ************
===============================================================================
vdm : deploy - Deploy SAS Viya ---------------------------------------- 724.79s
baseline : Deploy nfs-subdir-external-provisioner-sas ------------------ 74.51s
baseline : Deploy ingress-nginx ---------------------------------------- 35.77s
vdm : orchestration tooling - Extract orchestration tooling layers ----- 25.80s
vdm : orchestration tooling - Download orchestration tooling image ----- 11.73s
Delete tmpdir ----------------------------------------------------------- 6.91s
vdm : copy - VDM generators --------------------------------------------- 6.12s
vdm : copy - VDM transformers ------------------------------------------- 5.25s
baseline : Deploy nfs-subdir-external-provisioner-pg-storage ------------ 4.52s
vdm : assets - Extract downloaded assets -------------------------------- 1.71s
vdm : copy - VDM resources ---------------------------------------------- 1.68s
baseline : Retreive K8s cluster information ----------------------------- 1.52s
baseline : Remove deprecated efs-provisioner namespace ------------------ 1.49s
vdm : pg_config - Look for ConfigMap ------------------------------------ 1.48s
baseline : Retrieve K8s cluster information ----------------------------- 1.42s
vdm : sasdeployment custom resource - Create SAS Viya namespace --------- 1.38s
baseline : Check for metrics service ------------------------------------ 1.29s
vdm : pg_config - Find v4 crunchy deployment ---------------------------- 1.27s
baseline : Retreive K8s cluster information ----------------------------- 1.27s
vdm : orchestration tooling - Extract orchestration tooling archive ----- 1.25s

- Task completed.
- Viya installed successfully.
- If necessary, add the following information to your Network Address Management Entry System or local host file to access Viya:
	- IP Address ..: 52.146.31.107
	- Host ........: iot.viya-azure-5.unx.sas.com
	- Host ........: iot.iot.viya-azure-5.unx.sas.com
	- Host ........: ggai.iot.viya-azure-5.unx.sas.com

- Done!
```

The IP address to connect to Viya is displayed at the end of process. The final requirement involves the installation of the Deployment Operator through the following command:

**Viya_Manager -operator -install -cluster viya-azure-5 -provider azure**

Output:

```
- Installing the Viya Deployment Operator on viya-azure-5...
- Creating the configuration folder...
- Creating the configuration files...
- Customizing the configuration files...
- Creating the namespace...
- Deploying the Viya Deployment Operator...
- Done!
```
