---
title: "SIMAP: Concept, Requirements, and Use Cases"
abbrev: SIMAP Concept & Needs
docname: draft-ietf-nmop-simap-concept-latest
category: info

submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Network Management Operations"
keyword:
 - Service & Infrastructure Maps
 - Service emulation
 - Automation
 - Network Automation
 - Orchestration
 - service delivery
 - Service provisioning
 - service flexibility
 - service simplification
 - Network Service
 - Digital Map
 - Emulation
 - Simulation
 - Topology
 - Multi-layer

author:
  -
    fullname: Olga Havel
    org: Huawei
    email: olga.havel@huawei.com

  -
    fullname: Benoit Claise
    org: Huawei
    email: benoit.claise@huawei.com

  -
    fullname: Oscar Gonzalez de Dios
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com

  -
    fullname: Thomas Graf
    org: Swisscom
    email: thomas.graf@swisscom.com

contributor:

  -
    fullname: Ahmed Elhassany
    org: Swisscom
    email: Ahmed.Elhassany@swisscom.com


normative:

informative:

--- abstract

This document defines the concept of Service & Infrastructure Maps (SIMAP) and identifies a set of SIMAP
requirements and use cases. The SIMAP was previously known as Digital Map.

The document intends to be used as a reference for the assessment of the various topology modules to meet
SIMAP requirements.

--- middle

# Introduction

Service & Infrastructure Maps (SIMAP) is a data model that provides a view of the operator's networks and services,
including how it is connected to other models/data (e.g., inventory, observability sources, and
operational knowledge). It specifically provides an approach to model multi-layered topology
and an appropriate mechanism to navigate amongst layers and correlate between them.
This includes layers from physical topology to service topology.
This model is applicable to multiple domains (access, core, data center, etc.) and
technologies (Optical, IP, etc.).

The SIMAP modelling defines the core topological entities (network, node, link, and termination point) at each layer,
their role in the network topology, core topological properties, and topological relationships
both inside each layer and between the layers. It also defines how to access other external models
from a topology. The SIMAP model is a topological model that is linked to other functional
models and connects them all: configuration, maintenance, assurance (KPIs, status, health, and symptoms),
Traffic-Engineering (TE), different behaviors and actions, simulation, emulation, mathematical abstractions,
AI algorithms, etc. These other models exist outside of the SIMAP and are not defined during SIMAP modelling.

The SIMAP data consists of virtual instances of network and service topologies at different layers.
The SIMAP provides access to this data via standard APIs for both read and write access, typically as a nortbound
interface from a controller, with query capabilities and links to other YANG modules (e.g., Service Assurance for
Intent-based Networking (SAIN) {{?RFC9417}}, Service Attachment Points (SAPs) {{?RFC9408}},
Inventory {{?I-D.ietf-ivy-network-inventory-yang}}, and potentially linking to non-YANG models).
The SIMAP also provides write operations with the same set of APIs, not to change a topology layer
on the fly as a northbound interface from the controller, but for offline simulations, before applying
the changes to the network via the normal controller operations.

Both read and write APIs are similar, stemming from the same YANG model, to facilitate the comparison
of the offline simulated SIMAP with the network one.


# Terminology

This document makes use of the following terms:

Topology:
: Topology in this document refers to the network and service topology.
  A network topology defines how physical or logical nodes, links and
  termination points are related and arranged. A Service topology defines how
  service components (e.g., VPN instances, customer interfaces, and
  customer links) between customer sites are interrelated and
  arranged.
: There are several types of topologies: point-to-point,
  bus, ring, star, tree, mesh, hybrid, and daisy chain.
: Topologies may be unidirectional or bidirectional (bus, some
  rings).

Multi-layered topology:
: A multi-layered topology models relationships between different topology layers,
  where each layer represents a connectivity aspect of the network
  and services that needs to be configured, controlled and monitored.
  Each topology layer has a separate lifecycle.

: {{?RFC8345}} also refers to this multi-layered topology as topology hierarchy (Stack). It
  also uses layers when describing supporting relations (represent layered network topologies),
  underlay/overlay, network nodes and layering information. {{?RFC8345}} states that the model can be
  used for representation of layered network topologies.

: {{?RFC8345}} is flexible and can support both the same network topology instance with multiple layers (e.g., Layer 2 and Layer 3)
  or separate network topology instances with supporting relations between them (e.g., separate Layer 2 and Layer 3).
  Therefore, multiple topology layers can be grouped into the same network topology instance, if solution requires.

Topology layer:
: A topology layer represents topology at a single layer in the multi-layered topology.
: The topology layer can also represent what needs to be managed by a
  specific user or application, for example the IGP layer can be of interest to the operator
  troubleshooting or optimizing the routing, while the optical layer may be
  of interest to the user managing the optical network.
: Some topology layers may relate closely to OSI layers, like Layer 1 topology
  for physical topology, Layer 2 for link topology and Layer 3 for IPv4 and
  IPv6 topologies.
: Some topology layers represent the control aspects of Layer 3, like OSPF, IS-IS, or BGP.
: The service layer represents the service view of the connectivity, that can differ for
  different types of services and for different providers/solutions.
: The top layer represents the application/flow view of service connectivity.

Service:
: A service represents network connectivity service provided over a network that enables devices, systems, or networks to
communicate and exchange data with each other. It provides the underlying infrastructure and mechanisms
necessary for establishing, maintaining, and managing connections between different endpoints.
The example services are: L2VPN, L3VPN, EVPN, VPLS, VPWS,

Resource:
: Defined in {{?I-D.ietf-nmop-terminology}}

