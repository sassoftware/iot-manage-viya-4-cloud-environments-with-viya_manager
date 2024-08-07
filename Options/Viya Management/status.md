##### [&#11013;](../../README.md) Back
## -Status

Syntax:
```
Viya_Manager -status -cluster <cluster name> -provider <provider>
```
Where:
>>>
- **cluster name** is the name of the cluster being created or modified
- **provider** is one among **AZURE**, **AWS**, or **GCP**
- **-Status** is used to display the status of the Viya pods on the Kubernetes cluster.
>>>
**Example:**
```
Viya_Manager -status Viya -cluster viya-iot-azure -provider azure
```
Below is an example of the output generated by the command:
```
							Viya Status (Press CTRL-C to exit)

					PODS		RUNNING		PENDING		SUCCEEDED	CRASHLOOP	ERROR		FAILED

Tue Feb 8 22:54:50 EST 2022		172		166		0 	 	6	 	0	 	0 	 	0
Tue Feb 8 22:55:02 EST 2022		172		166		0	 	6	 	0	 	0	 	0
Tue Feb 8 22:55:13 EST 2022		172		166		0	 	6	 	0	 	0	 	0
Tue Feb 8 22:55:24 EST 2022		166		166		0	 	0	 	0	 	0	 	0
Tue Feb 8 22:55:35 EST 2022		166		166		0	 	0	 	0	 	0	 	0
Tue Feb 8 22:55:46 EST 2022		166		166		0	 	0	 	0	 	0	 	0
Tue Feb 8 22:55:57 EST 2022		166		166		0 	 	0	 	0	 	0	 	0
```
