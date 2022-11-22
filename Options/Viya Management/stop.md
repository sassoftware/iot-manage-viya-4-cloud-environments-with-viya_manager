##### [&#11013;](./README.md) Back
## -Stop

Syntax:
```
Viya_Manager -stop <component> [ -tenant <tenant ID> ] -cluster <cluster name> -provider <provider>
```
Where:
>>>
- **component** is either **Viya** or **CAS_Controller**
- **tenant** is valid only when component is **CAS_Controller**, and it represents the tenant ID associated with the CAS controller to stop
- **cluster name** is the name of the cluster being created or modified
- **provider** is one among **AZURE**, **AWS**, or **GCP**
>>>
**-Stop** is used to stop Viya or the CAS controller.

**Example:**
```
Viya_Manager -stop CAS_Controller -cluster viya-iot-azure -provider azure
```
Below is an example of the output generated by the command:
```
- Stopping the CAS controller...
- Waiting...
- The CAS controller was stopped successfully.
```