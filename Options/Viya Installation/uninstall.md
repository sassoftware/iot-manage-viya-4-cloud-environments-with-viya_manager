<a name="top"></a>

##### [&#11013;](./README.md) Back
## -Uninstall

Syntax:
```
Viya_Manager -uninstall -cluster <cluster name> -provider <provider> [ -tenant_list <file name> ] [ -v | -verbose ]
```
Where:
>>>
- **cluster name** is the name of the cluster being created or modified
- **provider** is one among **AZURE**, **AWS**, or **GCP**
- **tenant_list** is the file that stores the list of tenant IDs and optional passwords for the **sasprovider** ID when installing Viya in multi-tenant mode. The file must be created in $HOME/Viya_Manager/Cloud-Providers/\<provider\>/Credentials
- **-v | --verbose** enables or suppresses the output of the command to the log
>>>
**-Uninstall** is used for the removal of Viya from a Kubernetes cluster. The command can take several minutes to complete. Among other things, its execution resets the default storage class for the cluster which is set during the Viya installation. Even though not necessary, it's strongly recommended that Viya be stopped before uninstalling it.

**Example:**
```
Viya_Manager -uninstall -cluster viya-iot-azure -provider azure -v
```
Below is an excerpt from the output generated by the command:
```
- Displaying cluster infrastructure for viya-iot-azure...
...
Plan: 33 to add, 0 to change, 0 to destroy.
```
[&#11014;](#top) Top
### Multi-Tenancy

The removal of a multi-tenant Viya install is a two-step process that takes a single command compared to the two needed by a multi-tenant installation. The first part off-boards each tenant ID, after which the software is removed. This is done by running Viya_Manager with a command similar to the following:
```
Viya_Manager -uninstall -cluster viya-iot-azure -provider azure -tenant_list tenants.lst -v
```
The output generated by the command looks identical to the one shown in the example in the previous section.