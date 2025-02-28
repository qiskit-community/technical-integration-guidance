# Recommended workflow for IBM Quantum Platform (IQP) APIs

### (Optional) Get temporary Access token from Auth API via API Token

This is especially useful if you would like control over tokens, such as token invalidation

* Copy/Create an API token at https://quantum-computing.ibm.com/

* Make a POST request to https://auth.quantum-computing.ibm.com/api/users/loginWithToken 

```shell
curl -H "Content-Type: application/json" -d "{\"apiToken\": \"$token\"}" "https://auth.quantum-computing.ibm.com/api/users/loginWithToken"
```

### Prepare a job and (qasm) circuit to run

* Define a QASM quantum circuit e.g. 

```shell
qasm_string=''OPENQASM 3; \
include "stdgates.inc"; \
qreg q[1]; \
creg c[1]; \
x q[0]; \
c[0] = measure q[0]; ''
```

### Transpilation

The circuit provided will need to be mapped to instructions fitting to the hardware basis instructions of the QPU, which is a process known as Transpilation

* transpile [locally via Qiskit](https://docs.quantum.ibm.com/transpile/defaults-and-configuration-options) or:

* Use the Cloud Transpiler API. See general documentation at https://cloud-transpiler.quantum.ibm.com/docs 


```shell
curl -X POST \
  "https://cloud-transpiler.quantum.ibm.com/transpile" \
  -H 'accept: application/json' \
  -H 'authorization: Bearer '$token \
  -d '{
    "qasm_circuits": "'$qasm_string'",
    "backend": "ibm_peekskill",
    "optimization_level": 3,
    "use_ai": false
     }'
```
Request the results using the task_id

```shell
curl -X GET \
  "https://cloud-transpiler.quantum.ibm.com/transpile/${task_id}" \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer '$token
```
### Run job via Runtime API

* Make a POST request to `'https://api.quantum-computing.ibm.com/runtime/jobs'` with the following headers:     
    * 'Content-Type: application/json'     
    * 'x-access-token' with the access token obtained above
    * 'x-qx-client-application: qiskit-version-2/0.39.2/'+'your_application' specifying the application you might be running from. For an actual Integration project, this option it is invaluable to know where jobs are coming from. At this time the "qiskit-version-2/0.39.2/" string is a necessary prefix.
    * Include a JSON payload with the following 

        * 'program_id': 'sampler'/’estimator’,"backend": …, "hub": …, "group": …, "project": …, "params": {"circuits": [resulting_qasm]} with the qasm that the transpilation Service outputs

```shell
curl -X POST \
  "https://api.quantum-computing.ibm.com/runtime/jobs" \
  -H 'Content-Type: application/json' \
  -H 'x-access-token: '$api_key \
  -H 'x-qx-client-application: qiskit-version-2/0.39.2/your_application' \
  -d '{
    "program_id": "sampler",
    "backend": "ibm_peekskill",
    "hub": "ibm-q-internal",
    "group": "dev-sys-software",
    "project": "internal-test",
    "start_session": "False",
    "params": {"circuits": "'$resulting_qasm'"}
    }'

```
## (optional) Create Session 

c.f. documentation at https://docs.quantum.ibm.com/api/runtime

```shell
curl -X POST "https://api.quantum-computing.ibm.com/runtime/sessions" \
  -H 'Accept: application/json' \
  -H 'x-access-token: '$api_key \
  -H 'Content-Type: application/json' \
  -d '{
    "backend": "ibm_peekskill",
    "instance": "ibm-q-internal/dev-sys-software/internal-test"
}'
```

### Submit job(s) against resulting Session ID

```shell
curl -H 'Content-Type: application/json' -H "x-access-token: $auth_id" -H 'x-qx-client-application: qiskit-version-2/0.39.2/YOUR_APPlication' -d '{"program_id": "sampler","backend": "ibm_cusco","hub": "ibm-q-internal","group": "dev-sys-software","project": "internal-test","session_id": "YOUR_SESSION_ID","params": {"circuits": "OPENQASM 3;include \"stdgates.inc\";qreg q[1];creg c[1];x q[0];c[0] = measure q[0];"}}' 'https://api.quantum-computing.ibm.com/runtime/jobs'
```
### Wait for results, check job status
After submitting the job, the status can be checked using the job_id.

```shell
curl -H "x-access-token: $auth_id" 'https://api.quantum-computing.ibm.com/runtime/jobs/'$job_id
```
The status of the job can be either “Queued“, “Running“, “Completed“ or ”Failed”. 

### Get results  
* Make a GET request to 'https://api.quantum-computing.ibm.com/runtime/jobs/{job_id}/results' to retrieve the job results.   
* Include the 'x-access-token' header with the access token obtained above. 

```shell
curl -H "x-access-token: $auth_id" 'https://api.quantum-computing.ibm.com/runtime/jobs/'$job_id'/results'
```