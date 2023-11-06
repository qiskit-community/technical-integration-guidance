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
-H 'Authorization: Bearer ACESS_Token'\
-H 'Content-Type: application/json' \
-H 'Service-CRN: CRN_Service'\
-d '{"program_id": "sampler", "backend": "ibmq_qasm_simulator", "start_session": false, "params": {"circuits": ["\nOPENQASM 3;\ninclude \"stdgates.inc\";\nqreg q[1];\ncreg c[1];\nx q[0];\nc[0] = measure q[0]; \n"]}}' 
```
### Set Runtime Parameters (optional)

If no options are specified, Qiskit Runtime will automatically set parameters such as resilience level for Error Mitigation and optimization settings for remote transpilation of the circuit. So you can chose to be agnostic and leave these settings to the Qiskit Runtime default. If you are optimizing details, you can chose to specify any of the following settings. For details on each setting listed please see the [Qiskit Runtime Options documentation](https://docs.quantum-computing.ibm.com/api/qiskit-ibm-runtime/qiskit_ibm_runtime.options.Options)

```shell
{"program_id": "sampler", 
 "backend": "ibmq_qasm_simulator", 
 "start_session": false, 
 "params": {"circuits": ["\nOPENQASM 3;\ninclude \"stdgates.inc\";\nqreg q[1];ncreg c[1];\nx q[0];\nc[0] = measure q[0]; \n"]},"parameters": [
            []
        ],
        "circuit_indices": [
            0
        ],
        "parameter_values": [
            []
        ],
        "transpilation_settings": {
            "skip_transpilation": false,
            "initial_layout": null,
            "layout_method": null,
            "routing_method": null,
            "approximation_degree": null,
            "optimization_settings": {
                "level": 3
            },
            "coupling_map": null,
            "basis_gates": null
        },
        "resilience_settings": {
            "noise_amplifier": "TwoQubitAmplifier",
            "noise_factors": [
                1,
                3,
                5
            ],
            "extrapolator": "LinearExtrapolator",
            "level": 0
        },
        "run_options": {
            "shots": 10000,
            "init_qubits": true,
            "noise_model": null,
            "seed_simulator": null
        }
    }
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