---
title: "SIMAP: Concept, Requirements, and Use Cases"
abbrev: SIMAP Concept & Needs
docname: draft-ietf-nmop-simap-concept-latest
category: info

submissiontype: IETF
number:
date:
consensus: true
v: 0
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

The document intends to be used as a reference for the assessment effort of the various topology modules to meet
SIMAP requirements.

--- middle

# Introduction

Service & Infrastructure Maps (SIMAP) is a data model that provides a view of the operator's networks and services,
including how it is connected to other models/data (e.g., inventory, observability sources, and
operational knowledge). It specifically provides an approach to model multi-layered topology
and appropriate mechanism to navigate amongst layers and correlate between them.
This includes layers from physical topology to service topology.
This model is applicable to multiple domains (access, core, data center, etc.) and
technologies (Optical, IP, etc.).

The SIMAP modelling defines the core topological entities (network, node, link, and interface) at each layer,
their role in the network topology, core topological properties, and topological relationships
both inside each layer and between the layers. It also defines how to access other external models
from a topology. The SIMAP model is a topological model that is linked to the other functional
models and connects them all: configuration, maintenance, assurance (KPIs, status, health, and symptoms),
Traffic-Engineering (TE), different behaviors and actions, simulation, emulation, mathematical abstractions,
AI algorithms, etc. These other models exist outside of the SIMAP and are not defined during SIMAP modelling.

The SIMAP data consists of virtual instances of network and service topologies at different layers.
The SIMAP provides access to this data via standard APIs for both read and write operations
(write operations for offline simulations, typically), with query capabilities and links to other YANG modules
(e.g., Service Assurance for Intent-based Networking (SAIN) {{?RFC9417}},
Service Attachment Points (SAPs) {{?RFC9408}}, Inventory {{?I-D.ietf-ivy-network-inventory-yang}}, and non-YANG models).


# Terminology

The document makes use of the following terms:

Topology:
: Topology in this document refers to the network and service topology.
  A network topology defines how physical or logical nodes, links and
  interfaces are related and arranged. A Service topology defines how
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

Topology layer:
: Represents topology at a single layer in the multi-layered topology.
: The topology layer can also represent what needs to be managed by a
  specific user or application, for example IGP layer can be of interest to the operator
  troubleshooting or optimizing the routing, while the optical layer may be
  of interest to the user managing the optical network.
: Some topology layers may relate closely to OSI layers, like L1 topology
  for physical topology, Layer 2 for link topology and Layer 3 for IPv4 and
  IPv6 topologies.
: Some topology layers represent the control aspects of Layer 3, like OSPF, IS-IS, or BGP.
: The service layer represents the service view of the connectivity, that can differ for
  different types of services and for different providers/solutions.
: The top layer represents the application/flow view of service connectivity.

Service:
: Represents network connectivity service provided over a network that enables devices, systems, or networks to
communicate and exchange data with each other. It provides the underlying infrastructure and mechanisms
necessary for establishing, maintaining, and managing connections between different endpoints.
The example services are: L2VPN, L3VPN, EVPN, VPLS, VPWS,

Subservice:
: Represents component of the service that can be independently managed but is not provided as a service.
The example subservices are: MPLS Tunnels, SRV6 Tunnels, VRFs, VPN Links, IGP Links.

Resource:
: Defined in {{?I-D.ietf-nmop-terminology}}

The document defines the following terms:

Service & Infrastructure Maps (SIMAP):
: SIMAP is a data model that provides a view of the operator's networks and services,
  including how it is connected to other models/data (e.g., inventory, observability sources, and
  operational knowledge). It specifically provides an approach to model multi-layered topology
  and appropriate mechanism to navigate amongst layers and correlate between them.
  This includes layers from physical topology to service topology.
: This model is applicable to multiple domains (access, core, data centers, etc.) and
  technologies (Optical, IP, etc.).

SIMAP modelling:
: The set of principles, guidelines, and conventions to model the SIMAP
  using the IETF modelling approach {{?RFC8345}}. They cover the
  network types (layers and sublayers), entity types, entity roles
  (network, node, termination point, or link), entity properties,
  relationship types between entities and relationships to other entities.

SIMAP model:
: Defines the core topological entities, their role in the network,
  core topological properties, and relationships both inside each layer and
  between the layers.
