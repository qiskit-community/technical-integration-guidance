# technical-integration-guidance
Guidance and best practices for Technical Integration with QIskit Runtime APIs

# Qiskit Runtime APIs
Qiskit Runtime is accessible via REST API through the following channels
* [IBM Quantum Platform (IQP) API](https://runtime-us-east.quantum-computing.ibm.com/openapi#/) 
* [IBM Cloud API](https://us-east.quantum-computing.cloud.ibm.com/openapi/)

 General documentation is available in both cases 
 * [IQP API documentation](https://docs.quantum-computing.ibm.com/api/runtime)
 * [IBM Cloud API documentation](https://cloud.ibm.com/apidocs/quantum-computing)

 Here, we aim to add guidance on recommended workflows for Integration projects using these APIs. This includes control over issued tokens and websocket queries for live updates for submitted jobs. See the workflow markdown documents in this repository for the separate access channels

 * [IQP recommended workflow](/workflow_IBM_Quantum_Platform.md)
 * [IBM Cloud recommended workflow](/workflow_IBM_Cloud.md)

 We also provide exemplary jupyter notebooks following the recommended workflow

 * [IQP API exemplary jupyter notebook](/api-example-notebooks/rest_api_qasm_job.ipynb)
 * [IBM Cloud API exemplary jupyter notebook](/api-example-notebooks/rest_api_qasm_cloud.ipynb)