Termination Point:
: Defined in {{?RFC8345}}, as follows:
: The network-topology module defines a topology graph and components from which it is
composed: nodes, edges, and termination points.  Nodes (from the "ietf-network" module) represent
graph vertices and links represent graph edges.  Nodes also contain termination points that anchor the
links.
: A node has a list of termination points that are used to terminate links. An example of a termination point might
be a physical or logical port or, more generally, an interface.
Like a node, a termination point can in turn be supported by an underlying termination point, contained in the
supporting node of the underlay network.


The document defines the following terms:

Service & Infrastructure Maps (SIMAP):
: SIMAP is a data model that provides a view of the operator's networks and services,
  including how it is connected to other models/data (e.g., inventory, observability sources, and
  operational knowledge). It specifically provides an approach to model multi-layered topology
  and an appropriate mechanism to navigate amongst layers and correlate between them.
  This includes layers from physical topology to service topology.
: This model is applicable to multiple domains (access, core, data centers, etc.) and
  technologies (Optical, IP, etc.).

SIMAP modelling:
: SIMAP modelling is the set of principles, guidelines, and conventions to model the SIMAP
  using the IETF modelling approach {{?RFC8345}}. They cover the
  network types (layers and sublayers), entity types, entity roles
  (network, node, termination point, or link), entity properties,
  relationship types between entities and relationships to other entities.

SIMAP model:
: A SIMAP model defines the core topological entities, their role in the network,
  core topological properties, and relationships both inside each layer and
  between the layers.
: It is the basic topological model with references/pointers to other models and connects them all:
  configuration, maintenance, assurance (KPIs, status, health, symptoms, etc.), traffic engineering,
  different behaviors, simulation, emulation, mathematical abstractions, AI algorithms, etc.

SIMAP data:
: SIMAP data consists of instances of network and service topologies at
   different layers.  This includes instances of networks, nodes,
   links and termination points, topological relationships between
   nodes, links and termination points inside a network,
   relationships between instances belonging to different networks,
   links to functional data for the instances, including
   configuration, health, symptoms.
: The SIMAP data can be historical, real-time, or future data for 'what-if' scenarios.

# Sample SIMAP Use Cases

The following subsections provides a non-exhaustive list of SIMAP use cases.

## Common Enablers for SIMAP

This section identifies a set of enablers that are invoked when providing the various business-oriented SIMAP use cases.
These enablers are grouped here to avoid duplication.

### Service -> Resource

The SIMAP API can be be invoked to retrieve all services for selected network types.
An application that triggers such a request will be able to retrieve the topology for selected services via
the SIMAP API and, from the response,
it will be able to navigate via the supporting relationship top-down to the lower layers. In doing so,
the application will be able to
determine what logical resources are used by a service. The supporting relations to the lowest layer will help
the application to determine what physical resources are used by the service.

### Resource -> Service

An application can navigate from the physical, Layer 2, or Layer 3 topology to the services that rely upon specific
resources. For example, the application will be able to select the resources and by navigating the supporting
relationship bottom-up come to the service and its nodes, termination points and links.

This API can be invoked for service impact analysis, for example.

### Traffic Engineering (TE)

Traffic Engineering (TE) {{?RFC9522}} is a network optimization technique designed to enhance network performance
and resource utilization by intelligently controlling the flow of data, for example by enabling dynamic path
selection based on constraints such as bandwidth availability, latency, and link costs.

Its primary goals are to prevent network congestion, balance traffic loads, and ensure efficient use of
bandwidth while meeting performance requirements.

The TE use case is a combination of both the capacity planning and the simulation use case. Therefore, there
are no specific SIMAP requirements.

### Closed Loop

A network closed loop refers to an automated and intelligent system where network operations are continuously
monitored, analyzed, and optimized in real time through feedback mechanisms. This self-adjusting cycle ensures
that the network dynamically adapts to changes, resolves issues proactively, and maintains optimal performance
without manual intervention.

Key Characteristics of a Network Closed Loop:

* Real-Time Monitoring: Collects data from network devices, traffic flows, and applications to build
a comprehensive view of network health and performance.
* Automated Analysis: Ideally leverages AI and machine learning to identify anomalies, predict potential failures,
or detect security threats.
* Proactive Action: Automatically triggers corrective measures, such as reconfiguring devices, isolating
compromised endpoints, or rerouting traffic.
* Continuous Optimization: Uses feedback from previous cycles to refine network policies and improve future responses.

The application will be able to retrieve a topology layer and any network/node/termination point/link instances
from the controller via the SIMAP API and from the response it will be able to map the traffic analysis to
the entities (typically links and router) for automated analysis. The corrective measures would be applied,
either directly to the network by managing the SIMAP entities (network/node/termination point/link instances)
or by first validating the corrective measure in an offline simulation (see the simulation and
traffic engineering use cases).

## Inventory Queries

A network inventory refers to a comprehensive record or database that tracks and documents all the network
components and devices within an organization's IT infrastructure.

Key elements typically found in a network inventory include:

* Hardware Details:
: Descriptions of physical devices such as routers (including their internal components such as cards, power supply
units, pluggables), switches, servers, network cables, including model numbers, serial numbers, and manufacturer
information. This information will facilitate locating additional details of the hardware in the manufacturer systems
and the correlation with the purchase catalog of the company.

* Software and Firmware:
: Versions of operating systems, network management tools, and firmware running on network devices.
Note that a network device can have components with their own software and firmware.

* Licensing Information:
: For any licensed software or devices, the network inventory will track license numbers, expiry dates, and compliance.

A network inventory lifecycle refers to the stages a network device or component goes through from
its introduction to the network until its removal or replacement. It encompasses everything from acquisition and
deployment to maintenance, upgrade, and eventually decommissioning. Managing the network inventory lifecycle
efficiently is crucial for maintaining a secure, functional, and cost-effective network.

