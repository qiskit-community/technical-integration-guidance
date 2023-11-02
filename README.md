# 🚀 Integrating External Parties with IBM Quantum Services

Welcome to our technical integration documentation! This guide will help you seamlessly integrate external parties with our IBM Quantum Services using our APIs.

<p align="center" width="100%">
    <img width="25%" src="https://cdn-icons-png.flaticon.com/512/11083/11083235.png"> 
</p>

## 📖 Introduction

This repository contains comprehensive documentation for integrating external parties with our IBM Quantum Services through RESTful APIs. We aim to provide a smooth and efficient integration experience, enabling you to take full advantage of our services.

## 🌟 Let's get started! 

You have the ability to connect to IBM Quantum Systems via Qiskit Runtime, which is made available through different IBM Quantum Channels.

Qiskit Runtime is our quantum computing service and programming model for building, optimizing, and executing workloads at scale. Qiskit Runtime introduces primitives to seamlessly perform foundational quantum computing tasks with increased performance. It is the entrypoint to run quantum circuits in our fleet of quantum systems. The Qiskit Runtime Service interface is a REST API through which users and developers can interact with. In essence, it serves as the means by which external parties can leverage Qiskit Runtime's capabilities and functionality from remote locations or applications.

The term "IBM Quantum Channels" here refers to the various ways in which you can interface with Qiskit Runtime. These channels might include endpoints or methods that enable you to submit jobs, retrieve results, or manage computations on the quantum systems. This flexibility allows users to seamlessly integrate Qiskit Runtime Services into their own software, services, or applications, enhancing their quantum computing capabilities. Each channel can offer different functionalities and services built on top of Qiskit Runtime.

IBM Quantum expose two different channels:

* [IBM Quantum Platform](https://quantum-computing.ibm.com/): our suite of applications to support quantum research and development needs. It is an online platform allowing public and premium access to cloud-based quantum computing.
     > It is accesible via a [REST API with Open Specifications](https://runtime-us-east.quantum-computing.ibm.com/openapi#/). You can access to the [General Documentation](https://docs.quantum-computing.ibm.com/api/runtime)

* [IBM Cloud Qiskit Runtime Service](https://cloud.ibm.com/quantum): our cloud native interface for Qiskit Runtime as a quantum computing service on IBM Cloud. It allows users to run programs on larger quantum computers (that were previously limited to specific clients) as a pay-as-you-go model and to access classical developer tools and complementary cloud services.
     > It is accesible via a [REST API with Open Specifications](https://us-east.quantum-computing.cloud.ibm.com/openapi/). You can access to the [General Documentation](https://cloud.ibm.com/apidocs/quantum-computing)

 Here, we aim to add guidance on recommended workflows for Integration projects using these APIs. This includes control over issued tokens and websocket queries for live updates for submitted jobs. See the workflow markdown documents in this repository for the separate access channels

## 🔌 Integrating against IBM Quantum Platform

 * [Recommended workflow](/workflow_IBM_Quantum_Platform.md)


## 🔌 Integrating against IBM Cloud Qiskit Runtime Service

 * [Recommended workflow](/workflow_IBM_Cloud.md)

## 📖 Additional Resources

We also offer illustrative samples that align with our recommended workflow.

### 🐍 Python

 * [IBM Quantum Platform API exemplary jupyter notebook](/api-example-notebooks/rest_api_qasm_job.ipynb)
 * [IBM Cloud Qiskit Runtime API exemplary jupyter notebook](/api-example-notebooks/rest_api_qasm_cloud.ipynb)

## ❓ Questions and Support

If you have any questions or encounter difficulties during the integration process, please don't hesitate to open a GitHub issue in this repository. We are here to help and will address your concerns as quickly as possible. Your feedback is important to us, and we appreciate your contribution to improving our integration experience.

