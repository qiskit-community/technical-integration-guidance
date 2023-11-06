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

* Make a POST request to `'https://runtime-us-east.quantum-computing.ibm.com/jobs'` with the following headers:     
    * 'Content-Type: application/json'     
    * 'x-access-token' with the access token obtained above   
    * Include a JSON payload with the following 

        * 'program_id': 'sampler'/’estimator’,"backend": …, "hub": …, "group": …, "project": …, "params": {"circuits": [qasm_string]}

```shell
curl -H 'Content-Type: application/json' -H "x-access-token: $auth_id" -d '{"program_id": "sampler","backend": "ibmq_qasm_simulator","hub": "ibm-q-internal","group": "dev-sys-software","project": "internal-test","params": {"circuits": "OPENQASM 3;include \"stdgates.inc\";qreg q[1];creg c[1];x q[0];c[0] = measure q[0];"}}' 'https://runtime-us-east.quantum-computing.ibm.com/jobs'
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

### Get results  
* Make a GET request to 'https://runtime-us-east.quantum-computing.ibm.com/jobs/{job_id}/results' to retrieve the job results.   
* Include the 'x-access-token' header with the access token obtained above. 

```shell
curl -H "x-access-token: $auth_id" 'https://runtime-us-east.quantum-computing.ibm.com/jobs/'$job_id'/results'
```

### Invalidate Token (Optional)   

* Make a POST request to 'https://auth.quantum-computing.ibm.com/api/users/logout' to invalidate the access token.   
* Include the 'x-access-token' with the access token obtained above as json input

```shell
curl -H "x-access-token: $auth_id" 'https://auth.quantum-computing.ibm.com/api/users/logout'
```