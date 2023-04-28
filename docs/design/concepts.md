# 

## What is KubeFox?

KubeFox is a runtime infrastructure coupled with an SDK and toolset which provide for the construction and deployment of very secure and robust applications – without burdening development teams with DevOps responsibities.  Consider the following list:


1. Zero Trust
2. Versioned deployments
3. Simpfied environment management
4. Robust and integrated metrics, tracing, logging, auditing, and monitoring


Imagine the development of applications possessed of these capabities - straight out of the box and without DevOps expertise.  That is what KubeFox provides.  Want to learn more?  Read on…


Before we continue, we wanted to accommodate those of you who “don’t need any stinking badges”.  Click this link to get started now!

________________

***This belongs on a different page:***

[Composition of Quick Start – exists on a separate page]
a)	Quick Start
i)	Builds KubeFox compliant GitHub repository
(1)	Seeds the repository with a simple web application
ii)	Launches micro k8s with a KubeFox-compliant cluster
iii)	Bootstraps the cluster
iv)	Provides a simple interface
v)	Provides a walkthrough of telemetry
vi)	Guidance on next steps / creating one’s own application

[Continuation of primary documentation]

_______________


## Building Blocks

We’ll start with brief descriptions of KubeFox constituents and gradually drop deeper into detail.

1. Cluster
   
	A cluster is a Kubernetes cluster source with the KubeFox platform installed.

2.  Repository

	A repository is a Git repository.  Repositories map 1:1 with Systems.

3. System
	
	A System is a collection of Applications. KubeFox employs the concept of System to help developers with the reuse of components across applications.

4. 	Application
	
	Applications are just what you think they are:  collections of modules which together provide useful capabities to end users of the software.

5. 	System Ref
	
	A System Ref (or SysRef) is a point-in-time snapshot of a repository – hence it also represents the System at a point in time.  The SysRef is the basis for the deployment of a System. Note that the most common version of a SysRef is a Git tag.

6. 	Components
	
	In the KubeFox world, application modules are either Components or Providers.  Components are user-written microservices or functions which provide capabilities to fulfill requirements of an application.  The same component can of course be used by one or more applications.

7. 	Broker
	
	A side-car service attached to a component that serves as a proxy for events, requests, and responses between that component and other components in the System.  Brokers provide a number of advanced services and capabities – we’ll dig more into them later.

8. 	Providers
	
	In the KubeFox world, application modules are either Components or Providers.  Providers are simply third-party services, e.g.:  databases (e.g., Postgres, Snowflake, MongoDB), Key/Value datastores, ML libraries etc.

9. 	Adapter
	
	Adapters are Provider-specific Brokers.  Adapters are to Providers what Brokers are to Components, and serve to proxy requests to third-party services.

10.	Deployment
	
	A deployment is a SysRef that has been loaded into a cluster.  The result of a deployment is that all constituents necessary to run a SysRef are available on the cluster.

11.	Soft Release
	
	A Soft Release launches all constituents of a SysRef; however, the SysRef is available only via explicit URLs.  By default, the Soft Release occurs at Deployment time, but it can be rendered separate and explicit.

12.	Release
	
	A Release is the activation of a SysRef - general external traffic will be routed to the released SysRef.

## High-Level Workflow

So let’s build a workflow from these building blocks…

Application developers build one or more applications that compose a System.  Their focus is on the Components that they develop, the selection of Providers and the interactions amongst those constituents.  They store all of their work in the Git repository – just as they do today.

At some point – to test, release to an environment – they determine that the System is ready for Deployment, and they create a SysRef using KubeFox.  KubeFox collects and builds all necessary constituents (Components, Providers, Brokers and Adapters), deploys them to the cluster, and spools up the pods needed to support the deployment.

At some point, the team determines that the deployment is ready for public consumption, and performs a release.

You may be thinking “Okay, fine – but what you’ve described is pretty standard and aligned with our current methodology.  What’s the difference?”.  A partial list:

