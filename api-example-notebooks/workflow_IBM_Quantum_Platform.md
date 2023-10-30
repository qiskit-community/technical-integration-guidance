# Recommended workflow for IBM Quantum Platform (IQP) APIs


### Get temporary Access token from Auth API via API Token

* Copy/Create an API token at https://quantum-computing.ibm.com/

* Make a POST request to https://auth.quantum-computing.ibm.com/api/users/loginWithToken 

```shell
curl -H "Content-Type: application/json" -d "{\"apiToken\": \"$token\"}" "https://auth.quantum-computing.ibm.com/api/users/loginWithToken"
```

### Prepare a job and (qasm) circuit to run

* Define a QASM quantum circuit e.g. 

```shell
qasm_string=''
OPENQASM 3; 
include "stdgates.inc"; 
qreg q[1]; 
creg c[1]; 
x q[0]; 
c[0] = measure q[0];''
```

### Run job via Runtime API

* Make a POST request to https://runtime-us-east.quantum-computing.ibm.com/jobs with the following headers:     
    * 'Content-Type: application/json'     
    * 'x-access-token' with the access token obtained above   
    * Include a JSON payload with the following 

        * 'program_id': 'sampler'/’estimator’,"backend": …, "hub": …, "group": …, "project": …, "params": {"circuits": [qasm_string]}

```shell
curl -H 'Content-Type: application/json' -H "x-access-token: $auth_id" -d '{"program_id": "sampler","backend": "ibmq_qasm_simulator","hub": "ibm-q-internal","group": "dev-sys-software","project": "internal-test","params": {"circuits": "OPENQASM 3;include \"stdgates.inc\";qreg q[1];creg c[1];x q[0];c[0] = measure q[0];"}}' 'https://runtime-us-east.quantum-computing.ibm.com/jobs'
```

### Get results  
* Make a GET request to https://runtime-us-east.quantum-computing.ibm.com/jobs/{job_id}/results to retrieve the job results.   
* Include the 'x-access-token' header with the access token obtained above. 

```shell
curl -H "x-access-token: $auth_id" 'https://runtime-us-east.quantum-computing.ibm.com/jobs/'$job_id'/results'
```

### Invalidate Token (Optional)   

* Make a POST request to https://auth.quantum-computing.ibm.com/api/users/logout to invalidate the access token.   
* Include the 'x-access-token' with the access token obtained above as json input

```shell
curl -H "x-access-token: $auth_id" 'https://auth.quantum-computing.ibm.com/api/users/logout'
```