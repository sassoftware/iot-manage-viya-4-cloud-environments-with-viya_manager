##### [&#11013;](../../README.md) Back
## -Gencert

Syntax:
```
Viya_Manager -gencert -cluster <cluster name> -provider <provider>
```
Where:
>>>
- **cluster name** is the name of the cluster being created or modified
- **provider** is one among **AZURE**, **AWS**, or **GCP**
- **-v | --verbose** enables or suppresses the output of the command to the log
>>>
**-Gencert** is used to generate a certificate in PEM format for the client browsers. Once the certificate is created, it can be imported in the browser to avoid the displaying of warnings regarding the validity of self-signed a certificate. Generating a certificate is an optional step.

Example:
```
Viya_Manager -gencert -cluster viya-iot-azure -provider azure

- Generating /Users/mauro.cazzari/Viya_Manager/Cloud-Providers/azure/Clusters/viya-azure-1/License-and-certificates/viya-azure-1.pem certificate for client browsers...
- Certificate file viya-azure-1.pem generated successfully.
- It can now be imported in your client browser.
```