A well-maintained network inventory helps organizations with network management, troubleshooting, asset tracking,
security, and ensuring compliance with regulations. It also helps in scaling the network, planning upgrades,
and responding to issues quickly.  In order to facilitate the planning and troubleshooting processes it is
necessary to be able to navigate from network inventory to network topology and services.

The application will be able to retrieve physical topology from the controller via the SIMAP API and from the
response it will be able to retrieve the physical inventory of individual devices and cables.

The application may request either one or multiple topology layers via the SIMAP API and from the response
it will be able to retrieve both physical and logical inventory.

For Access network providers the ability to have linkage in the SIMAP of the complete network (active + passive) is
essential as it provides many advantages for optimized customer service, reduced Mean Time To Repair (MTTR), and
lower operational costs through truck roll reduction.
For example, operators may use custom-tags that are readily available for a customer-facing device, then query
the inventory based on that tag to correlate it with the inventory and then map it to the network/service topology.
The mapping and correlation can then be used for triggering appropriate service checks.

## Service Placement Feasibility Checks {#sec-feasibility}

Service placement feasibility checks refer to the process of evaluating whether a specific service can be deployed
and operated effectively in a given network. This includes accessing the various factors to ensure that the
service will function as intended (e.g., based on traffic performance requirements) without causing network disruptions
or inefficiencies and effecting other services already provisioned on the network.

Some of the factors that need assesing are network capabilities, status, limitations, resource usage and availability.
The service could be simulated during the feasibility checks to identify if there are any potential issues.
The load testing could be done to evaluate performance under stress.

The Service Feasibility Check application will be able to retrieve the topology at any layer from the controller
via the SIMAP API and from the response it will be able to navigate to any other YANG modules outside of the
core SIMAP topology to retrieve any other information needed, such as resource usage, availability, status, etc.

## Intent/Service Assurance

Network intent and service assurance work together to ensure that the network aligns with business goals and
that the services provided meet the agreed-upon Service Level Agreements (SLAs).

The Service Assurance for Intent-Based Networking Architecture (SAIN) {{?RFC9417}} approach emphasizes
a comprehensive view of components involved in service delivery, including network devices and functions,
to effectively monitor and maintain service health.

The key objectives of this architecture include:

* Holistic Service Monitoring:
: By considering all elements involved in service delivery, the architecture enables a thorough assessment of
service health.

* Correlation of Service Degradation:
: It assists in linking service performance issues to specific network components, facilitating precise
identification of faults.

* Impact Assessment:
: The architecture identifies which services are affected by the failure or degradation of particular
network components, aiding in prioritizing remediation efforts.

When a service is degraded, the SAIN architecture will highlight where to look in the assurance service graph,
as opposed to going hop by hop to troubleshoot the issue.
More precisely, the SAIN architecture will associate a list of symptoms originating
from specific SAIN subservices to each service instance, corresponding to components of the network.
These components are good candidates for explaining the source of a service degradation.

The application will be able to retrieve a topology layer and any network/node/termination point/link instances
from the controller via the SIMAP API and from the response it will be able to determine the health of each instance
by navigating to the SAIN subservices and its symptoms.

## Service E2E and Per-link KPIs

The application will be able to retrieve a topology at any layer from a controller via the SIMAP API and from the
response it will be able to navigate to and retrieve any KPIs for selected topology entity.

## Capacity Planning

Network Capacity Planning refers to the process of analyzing, predicting, and ensuring that the network has sufficient
capacity (e.g., {{?RFC5136}}), resources, and infrastructure to meet current and future demands. It involves
evaluating the network's ability to handle increasing (including forecasted) amounts of data, traffic, and users'
activity, while maintaining acceptable levels of performance, reliability, and security.

The capacity planning primary goal is to ensure that a network can support business operations, applications, and
services without interruptions, delays, or degradation in quality. This requires a thorough understanding of the
network's current state, as well as future requirements and growth projections.

Key aspects of network capacity planning include:

* Traffic analysis: Monitoring and analyzing network traffic patterns to identify trends, peak usage periods, and areas
of congestion. For example, by generating a core traffic matrix with IPFIX flow record {{?RFC7011}} or deducting
an approximate traffic matrix from the link utilization data.
* Resource utilization: Evaluating the link utilization throughout the network for the current demand to identify
bottlenecks and potential QoS peformance issues.
* Growth forecasting: Predicting future network growth based on business expansion, new applications, or changes in
users' behavior.
* What-if scenarios: Creating models to assess the network behavior under different scenarios, such as increased traffic,
failure conditions (link, router or Shared Risk Resource Group), and new application deployments (such as a new
Content Delivery Network source, a new peering point, a new data center...).
* Upgrade planning: Identifying areas where upgrades or additions are needed to ensure that the network can minimize the
 effect of node/link failures, mitigate QoS problems, or simply to support growing demands.
* Cost-benefit analysis: Evaluating the costs and benefits of upgrading or adding new resources to determine the most
cost-effective solutions.

By implementing a robust capacity planning process, organizations can:

* Ensure better network reliability: Minimize downtime and ensure that the network is always available when needed.
* Improve performance: Optimize network resources to support business-critical applications and services.
* Optimize costs: Avoid unnecessary over-provisioning by making informed decisions based on data-driven insights.
* Support business growth: Scale the network to meet increasing demands and support business expansion.

The application will be able to retrieve a topology layer and any network/node/termination point/link instances from
the controller via the SIMAP API and from the response it will be able to map the traffic analysis to the entities
(typically links and router), evaluate their current utilization, evaluate which elements
to add to the network based on the growth forecasting, and finally perform the 'what-if' failure analysis by
simulating the removal of link(s) and/or router(s) while evaluating the network performance.

## Network Design