: It is the basic topological model with references/pointers to other models and connects them all:
  configuration, maintenance, assurance (KPIs, status, health, symptoms, etc.), traffic engineering,
  different behaviors, simulation, emulation, mathematical abstractions, AI algorithms, etc.

SIMAP data:
: Consists of instances of network and service topologies at
   different layers.  This includes instances of networks, nodes,
   links and termination points, topological relationships between
   nodes, links and termination points inside a network,
   relationships between instances belonging to different networks,
   links to functional data for the instances, including
   configuration, health, symptoms.
: The SIMAP data can be historical, real-time, or future data for 'what-if' scenarios.

# Sample SIMAP Use Cases

The following are sample use cases that require SIMAP:

* Generic inventory queries
+ Service placement feasibility checks
+ Service-> subservice -> resource
+ Resource -> subservice -> service
+ Intent/service assurance
+ Service E2E and per-link KPIs on SIMAP (connectivity status, high-availability, delay, jitter, and loss)
+ Capacity planning
+ Network design
+ Simulation
+ Closed-loop
- Network Digital Twin (NDT)

Overall, the SIMAP is needed to provide the mechanism to connect data islands from the core multi-layered topology.
It is a solution feasible and useful in the short-term for the existing operations use cases, but it is also a
requirement for the SIMAP.

> The following subsections include some initial use case descriptions to initiate the discussion about what type of info
> is needed to describe the use cases in the context of SIMAP.
> The next version of the draft will include more info on these use cases and more input from the operators,
> from the perspective of what the value of the SIMAP for each use case is and how the SIMAP API can be used.
> This will also clarify if only read and if/when write interface is needed per use case.

## Inventory Queries

Network inventory refers to a comprehensive record or database that tracks and documents all the network components and devices within an organization's IT infrastructure.

Key elements typically found in a network inventory include:
* Hardware Details: Descriptions of physical devices such as routers (including its internal components such as cards, power supply units, pluggables), switches, servers, network cables, including model numbers, serial numbers, and manufacturer information. These information will facilitate locating additional details of the hadware in the manufacturer systems and the correlation with the purchase catalog of the company.
* Software and Firmware: Versions of operating systems, network management tools, and firmware running on network devices. Note that a network device can have components with their own software and firmware.
* Licensing Information: For any licensed software or devices, the network inventory will track license numbers, expiry dates, and compliance.

Network inventory lifecycle refers to the stages a network device or component goes through from its introduction to the network until its removal or replacement. It encompasses everything from acquisition and deployment to maintenance, upgrade, and eventually decommissioning. Managing the network inventory lifecycle efficiently is crucial for maintaining a secure, functional, and cost-effective network.

A well-maintained network inventory helps organizations with network management, troubleshooting, asset tracking, security, and ensuring compliance with regulations. It also helps in scaling the network, planning upgrades, and responding to issues quickly.  In order to facilitate the planning and troubleshooting processes it is necessary to be able to navigate from network inventory to network topology and services.

The application will be able to retrieve physical topology from the controller via the SIMAP API and from the
response it will be able to retrieve physical inventory of individual devices and cables.

The application may request either one or multiple topology layers via the SIMAP API and from the response
it will be able to retrieve both physical and logical inventory.

For Access network providers the ability to have linkage in the SIMAP of the complete network (active + passive) is
essential as it provides many advantages for optimized customer service, reduced Mean Time To Repair (MTTR), and lower operational costs
through truck roll reduction. For example, operators may use custom-tags that are readily available for a customer-facing device and then query
the inventory based on that tag to correlate it with the inventory and then map it to the network/service topology. The mapping and correlation can be then used for triggering apprpriate service checks.

## Service Placement Feasibility Checks

## Service-> Subservice -> Resource

The application will be able to retrieve all services from the SIMAP API for selected network types.
The application will be able to retrieve the topology for selected services via SIMAP API and from the response
it will be able to navigate via the supporting relationship top-down to the lower layers. That way, it will be able to
determine what logical resources are used by the service. The supporting relations to the lowest layer will help
application to determine what physical resources are used by the service.

## Resource -> Subservice -> Service

The application will be able to navigate from the Physical, L2 or L3 topology to the services that use specific
resources. For example, the application will be able to select the resource and by navigating the supporting relationship
bottom-up come to the service and its nodes, tps and links.

## Intent/Service Assurance

Network intent and service assurance work together to ensure that the network aligns with business goals and that the services provided meet the agreed-upon service level agreements (SLAs).

