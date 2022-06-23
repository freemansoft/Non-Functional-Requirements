This repository is also visible as https://freemansoft.github.io/NFRs/

# NFRs
Non Functional Requirements are architecturally significant requrements because they impact the system architecture.  Non functional requirements impact the system as a whole and are cross cutting across business features.

------------------

# NFR Definitions
This document describes one way of describing NFRs. 
This format is designed to present NFRs _at a glance_
Those requiring more details in their NFRs should choose a different format.

## References
* [Wikipedia](https://en.wikipedia.org/wiki/Non-functional_requirement )
* [Scaled Agile Framework](https://www.scaledagileframework.com/nonfunctional-requirements/) 

## Various NFR Category Taxonomies
NFRs are often grouped with related NFRs into _Categories_.  There are many different published NFR categories and Category Type aggeregationsrs.  The links below show some Categories that are grouped under described Category Types

| Site                                                                                                         | Category Type | Individual Category                                                                                                         |
| ------------------------------------------------------------------------------------------------------------ | ------------- | --------------------------------------------------------------------------------------------------------------------------- |
| [Wikipedia](https://en.wikipedia.org/wiki/Non-functional_requirement )                                       | Execution     | safety, security and usability                                                                                              |
| [Wikipedia](https://en.wikipedia.org/wiki/Non-functional_requirement )                                       | Evolution     | testability, maintainability, extensibility and scalability                                                                 |
| [Modern Requirements](https://www.modernrequirements.com/blogs/pillar/what-are-non-functional-requirements/) | Operational   | Access/Security, Accessability, Availability, Confidentiality, Efficiency, Integrity, Survivability, Reliability, Usability |
| [Modern Requirements](https://www.modernrequirements.com/blogs/pillar/what-are-non-functional-requirements/) | Revisional    | Maintainability, Modifiability, Flexibility, Scalability, Verifiability                                                     |
| [Modern Requirements](https://www.modernrequirements.com/blogs/pillar/what-are-non-functional-requirements/) | Transitional  | Portability, Reusability, Interopability, Installability                                                                    |
| [Scaled Agile Framework](https://www.scaledagileframework.com/nonfunctional-requirements/)                   | _Not Grouped_ | Security, reliability, performance, maintainability, scalability, and usability                                             |

I couldn't find many Sub-Category examples so there ones in the sample below are ad-hoc following no model.
## Generic NFR Attributes Template

| Category                              | Sub-Category                      | Requirement Definition     | Possible Implementation (opt)                         | Validation Method                       |
| ------------------------------------- | --------------------------------- | -------------------------- | ----------------------------------------------------- | --------------------------------------- |
| Top Level Categories from a Taxonomy. | One or two word requirements name | Describing the requirement | A suggested implementation or organizational standard | How the NFR implementation is validated |

## Scaled Agile Framework Aligned (SAFe) Template
See the SAFe web site for the meanings of these fields. https://www.scaledagileframework.com/nonfunctional-requirements/
* Labels in *(parenthesis)* are Scaled Agile Framework aligned.

| Category                               | _(Sub-Category)_                  | Requirement Definition     | Possible Implementation                               | Validation Method                       |     | _(Meter)_          | Metric Units _(Scale)_ | Metric _(Target)_       | Metric Failure _(Constraint)_ | Metric _(Current)_   |
| -------------------------------------- | --------------------------------- | -------------------------- | ----------------------------------------------------- | --------------------------------------- | --- | ------------------ | ---------------------- | ----------------------- | ----------------------------- | -------------------- |
| Top Level Categories from a  Taxonomy. | One or two word requirements name | Describing the requirement | A suggested implementation or organizational standard | How the NFR implementation is validated |     | Metric calculation | metric value type      | Target value for metric | Failure value for metric      | Current metric value |

  
------------------

# NFRs using the Generic template
This table captures a set of NFRs using the generic template.  These NFRs are owned by delvery teams in _Shift Left_ Models. It does not include any operational concerns.

### Security
| Category         | Sub-Category             | Requirement Definition                                                                | Possible Implementation (opt)                                                 | Validation Method                                                                           |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Security         | Encryption in Transit    | All data must be encrypted when moving from one system to another                     | Use TLS transport                                                             | Automated tests that verifies HTTP is not usable and that https is usable                   |
| Security         | Encryption at Rest       | All data must be encrypted at rest on drives to prevent data loss                     | Use disk encryption                                                           | Verify all drives are encrypted                                                             |
| Security         | Encryption at Rest       | All Personally Identifyable Information must be encrypted in persistence stores.      | Use field level encryption                                                    | Scan PII fields in databases to verify encryption. Scan Event streams to verify encryption. |
| Security         | Authentication           | All APIs, services, Messaging endpoints must require Authentication                   | Use Corporate Identity Tokens                                                 | Create tests for each endpoint that verify anonymous access is not allowed                  |
| Security         | Authentication           | All Persistence  endpoints must require Authentication                                | Use Corporate Identity Tokens                                                 | Create tests for each endpoint that verify anonymous access is not allowed                  |
| Security         | Authorization            | All APIs, services, Messaging must restrict functionality via authorizations          | Implement RBAC tied to roles                                                  | Verify endpoints with authenticated users of different roles                                |
| Security         | Authorization            | All Persistence must restrict functionality via authorizations                        | Leverage RBAC tied to roles                                                   | Verify persistence stores with authenticated users of different roles                       |
| Security         | Analysis                 | Generate a Threat Analysis                                                            | Use threat analysis tool                                                      | Manually verify analysis                                                                    |
| Security         | Analysis                 | Deployable artifacts must be scanned for vulnerabilities                              | Scan all binary artifacts and containers when built                           | Break builds when artifact scanning fails                                                   |
| Security         | Configuration Management | Secrets must be stored in a place only reachable by the applications/tools            | Use Cloud provided Key Store                                                  | Scan deployments to verify no secrets                                                       |
| Security         | Configuration Management | Configuration settings must not be changeable by development teams                    | Use automation or have segregated teams                                       | Verify permissions with automated tools                                                     |
| Security         | Identity Management      | All services will use Identities from corporate Identity provider                     | All identities are stored in cloud Identity Provider                          | Audit                                                                                       |
| Security         | Identity Management      | Use identities that don't require secret storage and manipulation                     | Use Cloud asset bound identities instead of service accounts                  | Audit                                                                                       |

### Availability
| Category         | Sub-Category             | Requirement Definition                                                                | Possible Implementation (opt)                                                 | Validation Method                                                                           |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Availability     | Analysis                 | Teams must conduct a failure mode analysis (FMA) whenever components are added        | Manual Analysis                                                               | FMA review                                                                                  |
| Availability     | Analysis                 | Understand SLA                                                                        | Generate RTO/RPO numbers for all critical components                          | Chaos Testing for verification                                                              |
| Availability     | Resilience               | All data must be protected from drive or machine failure                              | Replicated data nodes in data stores                                          | Verify configurations                                                                       |
| Availability     | Resilience               | Data must be recoverable from outages                                                 | Implement policies for desired RTO / RPO                                      | Chaos testing                                                                               |
| Availability     | Resilience               | All compute services must survive node failures - Meet SLAs                           | Support auto restart and run extra compute                                    | Chaos testing or Automated testing                                                          |
| Availability     | Resilience               | All data services must meet RTO/RPO                                                   | Redundent copies in multiple data centers with failover                       | Chaos testing                                                                               |

### Visibility
| Category         | Sub-Category             | Requirement Definition                                                                | Possible Implementation (opt)                                                 | Validation Method                                                                           |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Visability       | Observability            | Logging - All logs are parameterized and are sent to log aggregator                   | Send structured (JSON) logs to Splunk                                         | Log aggregator dashboard                                                                    |
| Visability       | Observability            | Tracing - Cross service calls are tracable across services                            | Implement correlation IDs.  Send Open Tracing information to Trace aggregator | Dashboard                                                                                   |
| Visability       | Observability            | Metrics - Gather statistical metrics for all resource usage                           | Instrument applications. Send Metrics information to metrics aggregator       | Dashboard                                                                                   |
| Visability       | Observability            | Metrics - Gather statistical metrics for all  API calls                               | Instrument applications. Send Metrics information to metrics aggregator       | Dashboard                                                                                   |
| Visability       | Observability            | Audit - Generate Business level events for all actions - no PII                       | Audit or Message framework                                                    | Manual                                                                                      |
| Visability       | Recoverability           | Event Streams - Retain api invocations and messages for x days                        | Instrument APIs to send all requests a Events that are stored in big data     | Audit                                                                                       |
| Visibility       | KPI                      | Create Product Key Performance Indicator (KPI) dashboard                              | Create Product KPIs and generate metrics and dashboard                        | Metrics dashboard                                                                           |
| Visibility       | KPI                      | Create Technical Key Performance Indicator (KPI) dashboard                            | Create SLAs and leverage metrics and dashboard                                | Metrics dashboard                                                                           |

### Scalability
| Category         | Sub-Category             | Requirement Definition                                                                | Possible Implementation (opt)                                                 | Validation Method                                                                           |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Scalability      | Auto-Scale               | Size compute to load to save costs                                                    | Autoscale compute resources                                                   | Performance metrics                                                                         |

### Compliance
| Category         | Sub-Category             | Requirement Definition                                                                | Possible Implementation (opt)                                                 | Validation Method                                                                           |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Compliance       | Traceablilty             | Design time: Schema changes must be tracked, lineage captured                         | Schema versions are registered in a schema catalog                            | Deployment tools can verify registration                                                    |
| Compliance       | Traceability             | Design time: Data mutation operations must be tracked, lineage captured, regulated ML | Transforms are registered in schema catalog                                   | Tests can verify deployed transforms are registered                                         |
| Compliance       | Traceability             | Run time: Data origins are tracked, lineage captured                                  | Tag data sets or records with lineage information                             | tbd                                                                                         |
| Compliance       | Privacy                  | Support "Opt Out "                                                                    | Ivoke opt-out service before contacting                                       | Functional Test automation with opt-in and opt-out                                          |
| Compliance       | Privacy                  | Support "Where is my data"                                                            | Use data lineage to find a person's data                                      | Automated Functional tests with test data                                                   |

### Maintainability
| Category         | Sub-Category             | Requirement Definition                                                                | Possible Implementation (opt)                                                 | Validation Method                                                                           |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Maintainability  | Agility                  | API compatability                                                                     | Either version APIs or always support backwards compatability in APIs         | Automated functional tests for old and new capabilities                                     |
| Maintainability  | Agility                  | Deploy Early - Enable in Prod                                                         | Implement Feature Flags to turn on when ready                                 | Functional Test automation                                                                  |
| Maintainability  | Scalability              | Test at scale                                                                         | Implement load tests                                                          | Automated tests                                                                             |
| Maintainability  | Agility                  | Experiment in Prod                                                                    | Implement Blue/Grean                                                          | Experimental frameworks and test automation                                                 |
| Maintianability  | Agility                  | Zero infrastructure patching                                                          | Choose serverless. Automate infrastructure. Automate deployments.             | IaC automation automated testing                                                            |
| Maintainability  | Agility                  | Automate Infrastructure                                                               | Use provider APIs or templating engines to create infrastructure              | Chaos Monkey                                                                                |
| Maintainability  | Agility                  | Automate Application deployments                                                      | CI/CD                                                                         | Automated Functional Tests                                                                  |
| Maintainability  | Agility                  | Automate schema and data model deployments                                            | CI/CD for database schemas and streaming contract models                      | Automated functional testing                                                                |
| Maintainability  | Taceability              | Support runtime verification of version info for services and APIs                    | Add metadata endpoints to APIs.  Add schema version info to database schemas  | Automated functional testing                                                                |
| Maintainability  | Traceability             | Support runtime verification of for data schemas in persistence                       | Add metadata information to all schemas                                       | Automated functional testing                                                                |
| Maintainability  | Traceability             | Support runtime verification of messaging schemas                                     | Add metadata information or tagging to topics and queues                      | Automated functional testing                                                                |
| Maintainability  | Traceability             | Support runtime version information for deployed binaries and infrastructure          | Add metadata information or tagging to the deployment                         | Automated Functional Testing                                                                |
| Maintainability  | Testability              | All components must have automated tests                                              | Unit tests for all components                                                 | Run test metrics                                                                            |
| Maintainability  | Testability              | All endpoints are verified at deployment                                              | Implement functional tests for all endpoints and run on deployment            | Automated functional testing                                                                |

### Interoperability
| Category         | Sub-Category             | Requirement Definition                                                                | Possible Implementation (opt)                                                 | Validation Method                                                                           |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Interoperability | Language                 | Components must support approved name formats                                         | All components must support UTF-8 encoding                                    | Automated Functinal Tests with different characters                                         |
| Interoperability | Time                     | All dates times must not assume timezones                                             | All dates and times will  support timzone info                                |                                                                                             |