Network design involves defining both the logical structure, such as access, aggregation, and core layers, and
the physical layout, including devices and links.

It serves as a blueprint, detailing how these elements
interconnect to deliver the intended network behavior and functionality. The application will retrieve a
candidate network topology as the initial design, which can then undergo further analysis (e.g., perform traffic flow
simulations to identify bottlenecks and redundancy checks to ensure resilience) before being transformed into
actionable intent and, eventually, deployment actions.

Throughout the network's lifecycle, the design rules
embedded within a topology can be continuously validated. For example, a link rule might specify that a connection
between core and aggregation layers must have its source(s) and destination(s) located within the same data center.
Another example is to declare that a specific link type should only exist between Core <-> Aggregation layer with
certain contraints on port optic speed, type (LR vs SR for instance), etc.

The application can (via SIMAP API):

* Write the proposed network interconnect (topology + rules), this is a new potential network interconnect.
One network (in case of small network) or interconnect of multiple networks (bigger networks).

* Write the intended network interconnect (topology + rules), this is the intent of the network topology that cannot
be retrieved from the real network (e.g. our L2 topology interconnect intent, or L3 topology interconnect intent).
One network (in case of small network) or interconnect of multiple networks (bigger networks).

* Retrieve the proposed network interconnect (topology + rules)

  * Use case can be for purpose of traffic simulation, testing behavior under failures. Network simulation
use case is described in {{sec-emule}}.
  * Use case can be for purpose of comparing different proposed network interconnects.
  * Use case can be to build a simulated environment using this design. Network simulation
use case is described in {{sec-emule}}.

* Retrieve the intended network interconnect (topology + rules)

* At any point in time, compare the discovered topology with intended one

  * Potentially validating discovered device configurations with intended ones assuming SIMAP has the
external reference to configuration from topology.



## Network Simulation and Network Emulation {#sec-emule}

Network simulation is a process used to analyse the behaviour of networks via software. It allows network engineers
and researchers to assess how the network protocols work under different conditions, such as different topologies,
traffic loads, network failures, or the introduction of new devices. Network emulation, on the other hand,
replicates the behavior of a real-world network, allowing for more realistic analysis compared to network simulation.
While network simulation focuses on modeling and approximating network behavior, network emulation involves creating
a real-time, functional network environment whose protocols behave exactly like a real network. Ideally, network
emulation uses the same software images as the real network, but it could also be peformed (with less accuracy)
using generic software.

### Types of Network Simulation

There are several types of network simulations, each designed to address specific needs and use cases. Below are
the main categories of network simulation:

1. Discrete Event Simulation (DES):
: This is the most common type of network simulation. It models a series of events that occur at specific points
in time. Each event triggers a change in the state of a network component (e.g., a link is down, a card fails,
or a packet arrives).

2. Continuous Simulation:
: In contrast to discrete event simulation, continuous simulation models systems where variables change continuously
over time. Network parameters like bandwidth, congestion, and throughput can be treated as continuous functions.
: The main use case is to model certain aspects of network performance that evolve continuously, such as link speeds
or delay distributions in links that are impacted by environmental conditions (such as microwave or satellite links).

3. Monte Carlo Simulation:
: This type of simulation uses statistical methods to model and analyze networks under uncertain or variable conditions.
Monte Carlo simulations generate a large number of random samples to predict the performance of a network across
multiple scenarios. It is used for probabilistic analysis, risk assessment, and performance evaluation under
uncertain conditions.

### Goals of Network Simulation

The simulations can be also classified depending on the goal of the simulation.

####  Network Protocol Analysis

This type of simulation focuses on simulating specific networking protocols (IS-IS, OSPF, BGP, SR) to understand
how they perform under different conditions. It models the protocol operations and interactions among devices in
the network. For example, simulation can be used to assess the impact of changing a link metric. Moreover, specific
features of the networking protocol can be tested. For example, how fast-reroute performs in a given network topology.

#### Traffic Simulation

This simulation focuses on modelling traffic flow across the network, including packet generation, flow control,
routing, and congestion. It aims to evaluate traffic's impact on network performance.

The main use is to model the impact of different types of traffic (e.g., voice, video, mobile data, web browsing) and
understand how they affect the network's bandwidth and congestion levels. It can be used to identify bottlenecks and
assist the capacity planning process.

#### Simulation of Different Topologies Under Normal and Failure Scenarios

This type of simulation focuses on the structure and layout of the network itself. It simulates different network
topologies, such as mesh, horse-shoe, bus, star, or tree topologies, and their impact on the network's performance.
It can be used, together with the traffic simulation, to evaluate the most efficient topology for a network under
normal conditions and considering factors like fault tolerance.

## Postmortem Replay

The postmortem replay use case consists of using SIMAPs for the purpose of analysis of network service property
evolution based on recorded changes. A collection of relevant timestamped network events, such as routing updates,
configuration changes, link status modifications, traffic metrics evolution, and service characteristics, is being
made accessible from and/or within a SIMAP to support investigation and automated processing.
Using a structured format, the stored data can be replayed in sequence, allowing network operators to examine
historical network behavior, diagnose issues, and assess the impact of such events on service assurance.

The mechanism supports correlation with external data sources to facilitate comprehensive post-mortem analysis.
Beyond centralizing and correlating such various sources of information, the SIMAP can provide simulation of
the network behaviour to assist investigations.

In essence, this use case builds upon a collection of other SIMAP use cases, such as inventory queries,
intent/service assurance, service KPIs, capacity planning, and simulation, to provide a thorough understanding of
a network event impacting service assurance.

Note that this use case may serve as a component of Service Disruption Detection fine tuning as described in
{{?I-D.ietf-nmop-network-anomaly-architecture}}.

## Network Digital Twin (NDT)

