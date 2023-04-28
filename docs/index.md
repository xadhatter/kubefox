# KubeFox

KubeFox is a software platform that makes creating, deploying, releasing, and
maintaining software applications on Kubernetes easier. It consists of a core
set of services for recording telemetry and storing configuration and a
framework to develop, test, and run components of an application. The KubeFox
Platform is highly opinionated greatly reducing its complexity and means there
is not endless configuration for it to work.

KubeFox is built on the concept of a system. Concretely, a KubeFox System is a
collection of Apps, which in turn are a collection of Components and Routes to
those Components. The definition of a System's Apps and Routes as well as the
source code of its Components is stored in a Git repository, referred to as a
System Repo.

A KubeFox System can be deployed to an instance of the Kubefox Platform running
on a Kubernetes Cluster. KubeFox will create Kubernetes Pods for each of the
System's Components. Even though the Components are running no requests will be
sent to the System until it is released. A System is released to an Environment
which defines configuration needed by the System's Components to process
requests. Once a System is released requests will be sent to it if they match
any of its Routes.

## Components

## Apps

### Routes

## System

Systems are a group of Apps, Components, and Routes. Despite best efforts, these
things often have strict dependencies. To ensure proper functionality compatible
versions of all three must be deployed and released together. Systems act as
immutable containers to ensure this happens.

A System Object can be created from Git commit Once all code and configuration
is ready and checked-in .

Once an instance of a System is created it is immutable. It  monolithic
packages. This guarantees that compatible versions of Components are always
deployed and release together.

### Repos

## Environments

> TODO detail concept of lightweight environments, etc.

### Configs

## Deployments

To deploy a System a specific state of that System must be used. Each commit to
the System Repo represents a state of the System that can be deployed. To record
that state the System's Components are compiled and packaged into OCI images.
The images are tagged with the commit hash from which the Component was built.
They are then pushed to a container registry. Next a System Object is generated
which is a JSON document containing the definitions of the System's Apps and
Routes, the image names of its Components including the tag and registry
hostname, and the commit hash from which the System Object was generated. The
System Object is then stored in KubeFox. A specific commit to the System Repo
can then be deployed to an instance of the KubeFox Platform by specifying the
System Object corresponding to that commit.

> TODO component set based deployments

### Refs

> TODO discuss need for refs (hashes are no good for hoomans), types of refs,
> and their use cases

## Releases

> TODO

## Events

> TODO concept of events, how they are represented, etc

### Genesis Events

### Event Based Configuration
