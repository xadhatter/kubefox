---
title: Requirements
filename: requirements.md
---

## Personas

- Software engineer
- Sysop engineer
- Release manager

### Alpha 1

1. Develop and run python component code using standard tools, no need for KubeFox specific tools
1. Develop a web service component in Python and route HTTP requests to the component
1. Develop a K8s controller component in Python and route Kubernetes events to the component allowing mutation and child resource creation
1. Log message that includes consistent metadata, can be linked with other telemetry, is durably stored, and can be searched
1. When calling another component have telemetry data attached, including creating child traces as needed, without additional code
1. Commit Python component code to a Git repo hosted on GitHub and have code built into docker container and publish container
1. Define a group of components that are interdependent and deploy them to Kubernetes using published containers

### Alpha 2

Define a unit test and have it automatically run during build, stopping the build if the test fails
Communicate with unmanaged component outside KubeFox

Secrets are automatically injected
Telemetry is automatically generated and stored
Secrets are injected when calling another component

#### Administration

View available components
View groups of interdependent components
View components currently deployed

### Beta 1

Develop a component in Javascript/Typescript and Go
Commit code to a git branch and have the component deployed to Kubernetes
Generate event on schedule

#### Security

Audit certain events
Explicit permission must be granted for component to communication (zero trust)

#### Telemetry

#### Administration

### Beta 2

#### Development

Develop code in Java
Create integration test using Python

#### CICD

#### Runtime

Scale components
Receive events of interest from external unmanaged components

#### Security

All connections between components is encrypted and trusted (mTLS)

#### Telemetry

#### Administration