Per {{?I-D.irtf-nmrg-network-digital-twin-arch}}, Network Digital Twin (NDT) is a digital representation that is
used in the context of Networking and whose physical counterpart is a data network (e.g., provider network or
enterprise network). Also, as discussed in {{Section 9.2 of ?I-D.irtf-nmrg-network-digital-twin-arch}}, network element
models and topology models help generate a virtual twin of the network according to the network element configuration,
operation data, network topology relationship, link state and other network information. The operation status can be
monitored and displayed and the network configuration change and optimization strategy can be pre-verified.

{{Section 9.4 of ?I-D.irtf-nmrg-network-digital-twin-arch}} further elaborates on the requirements on various
interfaces:

   * Network-facing interfaces are twin interfaces between the real network and its twin entity.
     They are responsible for the information exchange between a real network and NDT. SIMAP API can be invoked within
     such interfaces.

   * Application-facing interfaces are between the NDT and applications. They are responsible for the information
     exchange between Network Digital Twin and network applications. SIMAP API can be used for feasibility checks
     ({{sec-feasibility}}) or emulation ({{sec-emule}})).

{{Section 9.4 of ?I-D.irtf-nmrg-network-digital-twin-arch}} recommends that these interfaces are open
and standardized so as to avoid either hardware or software vendor lock and achieve interoperability.

# SIMAP Requirements

The SIMAP requirements are split into three groups for different target audiences:

* Operator requirements:
: These requirements are collected from the operators. They are functional requirements derived from the operators'
use cases. Some of the more specific semantic requirements were identified as {{!RFC8345}} gaps during the Hackathons
with operators and added as specific semantic requirements to the operator use cases.

* Design requirements:
: These requirements are derived from the operators' requirements. Although there is some duplication,
these are focused on summarizing the operators' requirements for the design of the data model and API.
These are functional requirements translated into low-level requirements for the model designers.
The rationale for adopting this approach is to ensure that the data model is designed according to the operators'
requirements and that they could be used for both design and review of the candidate YANG module(s).

* Architecture requirements:
: Architectural (non-functional) requirements are captured as well, as operators identified performance needs,
large scale support,  and network discovery. These are not data model requirements, but are requirements
either to drive the API design itself (e.g., to better optimize performance) or for the network controllers and
orchestrators that expose a SIMAP API. Although, they may be common sense requirements
not specific to SIMAP API,  they are listed here for completeness.


## Operator Requirements

The following are the operators' requirements for the SIMAP. Note that some of these requirements are supported by
default by {{!RFC8345}}.

REQ-BASIC-MODEL-SUPPORT:
: Basic model with network, node, link, and termination point entity types.

: This means that users of the SIMAP model
must be able to understand a topology model at any layer via these core concepts only,
without having to go to the details of the specific augmentations to understand the topology.

REQ-LAYERED-MODEL:
: Topology layers from physical layer up to service layer.

: SIMAP must provide views for all layers of network topology, from physical network
(ideally optical), Layer 2, Layer 3 up to  service and intent views. It must provide flexibility
to support both the same network topology instance with multiple layers (e.g., Layer 2 and Layer 3)
or separate network topology instances with supporting relations between them (e.g., separate Layer 2 and Layer 3).
Multiple topology layers can be grouped into the same network topology instance, if solution requires.

REQ-VIEWPOINTS:
: SIMAP should provide different views to different applications. For example, one application
may need to see Layer 2 and Layer 3 layers in a single network topology instance, while another application may need to see
them as separate network topology instances.

REQ-PASSIVE-TOPO:
: SIMAP must support capability to model topology of the complete network, including active and passive parts.
: For Access network providers the ability to have linkage in the SIMAP of the complete network (active + passive) is
essential as it provides many advantages for optimized customer service, reduced MTTR, and lower operational costs
through truck roll reduction.
: The passive topology must be either implemented in the SIMAP (what cannot be discovered can be added using the write API)
or accessible from the SIMAP. Whether the passive topology is included as part of the SIMAP or
accessible from the SIMAP is left to the solutions.

REQ-PROG-OPEN-MODEL:
: Open and programmable SIMAP.
: This includes "read" operations to retrieve the view of the network, typically as application-facing interface of
Software Defined Networking (SDN) controllers or orchestrators.
: It also includes "write" operations, not for the ability to directly change the SIMAP data
(e.g., changing the network or service parameters), but for offline simulations, also known as what-if scenarios.
:  Running a "what-if" analysis requires the ability to take
snapshots and to switch easily between them.
: Note that there is a need to distinguish between a change on the SIMAP
for future simulation and a change that reflects the current reality of the network.

REQ-STD-API-BASED:
: Standard based SIMAP models and APIs, for multi-vendor support.
:  SIMAP must provide the standard YANG APIs that provide for read/write and queries.
These APIs must also provide the capability to retrieve the links to external data/models.

REQ-COMMON-API:
: Common SIMAP models and APIs, for multi domain.
: SIMAP models and APIs must be common over different network domains (campus, core, data center, etc.).
: This means that clients of the SIMAP API must be able to understand the topology model of layers of any
domain without having to understand the details of any technologies and domains.

REQ-GRAPH-TRAVERSAL:
: Topology graph traversal.
: SIMAP must be optimized for graph traversal for paths. This means that only providing link nodes and
source and sink relationships to termination-points may not be sufficient, we may need to have the direct
relationship between the termination points or nodes.

REQ-TOPOLOGY-ABSTRACTION:
: Navigation across the abstraction levels inside a single network layer.
: In a multi-layer network we need to navigate across multiple layers. We can also define multiple abstraction
levels for a single layer and there is a need to navigate across these abstraction levels as well. Please refer
to the {{sec-topology-abstraction}} for some background on the topology abstraction.