The Service Assurance for Intent-Based Networking Architecture (SAIN) {{?RFC9417}} approach emphasizes a comprehensive view of all components involved in service delivery, including network devices and functions, to effectively monitor and maintain service health.

The key objectives of this architecture:
* Holistic Service Monitoring: By considering all elements involved in service delivery, the architecture enables a thorough assessment of service health.
* Correlation of Service Degradation: It assists in linking service performance issues to specific network components, facilitating precise identification of faults.
* Impact Assessment: The architecture identifies which services are affected by the failure or degradation of particular network components, aiding in prioritizing remediation efforts.

When a service is degraded, the SAIN architecture will highlight where in the assurance service graph to look, as opposed to going hop by hop to troubleshoot the issue.
More precisely, the SAIN architecture will associate to each service instance a list of symptoms originating from specific subservices, corresponding to components of the network.
These components are good candidates for explaining the source of a service degradation.

The application will be able to retrieve topology layer and any network/node/termination point/link instances from the controller via the SIMAP API and from the response it will be able to determine the health of each instance by navigating to the SAIN subservices and its symptoms.

## Service E2E and Per-link KPIs

The application will be able to retrieve the topology at any layer from the controller via the SIMAP API and from the
response it will be able to navigate any retrieve any KPIs for selected topology entity.

## Capacity Planning

Network Capacity Planning refers to the process of analyzing, predicting, and ensuring that the network has sufficient
capacity (e.g., {{?RFC5136}}), resources, and infrastructure to meet current and future demands. It involves evaluating the network's ability
to handle increasing (including forecast) amounts of data, traffic, and users activity, while maintaining acceptable levels of performance,
reliability, and security.

The capacity planning primary goal is to ensure that a network can support business operations, applications, and
services without interruptions, delays, or degradation in quality. This requires a thorough understanding of the
network's current state, as well as future requirements and growth projections.

Key aspects of network capacity planning include:

* Traffic analysis: Monitoring and analyzing network traffic patterns to identify trends, peak usage periods, and areas
of congestion. For example, by generating a core traffic matrix with IPFIX flow record {{?RFC7011}} or deducting an approximate traffic matrix from the link utilization data.
* Resource utilization: Evaluating the link utilization throughout the network for the current demand identifying bottlenecks and potential QoS peformance issues.
* Growth forecasting: Predicting future network growth based on business expansion, new applications, or changes in users
behavior.
* What-if scenarios: Creating models to assess the network behavior under different scenarios, such as increased traffic,
failure conditions (link, router or Shared Risk Resource Group), and new application deployments (such as a new Content Delivery Network source, a new peering point, a new data center...).
* Upgrade planning: Identifying areas where upgrades or additions are needed to ensure that the network can minimize the
 effect of node/link failures, mitigate QoS problems, or simply to support growing demands.
* Cost-benefit analysis: Evaluating the costs and benefits of upgrading or adding new resources to determine the most
cost-effective solutions.

By implementing a robust capacity planning process, organizations can:

* Ensure better network reliability: Minimize downtime and ensure that the network is always available when needed.
* Improve performance: Optimize network resources to support business-critical applications and services.
* Optimize costs: Avoid unnecessary over-provisioning by making informed decisions based on data-driven insights.
* Support business growth: Scale the network to meet increasing demands and support business expansion.

The application will be able to retrieve topology layer and any network/node/termination point/link instances from
the controller via the SIMAP API and from the response it will be able to map the traffic analysis to the entities
(typically links and router), evaluate their current utilization, based on the grow forecasting evaluate which elements
to add to the network, and finally perform the 'what-if' failure analysis by simulating the removal of link(s) and/or
router(s) while evaluating the network performance.

## Network design

## Network Simulation and Network Emulation

Network simulation is a process used to analyse the behaviour of networks via software. It allows network engineers and researchers to study how the network protocols work under different conditions, such as diffenet topologies, traffic loads, network failures, or the introduction of new devices. Network emulation, on the other hand, replicates the behavior of a real-world network, allowing for more realistic analysis compared to network simulation. While network simulation focuses on modeling and approximating network behavior, network emulation involves creating a real-time, functional network environment whose protocol behaves exactly like a real network. Ideally, network emulation uses the same software images as in the real network, but it could also be peformed (with less accuracy) using generic software.

