## AWS Simple Deployment

An internal project for the IoT division requires the deployment of Viya 4 on a cluster running on AWS. The list of requirements include:

```
- The cluster name is viya-iot-aws
- The cluster must be defined in the US East (N. Virginia) region
- The cluster must come with both a Jump and a NFS box
- The Jump and NFS boxes must be accessible via a public IP address
- The deployment is going to rely on standard storage
- TLS must be implemented throughout the environment
- Future updates of the environment will be applied via the Deployment Operator
```

The order info for the deployment is the following:

```
- Order number is 9CNK14
- The cadence is 2021.2.3
- The install type is stable
- The email of the user authorized to access the order is john.doe@sas.com
```

In order to deploy the cluster infrastructure, credentials must exist to create resources on AWS. The file that stores them is called **aws-credentials** and is located in the \<Viya_Manager-root-folder\>/Cloud-Providers/aws/Credentials folder:

<table align="center"><tr><td align="center" width="9999">
<img src="https://github.com/sassoftware/iot-manage-viya-4-cloud-environments-with-viya_manager/blob/main/Images/AWS_credentials.png" align="left" height="60">
</td></tr></table>

Once the Kubernetes cluster is created, the deployment of Viya requires a separate set of credentials to be able to download the order. These credentials are found in the **sas-api** file located inside the \<Viya_Manager-root-folder\>/Cloud-Providers/aws/Credentials folder:

<table align="center"><tr><td align="center" width="9999">
<img src="https://github.com/sassoftware/iot-manage-viya-4-cloud-environments-with-viya_manager/blob/main/Images/Sample_SAS_API_credentials.png" align="left" height="60">
</td></tr></table>

Since the installer’s email is already present in the sas-api file, no changes are required. For more information on credentials, please refer to the Credentials section of this document.

To following command creates the cluster infrastructure:

**Viya_Manager -apply -cluster viya-iot-aws -provider aws -v**

Output:

```
- Creating cluster infrastructure for viya-iot-aws...
- Creating configuration folder for viya-iot-aws...
- Creating credentials folder for viya-iot-aws...
- Creating aws configuration file for viya-iot-aws...
- Customizing aws configuration file for viya-iot-aws...
…
```

The creation of the cluster can also be seen from the AWS Console:

<table align="center"><tr><td align="center" width="9999">
<img src="https://github.com/sassoftware/iot-manage-viya-4-cloud-environments-with-viya_manager/blob/main/Images/AWS_Console.png" align="left" height="300">
</td></tr></table>

The information below is displayed at the end of the process:

```
Apply complete! Resources: 3 added, 16 changed, 2 destroyed.

Outputs:

autoscaler_account = "arn:aws:iam::299238628350:role/viya-iot-aws-cluster-autoscaler"
cluster_endpoint = "https://118AA54B7A088F70B04A168590D1E20D.gr7.us-east-1.eks.amazonaws.com"
cluster_iam_role_arn = "arn:aws:iam::299238628350:role/terraform-20220206205252734800000002"
cluster_name = "viya-iot-aws-eks"
cluster_node_pool_mode = "default"
cr_endpoint = "https://299238628350.dkr.ecr.us-east-1.amazonaws.com"
infra_mode = "standard"
jump_admin_username = "jumpuser"
jump_private_dns = "ip-192-168-129-54.ec2.internal"
jump_private_ip = "192.168.129.54"
jump_public_dns = "ec2-3-228-102-59.compute-1.amazonaws.com"
jump_public_ip = "3.228.102.59"
jump_rwx_filestore_path = "/viya-share"
kube_config = <sensitive>
location = "us-east-1"
nat_ip = "54.144.68.63"
nfs_admin_username = "nfsuser"
nfs_private_ip = "192.168.129.47"
nfs_public_dns = "ec2-3-209-124-15.compute-1.amazonaws.com"
nfs_public_ip = "3.209.124.15"
nsf_private_dns = "ip-192-168-129-47.ec2.internal"
prefix = "viya-iot-aws"
provider = "aws"
rwx_filestore_endpoint = "ip-192-168-129-47.ec2.internal"
rwx_filestore_path = "/export"
worker_iam_role_arn = "arn:aws:iam::299238628350:role/terraform-20220206210622471900000008"

- Task completed.
- Cluster added to KUBECONFIG file. Switched to context "viya-iot-aws-eks".

- Re-setting default storage class...
- To access the Jump and NFS boxes via ssh, use the /home/john.doe/Viya_Manager/Cloud-Providers/aws/Clusters/viya-iot-aws/Keys/viya-iot-aws identity key.
- Done!
```