: In a nutshell, a network (even a single layer network) can be represented
in multiple ways providing different abstraction views of the same network. In such a case, it would be beneficial
being able to navigate amongst the different levels of abstractions (e.g. to understand which set of nodes in the native
topology are actually represented as a single node in an abstract topology being built on top of the native topology).
This navigation is different and orthogonal to the multi-layer navigation where we need to report which Layer 2 path is
supporting a given Layer 3 node or link. Nevertheless, it would not be the best practice to expose it via
different topology API and model.

: SIMAP must provide a mechanism to navigate across the abstraction levels inside a single network layer.

REQ-LIVE:
: Live network topology.
: SIMAP must enable retrieval of multi-layered topology of a live network.
: Live network is the latest known view of the network

REQ-SNAPSHOT:
: Network snapshot topology.
: SIMAP must enable retrieval of multi-layered topology of different snapshots
: Snapshot is the view of the network at any given point in time

REQ-POTENTIAL:
: Potential new network topology.
: SIMAP must enable both retrieval and write access to the potential new network.
: A potential new network is the view at a given point with modifications from the snapshot.
: This view may contain either the full topology or just differences from the snapshot.

REQ-INTENDED:
: Intended network topology.
: SIMAP must enable both retrieval and write access to the intended network topology that cannot be
discovered from the real network (e.g., Intended Layer 2 topology, Intended Layer 3 topology, passive topology that
cannot be discovered).

REQ-SEMANTIC:
: Network topology semantics.
: SIMAP must provide semantics for layered network topologies and for linking external models and data.

The following requirements are more specific requirements for semantics:

REQ-LAYER-NAVIGATE:
: SIMAP must provide capability to navigate inside the topology layer and between the topology layers.

REQ-EXTENSIBLE:
: SIMAP must be extensible with metadata.

REQ-PLUGG:
: SIMAP must be pluggable. That is,

     + Must connect to other YANG modules for device configuration, inventory, configuration, assurance, etc.
The SIMAP does not contain the detailed device configuration, so a mechanism is needed to be able to link it from SIMAP.
SIMAP should also be linked to a 'logical configuration inventory'. Several examples of the type of logical information
to be linked from SIMAP: inventory of logical interfaces, inventory of ACLs, or inventory of routing policies.
     + Given that no all involved components can be available using YANG, there is a need to connect
       SIMAP YANG model with other modelling mechanisms.

REQ-BIDIR:
: SIMAP must provide a mechanism to model bidirectional links.
While data flows are unidirectional, the
bidirectional links are also common in networking.  Examples are
Ethernet cables, bidirectional SONET rings, socket connection to the
server, etc.  There is also the requirement for simplified service
layer topology, where a link is modeled as bidirectional in order to be
supported by unidirectional links at the lower layer.

REQ-MULTI-POINT:
: SIMAP must provide a mechanism to model multipoint links.
A topology model should be able to model any
topology type in a simple and explicit way, including point to
multipoint, bus, ring, star, tree, mesh, hybrid and daisy chain. A
topology model should also be able to model any link cardinality in a
simple and explicit way, including point-to-point, point-to-multipoint,
multipoint-to-multipoint or hybrid.

REQ-MULTI-DOMAIN:
: SIMAP must provide a mechanism to model links between networks.
This requirement is about covering connectivity between different networks, sub-networks, or domains.

REQ-SUBNETWORK:
: SIMAP must provide a mechanism to model network decomposition into sub-networks.
The requirement is about modelling hierarchical networks , Autonomous Systems (ASes) with multiple areas, or a network
with multiple domains (e.g., access, core, data center).
: The network can be partitioned by providing capability to have multiple child network instances as part of a
single parent network, with a relation between the parent network and child networks.

REQ-SHARED:
: SIMAP must provide a mechanism to share nodes, links, and termination points between different networks.

REQ-SUPPORTING:
: SIMAP must provide a mechanism to model supporting relationships between different types of topological entities
(e.g., a termination point is supported by the node). This may be required, e.g., if a termination point is not
supported by the underlying a termination point, but by the node (e.g., a loopback does not have physical
representation, so it is supported by physical device).

REQ-STATUS:
: Links and nodes that are down must appear in the topology. The status of the nodes and links must be either
implemented in the SIMAP or accessible from the SIMAP. Whether the status is included as part of the SIMAP or
accessible from the SIMAP is left to the solutions.