### Types of Network simulation
There are several types of network simulations, each designed to address specific needs and use cases. Below are the main categories of network simulation:
1. Discrete Event Simulation (DES):
This is the most common type of network simulation. It models a series of events that occur at specific points in time. Each event triggers a change in the state of a network component (e.g. a link is down, a card fails, a packet arrives…).
 2. Continuous Simulation:
In contrast to discrete event simulation, continuous simulation models systems where variables change continuously over time. Network parameters like bandwidth, congestion, and throughput can be treated as continuous functions.
The main use case is to model certain aspects of network performance that evolve continuously, such as link speeds or delay distributions in links that are impacted by envirnnmental conditions (such as microwave or satellite links).
3. Monte Carlo Simulation:
This type of simulation uses statistical methods to model and analyze networks under uncertain or variable conditions. Monte Carlo simulations generate a large number of random samples to predict the performance of a network across multiple scenarios. It is used for probabilistic analysis, risk assessment, and performance evaluation under uncertain conditions.
### Goals of Network simulation
The simulations can be also classified depending on the goal of the simulation
####  Network Protocol Analysis
This type of simulation focuses on simulating specific networking protocols (IS-IS, OSPF, BGP, SR) to understand how they perform under different conditions. It models the protocol operations and interactions among devices in the network. For example, simulation can be used to asses the impact of changing a link metric. Morever, specific features of the networking protocol can be tested. For example, how fast-reroute performs in a given network topology.

#### Traffic Simulation
This simulation focuses on modelling traffic flow across the network, including packet generation, flow control, routing, and congestion. It aims to evaluate traffic's impact on network performance.

The main use is to model the impact of different types of traffic (e.g., voice, video, mobile data, web browsing) and understand how they affect the network's bandwidth and congestion levels. It can be used to identify bottelnecks and assist the capacity planning process.

#### Simulation of different topologies under normal and failure scenarios
This type of simulation focuses on the structure and layout of the network itself. It simulates different network topologies, such as mesh, horse-shoe, bus, star, or tree topologies, and their impact on the network's performance.  It can be used, together with the traffic simualtion to evaluate the most efficient topology for a network, under normal conditions and considering factors like fault tolerance.

## Closed Loop

## Digital Twin

# SIMAP Requirements

## Core Requirements

The following are the core requirements for the SIMAP (note that some of them are supported by
default by {{!RFC8345}}):

REQ-BASIC-MODEL-SUPPORT:
: Basic model with network, node, link, and interface entity types.
: This means that users of the SIMAP model
must be able to understand topology model at any layer via these core concepts only,
without having to go to the details of the specific augmentations to understand the topology.

REQ-LAYERED-MODEL:
: Layered SIMAP, from physical network (ideally optical, layer 2, layer 3) up to  service and intent views.

REQ-PASSIVE-TOPO:
: SIMAP must support topology of the complete network, including active and passive parts.
: For Access network providers the ability to have linkage in the SIMAP of the complete network (active + passive) is
essential as it provides many advantages for optimized customer service, reduced MTTR, and lower operational costs
through truck roll reduction.

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
: Standard based SIMAP Models and APIs, for multi-vendor support.
:  SIMAP must provide the standard YANG APIs
that provide for read/write and queries.  These APIs must also provide the capability to retrieve the
links to external data/models.

REQ-COMMON-APP:
: SIMAP models and APIs must be common over different network domains (campus, core, data center, etc.).
: This means that clients of the SIMAP API must be able to understand the topology model of layers of any
domain without having to understand the details of any technologies and domains.

REQ-SEMANTIC:
: SIMAP must provide semantics for layered network topologies and for linking external models/data.

REQ-LAYER-NAVIGATE:
: SIMAP must provide intra-layer and inter-layer relationships.

REQ-EXTENSIBLE:
: SIMAP must be extensible with metadata.

REQ-PLUGG:
: SIMAP must be pluggable. That is,

     + Must connect to other YANG modules for inventory, configuration, assurance, etc.
     + Given that no all involved components can be available using YANG, there is a need to connect
       SIMAP YANG model with other modelling mechanisms.

REQ-GRAPH-TRAVERSAL:
: SIMAP must be optimized for graph traversal for paths. This means that only providing link nodes and
source and sink relationships to termination-points may not be sufficient, we may need to have the direct
relationship between the termination points or nodes.

