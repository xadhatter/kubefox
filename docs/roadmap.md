# Roadmap

## v0.1.0-alpha

Focus: **Basic Functionality**

## v0.2.0-alpha

Focus: **Infrastructure Security**

1. Turn Vault into Certificate Authority and issue all required certs.
2. Integrate with Vault NATS plugin to issue JWT.
3. Create fine grain NATS account for each System/App combination.
4. Reduce complexity of telemetry collection by switching to OTEL collector.
5. Tighten Vault policies for brokers.
6. Add concept of tenant/organization.
7. Require components to request portion of environment and prune during
   weaving.

## v0.3.0-alpha

Focus: **Kit SDK**

## v0.4.0-alpha

Focus: **Platform Management**

1. Enforce configuration requirements for types

## v0.5.0-alpha

Focus: **Adapters**

## v0.6.0-alpha

Focus: **Platform Administration**

## v0.7.0-beta

Focus: **Stabilization**

## v0.8.0-beta

Focus: **Developer Usability**

## v0.9.0-beta

Focus: **Pup QDK**

## v0.10.0-beta

Focus: **Telemetry**

## Alpha2

**Infrastructure Projects**

1. QDK
   1. Hooks to spawn test events, record responses
      1. Activate globally or incrementally
      2. Group of built-in events provided with all installations
      3. Classify event types?
2. Move from Helm to Operator
   1. charts have proven inflexible
   2. yields greater control
   3. simpler problem determination and mitigation
3. Injection of OAuth
   1. May be complex
   2. Interface for user policy creation
4. Telemetry
5. Switch to fluentbit for all telemetry
6. Decision on Traefik

**Developer Support**

1. Federated docs
   1. Work off of community template
   2. Use CLOMonitor to ensure best doc practices for repository?
2. Automate Platform / Network setup
   1. Currently have to run broker, port forward, etc.

**Features**

1. QDK
   1. SDK support for user-developed events / messages
   2. Test framework
      1. Look at partner packages – e.g., Selenium, TestKube, Cypress etc.?
      2. Language-specific?
      3. Limit KubeFox involvement to KubeFox events – defer to external packages for generalized testing?
2. Dapr Support for state stores
   1. Relational DB
      1. Postgres ((initially – popularity, familiarity, robustness)
         1. Backup/Restore solution
         2. Availability solution
   2. Message Brokers
      1. Kafka (initially – popularity, familiarity, robustness)

**Beta 1**

**Infrastructure Projects**

1. Evaluation store for non-config resources (OpenSearch?)
   1. Query support

**Developer Support**

1. Code quality in preparation for open sourcing
2. Docs quality in preparation for open sourcing

**Features**

1. Rudimentary UI
2. KubeFox gateway hosted service
3. QDK
   1. Initial event debugger
      1. Event trace
      2. Event visualization
         1. Drill down
         2. Associated logs
         3. Associated metrics
         4. Associated code
      3. Integrated with SDK hooks

**Beta 2**

**Infrastructure Projects**

**Developer Support**

1. VS Code plug-in

**Features**

1. User management / permissions
2. Basic broker policies
3. Auditing
   1. QDK hooks
   2. Storage of audit records
4. Enhanced UI
   1. Need to break down the features

**v1.0**

**Infrastructure Projects**

1. KubeFox Platform
   1. Backup / Restore
   2. Availability

**Developer Support**

**Features**

1. Paid Service Offerings
   1. Managed Option
      1. Define what XigXog will and won’t do
   2. Multi-cluster support
   3. SSO
   4. Gated releases
   5. git flow based deployments

**v1.1 +**

**Infrastructure Projects**

**Developer Support**

**Features**

1. More complex deployment patterns
   1. Blue/Green
   2. Canary
2. Autoscaling
   1. Guardrails / Controls around ramp ups
   2. Scale-to-0
3. DR