- The infrastructure in which your components run is zero trust – and your team didn’t have to spend any time wiring up the system
- You can deploy and release multiple versions of your applications – even within an environment. KubeFox guarantees devery to the correct component version
- In seconds, you can release a new version of your system or fall back to a prior release
- You have immediate access to component-specific telemetry – including logs, metrics and traces – and as your applications evolve, that telemetry will evolve with them
- You possess the abity to make fine-grained changes to authorization policies – without code changes



# Concepts




## Events




KubeFox is built around event sourcing. All requests and responses, even synchronous calls, are modeled as events and are exchanged as messages via brokers.

[CloudEvents](https://github.com/cloudevents/spec/blob/v1.0.2/cloudevents/spec.md#type) are used to wrap all events passed thru the message broker. This includes things like requests/responses, audit records, etc.

To fulfil the requirement that the `source` attribute of the `CloudEvent` be a URI a KubeFox scheme is described below:

```plain
kubefox://[componentId.componentHash.]componentName
```

Where `componentId.componentHash` is optional. If provided `componentId` must be length 5 and `componentHash` must also be length 7. Both must only contain lowercase alpha-numeric characters.

As an example:

```plain
kubefox://6cqbp.ab883d2.joker
```

This is the URI to the component named `joker` with the id `6cqbp`.

An complete example of a `CloudEvent` request coming from the `api-gateway` component with a response coming from the `joker` component is provided below.

Request:

```yaml
specversion: 1.0
type: io.kubefox.http_request
id: 69c6aac6-f839-4c98-a380-3c3a79bb2788
time: 2022-04-12T23:20:50.52Z
source: kubefox://vwqbx.223aa15.api-gateway/
dataschema: kubefox.proto.v1.KubeFoxData
datacontenttype: application/protobuf
```

Response:

```yaml
specversion: 1.0
type: io.kubefox.http_response
id: abb8ab78-c78e-4ab2-acf0-5fae242a5b6e
requestid: 69c6aac6-f839-4c98-a380-3c3a79bb2788
time: 2022-04-12T23:20:51.10Z
source: kubefox://6cqbp.ab883d2.joker/
dataschema: kubefox.proto.v1.KubeFoxData
datacontenttype: application/protobuf
```

### Common Attributes

These are common attributes that are included in events, logs, and traces to allow correlation.

| Name           | Type            | Description | Cloud Event Key               | Log Key         | Trace Key                |
| -------------- | --------------- | ----------- | ----------------------------- | --------------- | ------------------------ |
| SysRef     | git hash, len 7 |             |                               | `systemTag`     | `kubefox.system-tag`     |
| Environment    | string          |             |                               | `environment`   | `kubefox.environment`    |
| Component Id   | string, len 5   |             |                               | `componentId`   | `kubefox.component-id`   |
| Component Name | string          |             |                               | `componentName` | `kubefox.component-name` |
| Component Hash | git hash, len 7 |             |                               | `componentHash` | `kubefox.component-hash` |
| Event Id       | UUID            |             | `id`                          | `eventId`       | `kubefox.event-id`       |
| Event Type     | string          |             | `type`                        | `eventType`     | `kubefox.event-type`     |
| Event Source   | URI             |             | `source`                      | `eventSource`   | `kubefox.event-source`   |
| Event Target   | URI             |             | `data.metadata.target`        |                 |                          |
| Trace Id       | 64-bit uint     |             | `data.metadata.span.trace_id` | `traceId`       | `trace`                  |
| Span Id        | 64-bit unit     |             | `data.metadata.span.span_id`  | `spanId`        | `span`                   |

### Event Types

#### Request/Response

- `io.kubefox.component_request`
- `io.kubefox.component_response`
- `io.kubefox.cron_request`
- `io.kubefox.cron_response`
- `io.kubefox.dapr_request`
- `io.kubefox.dapr_response`
- `io.kubefox.http_request`
- `io.kubefox.http_response`
- `io.kubefox.kubernetes_request`
- `io.kubefox.kubernetes_response`
- `io.kubefox.metrics_request`
- `io.kubefox.metrics_response`
- `io.kubefox.telemetry_request`
- `io.kubefox.telemetry_response`

#### Audit

- `io.kubefox.audit_record`

#### Errors

- `io.kubefox.error`
- `io.kubefox.rejected`

### Message Subjects

| Event Type | Subject                                    |
| ---------- | ------------------------------------------ |
| Request    | `{componentName}.{componentHash}.req`      |
| Response   | `{componentName}.{componentHash}.{id}.res` |

## Cluster
Components are primitives of the KubeFox platform. They are services that provide capabilities to fulfil requirements of an application. Entrypoints are registered with KubeFox using the Kit SDK which are invoke by incoming events to the component. The source code is stored in a registered Git repository that follows the KubeFox conventions. Building, packing, testing, and execution of the component is handled by KubeFox.

### Providers

Providers are services that exist outside of the control of KubeFox. A common example of a provider is a state store such as a database but things like 3rd party HTTP services are also applicable. Even though KubeFox does not completely control Providers, they still must be registered with KubeFox. This allows requests proxied through brokers to perform access control, secret injection, and telemetry recording. Events from Providers are translated to KubeFox compliant events by an adapter.

Based on the type of Provider certain variables must be specified in the Environment.

### Adapter

Adapters bridge providers and KubeFox.

## Application

An Application is a logical group of one or more components that satisfy a set of features. The same component can be used by one or more applications. Components not assigned to an application will not be deployed.

Components within an application are able to communicate with each other without explicit permission. Communication between applications requires explicit authorization and is allowed on a component to component level.

## System

A group of one or more applications. Systems are represented by Git repositories. All managed components in the Git repository are part of the system.

## System Ref

An immutable snapshot of a System at a particular moment in time created from a Git ref. The System Ref includes the container image hash of the components built at that Git ref. A System Ref can be created from a commit to a branch or a tag.

## Environment

A lightweight set of configuration that provide values for application variables. Multiple systems and multiple tags of the same system can be assigned to a environment. When multiple tags of the same system are assigned one of the tags must be designated as the default.

## Variable

Variables are tuples that provide a key, value, and default value. They are defined as part of the application specification. Their values are provide by Environments. These variables are available to components at runtime and can be used in configuration and policies.

## Broker

A side-car service attached to a component that proxies events, requests, and responses between the KubeFox message transport and the component. When an event is generated by the attached component the broker is responsible for applying applicable policies, attaching relevant metadata, and to ensure proper routing. Additionally the broker is responsible for gathering context, such as variables and secrets, and providing it to the component.

## Route

Routes are used to send events from external sources to a component of a particular SysRef. A route includes a set of criteria and a target component. A target can be an environment, SysRef, or component. Route criteria are expressed in adapter specific language and are processed when an event is received by the adapter.

Routes are matched once at event genesis. The target component as well as its application, system version, and environment are attached to the event to provide context for future routing of events generated by downstream components. This context is needed as a single component instance might be shared between multiple deployed SysRefs. When a downstream component makes a call the of the context of the genesis event is used to ensure the next component being called is the correct version.

Routes optionally can contain logic to extract parts of the event into well name arguments which are passed to the component as key/value pairs.

### Environment Route

Environment routes are used to match an event to an environment and optionally to a specific SysRef in the environment. They are processed first, before application routes. If no environment routes match then the default environment of the cluster and the default SysRef of the environment is used before applying application routes.

### Application Route

Application routes are used to match events and send them to a specified component. They are processed after a SysRef is selected by environment routes. If no application routes match then a `Not Found` error is returned to the adapter.

## Subscription

Subscriptions are used to match messages and then send an event to a component. Like routes, subscriptions contain a set of criteria to determine if an event is relevant.

## Deployment

A deployment is a SysRef that has been applied to a cluster. The result of a deployment is that all the components of the SysRef are running on the cluster.

## Release

A release is when a deployment is activated which allows traffic to flow to the components of the SysRef.