REQ-BIDIR:
: SIMAP must provide a mechanism to model bidirectional links
One of the core characteristics of any network topology is the link
directionality.  While data flows are unidirectional, the
bidirectional links are also common in networking.  Examples are
Ethernet cables, bidirectional SONET rings, socket connection to the
server, etc.  We also encounter requirements for simplified service
layer topology, where we want to model link as bidirectional to be
supported by unidirectional links at the lower layer.

REQ-MULTI-POINT:
: SIMAP must provide a mechanism to model multipoint links
One of the core characteristics of any network topology is its type
and link cardinality.  Any topology model should be able to model any
topology type in a simple and explicit way, including point to
multipoint, bus, ring, star, tree, mesh, hybrid and daisy chain.  Any
topology model should also be able to model any link cardinality in a
simple and explicit way, including point to point, point to
multipoint, multipoint to multipoint or hybrid.

REQ-MULTI-DOMAIN:
: SIMAP must provide a mechanism to model links between the network/domains
One of the core characteristics of any topology is connectivity between different network, subnetworks or domains.

REQ-SUBNETWORK:
: SIMAP must provide a mechanism to model network decomposition into sub-networks.
This would allow modelling hierarchical network domains, Autonomous System with multiple Areas or e2e network
with multiple domains.

REQ-SHARED:
: SIMAP must provide a mechanism to share nodes, links and termination points between different networks.

REQ-SUPPORTING:
: SIMAP must provide a mechanism to model supporting relationships between different types of topological entities
(e.g., tp is supported by the node). This may be required in the cases when tp is not supported by the underlying tp,
but by the node (e.g., loopback does not have physical representation, so it is supported by physical device).

REQ-STATUS:
: Links and nodes that are down must appear in the topology. The status of the nodes and links must be either
implemented in the SIMAP model or accessible from the SIMAP model.

## Design Requirements

The following are design requirements for modelling the SIMAP. They are derived from the core requirements
collected from the operators and although there is some duplication, these are focused on summarizing the requirements
for the design of the model and API:

REQ-TOPO-ONLY:
: SIMAP should contain only topological information.
: SIMAP is not required to contain all models and data required for
all the management and use cases. However, it should be designed to support adequate pointers to other functional
data and models to ease navigating in the overall system. For example:

  + ACLs and Route Policies are not required to be supported in the SIMAP, they would be linked to the SIMAP
  + Dynamic paths may either be outside of the SIMAP or part of traffic engineering data/models

REQ-PROPERTIES:
: SIMAP entities should mainly contain properties used to identify topological entities at different layers,
identify their roles, and topological relationships between them.

REQ-RELATIONSHIPS:
: SIMAP should contain all topological relationships inside each layer or between the layers (underlay/overlay)
: SIMAP should contain links to other models/data to enable generic navigation to other YANG models in
generic way.

REQ-CONDITIONAL:
: Provide capability for conditional retrieval of parts of SIMAP.

REQ-TEMPO-HISTO:
: Must support geo-spatial, temporal, and historical data.  The temporal and historical can also be supported
external to the SIMAP.

## Architectural Requirements

The following are the architectural requirements for the controller that provides SIMAP API:

REQ-DM-SCALES:
: Scale, performance, ease of integration.

REQ-DM-DISCOVERY:
: Initial discovery and dynamic (change only) synch with the physical network.

# Security Considerations

As this document covers the SIMAP concepts, requirements, and use cases, there is no specific security considerations.
However, the RFC 8345 Security Considerations aspects will be useful when designing the solution.

# IANA Considerations

This document has no actions for IANA.

--- back

#  Related IETF Activities

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

##  Core SIMAP Components {#sec-core}

   The following specifications are core for the SIMAP:

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
Logical inventory is currently outside of the scope.
It does not augment RFC8345 like the two Internet-Drafts that it evolved from {{?I-D.ietf-ccamp-network-inventory-yang}}
and {{?I-D.wzwb-opsawg-network-inventory-management}}. {{?I-D.ietf-ivy-network-inventory-topology}}
correlates the network inventory with the general topology via RFC8345 augmentations that reference inventory.

*  KPIs: delay, jitter, loss

*  Attachment Circuits (ACs) {{?I-D.ietf-opsawg-ntw-attachment-circuit}} and {{?I-D.ietf-opsawg-teas-attachment-circuit}}

*  Configuration: L2SM {{?RFC8466}}, L3SM {{?RFC8299}}, L2NM {{?RFC9291}}, and L3NM {{?RFC9182}}

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