The last portion of the output shows that the KUBECONFIG file for the user running the command has been updated to add an entry for the cluster being created (viya-iot-aws). That allows the user to use the **kubectl** utility from the command line to issue commands against the cluster, making it easier to administer it.

With the cluster infrastructure in place, Viya can now be deployed using the following command:

**Viya_Manager -install -cluster viya-iot-aws -provider aws -order 9CNK14 -cadence 2021.2.3 -type stable -email john.doe@sas.com -v**

Output:

```
- Installing Viya...2022/02/06 21:48:20 INFO: using config file: /Keys/viya4-orders-cli.yaml
OrderNumber: 9CNK14
AssetName: certificates
AssetReqURL: https://api.sas.com/mysas/orders/9CNK14/certificates
AssetLocation: /License-and-certificates/SASViyaV4_9CNK14_certs.zip
Cadence:
CadenceRelease:
2022/02/06 21:48:21 INFO: using config file: /Keys/viya4-orders-cli.yaml
OrderNumber: 9CNK14
AssetName: license
AssetReqURL: https://api.sas.com/mysas/orders/9CNK14/cadenceNames/stable/cadenceVersions/2021.2.3/license
AssetLocation: /License-and-certificates/SASViyaV4_9CNK14_stable_2021.2.3_license_2022-01-20T155048.jwt
Cadence: STABLE 2021.2.3
CadenceRelease:
2022/02/06 21:48:22 INFO: using config file: /Keys/viya4-orders-cli.yaml
OrderNumber: 9CNK14
AssetName: deploymentAssets
AssetReqURL: https://api.sas.com/mysas/orders/9CNK14/cadenceNames/stable/cadenceVersions/2021.2.3/deploymentAssets
AssetLocation: /Orders/SASViyaV4_9CNK14_stable_2021.2.3_20220205.1644033355437_deploymentAssets_2022-02-06T214831.tgz
Cadence: Stable 2021.2.3
CadenceRelease: 20220205.1644033355437

- Re-setting default storage class...
- Creating viya configuration file for viya-iot-aws...
- Customizing viya configuration file for viya-iot-aws...
…
```

The following info is displayed at the end of the installation:

```
Sunday 06 February 2022  21:57:47 +0000 (0:00:00.257)       0:08:49.340 *******
===============================================================================
vdm : manifest - deploy ----------------------------------------------- 184.60s
vdm : prereqs - cluster-local deploy ----------------------------------- 56.32s
vdm : manifest - deploy update ----------------------------------------- 46.93s
vdm : kustomize - Generate deployment manifest ------------------------- 35.33s
baseline : Deploy ingress-nginx ---------------------------------------- 34.28s
baseline : Deploy cert-manager ----------------------------------------- 31.46s
baseline : Deploy metrics-server --------------------------------------- 24.44s
vdm : assets - Extract downloaded assets ------------------------------- 19.55s
vdm : prereqs - cluster-wide ------------------------------------------- 18.03s
baseline : Deploy nfs-subdir-external-provisioner ---------------------- 14.94s
vdm : prereqs - cluster-api --------------------------------------------- 8.84s
baseline : Deploy cluster-autoscaler ------------------------------------ 7.71s
jump-server : jump-server - lookup groups ------------------------------- 4.59s
jump-server : jump-server - create folders ------------------------------ 3.83s
vdm : Download viya4-orders-cli ----------------------------------------- 3.42s
baseline : Remove deprecated nfs-client-provisioner --------------------- 3.09s
baseline : Remove deprecated efs-provisioner namespace ------------------ 2.95s
vdm : copy - VDM transformers ------------------------------------------- 2.70s
vdm : copy - VDM generators --------------------------------------------- 2.64s
vdm : Create namespace -------------------------------------------------- 1.80s

- Task completed.
- Viya installed successfully. It should be up and running in about 1 hour.
- If necessary, add the following information to your Network Address Management Entry System to access Viya:
	- Host ........: viya-iot-aws.unx.sas.com
	- IP Address ..: 34.198.109.253
```

The IP address to connect to Viya is displayed at the end of process. The final requirement involves the installation of the Deployment Operator through the following command:

**Viya_Manager -operator -install -cluster viya-iot-aws -provider aws**

Output:

```
- Installing the Viya Deployment Operator on viya-iot-aws...
- Creating the configuration folder...
- Creating the configuration files...
- Customizing the configuration files...
- Creating the namespace...
- Deploying the Viya Deployment Operator...
- Done!
```
