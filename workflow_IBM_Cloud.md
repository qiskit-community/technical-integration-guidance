# Recommended workflow for IBM Cloud

* https://us-east.quantum-computing.cloud.ibm.com/openapi/ 
* General documentation at https://cloud.ibm.com/apidocs/quantum-computing 
* Users needs to [create an account through IBM Cloud Qiskit Runtime Service and access to the API key and Cloud Resource Name (CRN)](https://cloud.ibm.com/apidocs/quantum-computing#authentication). 

### Get temporary Access token from Auth API via API Token

* Make a POST request to https://us-east.quantum-computing.cloud.ibm.com/programs with the following following header:

```shell
curl -X POST 'https://iam.cloud.ibm.com/identity/token'\
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=API_Token' 
```

### Prepare a job

Define a QASM string which represents the quantum circuit. Let’s start with a simple example: 

```shell
qasm_string=''OPENQASM 3; 
include "stdgates.inc"; 
qreg q[1]; 
creg c[1]; 
x q[0]; 
c[0] = measure q[0]; ''
```

### Run job

After creating a QASM string from a circuit and defining the options, the job can be send to the IBM Quantum systems through the REST API as follows.

```shell
curl -X POST 'https://us-east.quantum-computing.cloud.ibm.com/jobs'\
-H 'x-qx-client-application: qiskit-version-2/0.39.2/your_application'\ # for an actual Integration, this option it is invaluable to know where jobs are coming from. At this time the "qiskit-version-2/0.39.2/" string is a necessary prefix.
-H 'Authorization: Bearer ACESS_Token'\
-H 'Content-Type: application/json' \
-H 'Service-CRN: CRN_Service'\
-d '{"program_id": "sampler", "backend": "ibmq_qasm_simulator", "start_session": false, "params": {"circuits": ["\nOPENQASM 3;\ninclude \"stdgates.inc\";\nqreg q[1];\ncreg c[1];\nx q[0];\nc[0] = measure q[0]; \n"]}}' 
```

### Wait for results

After submitting the job, the status can be checked using the job_id.

```shell
curl -X GET 'https://us-east.quantum-computing.cloud.ibm.com/jobs/job_id'\
-H 'Authorization: Bearer ACCESS_Token'\
-H 'Content-Type: application/json' \
-H 'Service-CRN: CRN_Service' 
```

The status of the job can be either “Queued“, “Running“, “Completed“ or ”Failed”. 

### Get results

After the job is completed the results can be retrieved using the job_id through the following curl command

```shell
curl -X GET 'https://us-east.quantum-computing.cloud.ibm.com/jobs/job_id/results'\
-H 'Authorization: Bearer ACCESS_Token'\
-H 'Content-Type: application/json' \
-H 'Service-CRN: CRN_Service'\

```