REQ-DATA-PLANE-FLOW:
: Provider data plane (Flow) needs to be correlatable to underlay and customer data plane to overlay topology
: An SRv6 example:
: In a SRv6 enabled network, sourceIPv6Address appears in a IPFIX data-template/data-record
for a captured flow on a SRv6 enabled provider interface twice. Once in relation to provider data plane in the
underlay, and once as relation to the customer data plane in the overlay.
: SIMAP must provide the semantic capability that each sourceIPv6Address can be mapped to the overlay and
underlay network topology. Both topologies might not be uniquely addressed, the VPN context
(in SRv6 these are the SID's, {{Section 3 of ?RFC8986}}) needs to be considered therefore as well.

: IPFIX protocol, defined in {{?RFC7011}}, is the protocol for the exchange of flow information from
an Exporting Process to a Collecting Process. {{Section 8 of ?RFC7011}} describes the management of
Templates and Option templates at the Exporting and Collecting Processes, and states the following:

{:quote}
> If an Information Element is required more than once in a Template,
the different occurrences of this Information Element SHOULD follow
the logical order of their treatments by the Metering Process. For
example, if a selected packet goes through two hash functions, and if
the two hash values are sent within a single Template, the first
occurrence of the hash value should belong to the first hash function
in the Metering Process. For example, when exporting the two source
IP addresses of an IPv4-in-IPv4 packet, the first sourceIPv4Address
Information Element occurrence should be the IPv4 address of the
outer header, while the second occurrence should be the address of
the inner header. Collecting Processes MUST properly handle
Templates with multiple identical Information Elements.

REQ-CONTROL-PLANE:
: Underlay control plane routing state needs to be correlatable to underlay L3 topology. Overlay control-plane
routing state needs to be correlate-able to overlay L3 network topology.

: A BMP/BGP example:
: The BMP peer distinguisher ({{Section 4.2 of ?RFC7854}}) needs to be correlateable to the VRF
of a node and the next-hop attribute of the NLRI in the BMP route-monitoring ({{Section 4.6 of ?RFC7854}}) encapsulated
message to the underlay network topology while the path attribute of the NLRI in the BMP route-monitoring
encapsulated message to the overlay topology.

## Design Requirements

The following are the design requirements for the SIMAP data model:

REQ-TOPO-ONLY:
: SIMAP should contain only topological information.
: SIMAP is not required to contain all models and data required for
all the management and use cases. However, it should be designed to support adequate pointers to other functional
data and models to ease navigating in the overall system. For example:

  + ACLs and Route Policies are not required to be supported in the SIMAP, they would be linked to the SIMAP.
  + Dynamic paths may, depending on the solution, be either inside or outside of the SIMAP. If outside of SIMAP,
    dynamic paths could be linked to the SIMAP.

: SIMAP should ensure that it is possible to represent the paths/routes and leave the choice of what level of dynamics
to represent to the specific solution/application. The model needs to be rich enough to represent any level of dynamics.
BUT from experience, we suspect it can be the same model for all level of dynamics.

REQ-PROPERTIES:
: SIMAP entities should mainly contain properties used to identify topological entities at different layers,
identify their roles, and topological relationships between them.

: SIMAP entities should also provide information required to define semantics for layered network topologies, such as:

* link directionality,
*  whether the links are multipoint or not and, if so, are whether these links are point-to-multipoint or multipoint-to-multipoint,
* role of the termination points in the link (source, destination, hub, spoke), and
* some generic mechanism to add metadata.

REQ-RELATIONSHIPS:
: SIMAP should contain all topological relationships inside each layer or between the layers (underlay/overlay)
: SIMAP should contain links to other models/data to enable generic navigation to other YANG models in
generic way.

: The SIMAP relationships should also provide information required to define semantics for layered network topologies,
such as providing:

* underlay and overlay relations between different types of topological entities,
* additional information that helps with navigation inside a layer and between the layers, for example, easy
identification of resources at the physical layer in primary versus backup paths, if the underlay
resources are used for load balancing or for backup,
* capability to model nodes, termination points, and links contained in a network, but also nodes and links shared between networks, and
* relationships between networks, either for modelling of underlay and overlay or modelling network that contains
multiple networks.


REQ-CONDITIONAL:
: Provide capability for conditional retrieval of parts of SIMAP.

REQ-TEMPO-HISTO:
: Must support geo-spatial, temporal, and historical data.  The temporal and historical can also be supported
external to the SIMAP.

## Architectural Requirements {#sec-arch}

The following are the architectural requirements for the controller that provides SIMAP API, they are the
non-functional requirements for the SIMAP API or controllers:

REQ-SCALES:
: The SIMAP API must be scalable, it must support any provider network, independent of its size.

REQ-PERFORMANCE:
: The SIMAP API must be  performant, and have acceptable response-time. Although we are not to define the response time here.

REQ-USABILITY:
: The SIMAP API must be simple and easy to integrate with the client applications, whose developers
may not be networking experts.

REQ-DISCOVERY:
: A network controller must perform the initial and on-demand discovery of a network in order to provide the layered
topology via the SIMAP API to a client/application.

REQ-SYNCH:
: The controller must perform the sync with the network in order to provide up to date layered topology
via SIMAP API to the client/application

REQ-SECURITY:
: The conventional NACM control access rules {{!RFC8341}} should apply. This includes module control access rules,
protocol operation control access rules, data node control access rules, and notification control access rules.

# Positioning SIMAP

{{?RFC8199}} advocates for a consistent classification of YANG modules and introduces two abstraction layers for
YANG modules:

+ network element YANG modules
+ network service YANG modules

The IRTF {{?RFC7426}} defines the SDN layers and architecture and proposes the following interfaces:

+ southbound interfaces between the network devices and controllers/managers
+ service interface between controllers/managers and applications

{{?RFC8309}} defines where service model might fit into the SDN Architecture, although the service model
does not require or preclude the use of SDN. It shows the following models at different layers of abstraction:

+ device model, between network elements and controllers
+ network model, between controllers and network orchestrators
+ service model, between network orchestrators and service orchestrators
+ customer service model, between service orchestrators and customer

{{?RFC8453}} describes the ACTN architecture in the context of the YANG service models. It shows how ACTN interfaces
relate to device model, network model and customer service model.

{{?RFC8969}} describes a framework for service and network management automation that takes advantage of YANG
modelling technologies. This framework is drawn from a network operator perspective irrespective of the origin of a
data model. {{?RFC8969}} introduces "network service models" and describes the layering and representation of models
within a network operator as follows:

+ device model, between device and controller
+ network model (operator oriented), between controller (that includes network orchestration function) and
service orchestrator
+ service model (customer oriented), between service orchestrator and customer, this is network service model

The SIMAP YANG module can be used at different layers of abstraction and SIMAP can provide topology at
different interfaces. Although the SIMAP module and API is primarily positioned as northbound multi-layered topology
model from (SDN) Controllers, it can also be positioned as follows:

+ In the context of {{?RFC8199}}, SIMAP can provide multi-layered topology YANG module as part of both network element
and network service YANG modules
+ In the context of {{?RFC7426}}, SIMAP can provide multi-layered topology interface as part of both Southbound and
Service Interfaces
+ In the context of {{?RFC8309}}, SIMAP can provide multi-layered topology model as part of device model, network model,
service model and customer service model
+ In the context of {{?RFC8453}}, SIMAP can provide multi-layered topology model as part of SBI (southbound interface to
network), MPI (interface between multi-domain service coordinator and network controller) and CMI (interface between
customer network controller and multi-domain service controller)
+ In the context of {{?RFC8969}}, SIMAP can provide multi-layered topology model as part of device model, network model
and network service model

# Security Considerations

As this document covers the SIMAP concepts, requirements, and use cases, there is no specific security considerations other
that those discussed in {{sec-arch}}.

{{Section 8 of !RFC8345}} discusses security aspects that will be useful when designing the SIMAP solution.

# IANA Considerations

This document has no actions for IANA.

--- back

#  Related IETF Activities

> Note: The models cited in this section are provided for illustration puroses. It is out of scope to recomend
> which models will be used as base to build the SIMAP.

##  Network Topology {#sec-ntw-topo}

   Interestingly, we could not find any network topology definition in
   IETF RFCs (not even in {{!RFC8345}}) or Internet-Drafts.  However, it is mentioned
   in multiple documents.  As an example, in Overview and Principles of
   Internet Traffic Engineering {{?RFC9522}}, which
   mentions:

   {:quote}
   > To conduct performance studies and to support planning of existing
   and future networks, a routing analysis may be performed to determine
   the paths the routing protocols will choose for various traffic
   demands, and to ascertain the utilization of network resources as
   traffic is routed through the network.  Routing analysis captures the
   selection of paths through the network, the assignment of traffic
   across multiple feasible routes, and the multiplexing of IP traffic
   over traffic trunks (if such constructs exist) and over the
   underlying network infrastructure.  A model of network topology is
   necessary to perform routing analysis.  A network topology model may
   be extracted from:
   >
   > *  Network architecture documents
   > *  Network designs
   > *  Information contained in router configuration files
   > *  Routing databases such as the link state database of an interior gateway protocol (IGP)
   > *  Routing tables
   > *  Automated tools that discover and collate network topology information.
   >
   > Topology information may also be derived from servers that monitor
   network state, and from servers that perform provisioning functions.

   Another example is {{?RFC8453}} that defines native topology, abstract topology, black topology, and grey topology,
   but all in the context of actual topology and physical topology that are not specifically defined.

## Topology Abstraction {#sec-topology-abstraction}

Please refer to the following documents for some background on topology abstractions:

* {{?RFC7926}} defines topology abstraction.
* {{Section 5 of ?RFC8453}} describes the topology abstraction methods and discusses topology abstraction factors,
types, and their context in the ACTN architecture.
* {{Section 3.13 of ?RFC8795}} defines abstract TE topologies.
* {{Section 4.1 of ?RFC8795}} defines native TE topologies.
* {{Section 4.4 of ?RFC8795}} describes how to deal with multiple abstract TE topologies provided by the same provider.
* {{Section 1.3 of ?I-D.ietf-teas-te-topo-and-tunnel-modeling}} gives some background on topology abstraction.


##  Core SIMAP Components {#sec-core}

   The following specifications are relevant to the core functions provided by the SIMAP:

   *  IETF network model and network topology model {{?RFC8345}}

   *  A YANG grouping for geographic location {{?RFC9179}}

   *  IETF modules that augment {{!RFC8345}} for different technologies:

       *  A YANG data model for Traffic Engineering (TE) Topologies {{?RFC8795}}

       *  A YANG data model for Layer 2 network topologies {{?RFC8944}}

       *  A YANG data model for OSFP topology  {{?I-D.ogondio-opsawg-ospf-topology}}

       *  A YANG data model for IS-IS topology {{?I-D.ogondio-nmop-isis-topology}}

##  Additional SIMAP Components {#sec-add}

The SIMAP may need to link to the following models, some are already augmenting {{?RFC8345}}:

*  Service Attachment Point (SAP) {{?RFC9408}}, augments 'ietf-network' data model {{?RFC8345}} by adding the SAP.

*  SAIN {{?RFC9417}} {{?RFC9418}}

*  Network Inventory Model {{?I-D.ietf-ivy-network-inventory-yang}} focuses on physical and virtual inventory.
Logical inventory is currently outside of the scope. It does not augment {{!RFC8345}}.

 * {{?I-D.ietf-ivy-network-inventory-topology}} correlates the network inventory with the general topology via RFC8345 augmentations that reference inventory.

*  KPIs: delay, jitter, loss

*  Attachment Circuits (ACs) {{?I-D.ietf-opsawg-ntw-attachment-circuit}} and {{?I-D.ietf-opsawg-teas-attachment-circuit}}

*  Configuration: The L2SM {{?RFC8466}}, L3SM {{?RFC8299}}, L2NM {{?RFC9291}}, and L3NM {{?RFC9182}}

*  Incident Management for Network Services {{?I-D.ietf-nmop-network-incident-yang}}

# Acknowledgments
{:numbered="false"}

Many thanks to Mohamed Boucadair for his valuable contributions, reviews, and comments.
Many thanks to Adrian Farrel for his SIMAP suggestion and helping to agree the terminology.
Many thanks to Dan Voyer, Brad Peters, Diego Lopez, Ignacio Dominguez Martinez-Casanueva, Italo Busi, Wu Bo,
Sherif Mostafa, Christopher Janz, Rob Evans, Danielle Ceccarelli, and many others for their contributions, suggestions
and comments.

Many thanks to Nigel Davis <ndavis@ciena.com> for the valuable discussions and his confirmation of the
modelling requirements.



