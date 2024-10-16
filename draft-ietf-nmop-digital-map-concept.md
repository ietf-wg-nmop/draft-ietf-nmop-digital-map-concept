---
title: "Digital Map: Concept, Requirements, and Use Cases"
abbrev: Digital Map Concept & Needs
docname: draft-ietf-nmop-digital-map-concept-latest
category: info

submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Network Management Operations"
keyword:
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

This document defines the concept of Digital Map, and identifies a set of Digital Map requirements and use cases.

The document intends to be used as a reference for the assessment effort of the various topology modules to meet
Digital Map requirements.

--- middle

# Introduction

Digital Map is a data model that provides a view of the operator's networks and services,
including how it is connected to other models/data (e.g. inventory, observability sources, and
operational knowledge). It specifically provides an approach to model multi-layered topology
and appropriate mechanism to navigate amongs layers and correlate between them.
This includes layers from physical topology to service topology.
This model is applicable to multiple domains (access, core, data centers, etc.) and
technologies (Optical, IP, etc.).

The Digital Map modelling defines the core topological entities (network, node, link, and interface) at each layer,
their role in the network topology, core topological properties, and topological relationships both inside each
layer and between the layers. It also defines how to access other external models from the topology.

The Digital Map model is a topological model that is linked to the other functional models and
connects them all: configuration, maintenance, assurance (KPIs, status, health, and symptoms), Traffic-Engineering (TE),
different behaviors and actions, simulation, emulation, mathematical abstractions, AI algorithms, etc.
These other models exist outside of the digital map and are not defined during digital map modelling.

The Digital Map data consists of virtual instances of network and service topologies at different layers.
The Digital Map provides access to this data via standard APIs for both read and write operations
(write operations for offline simulations), with query capabilities and links to other YANG modules
(e.g., Service Assurance for Intent-based Networking (SAIN) {{?RFC9417}}, Service Attachement Points (SAPs) {{?RFC9408}},
Inventory {{?I-D.ietf-ivy-network-inventory-yang}}, and non-YANG models.


# Terminology

The document makes use of the following terms:

Topology:
: Topology in this document refers to the network and service topology.
  Network topology defines how physical or logical nodes, links and
  interfaces are related and arranged.  Service topology defines how
  service components (e.g., VPN instances, customer interfaces, and
  customer links) between customer sites are interrelated and
  arranged.  There are at least 8 types of topologies: point to
  point, bus, ring, star, tree, mesh, hybrid and daisy chain.
  Topologies may be unidirectional or bidirectional (bus, some
  rings).

Multi-layered topology:
: A multi-layered topology models relationships between different layers of topology,
  where each layer represents a connectivity aspect of the network
  and services that needs to be configured, controlled and monitored.
  Each layer of topology has a separate lifecycle.

Topology layer:
: Represents topology at a single layer in the multi-layered topology.
: The topology layer can also represent what needs to be managed by a
  specific user, for example IGP layer can be of interest to the operator
  troubleshooting or optimizating the routing, while the optical layer may be
  of interest to the user managing the optical network.
: Some topology layers may relate closely to OSI layers, like L1 topology
  for physical topology, Layer 2 for link topology and Layer 3 for IPv4 and
  IPv6 topologies.
: Some topology layers represent the control aspects of Layer 3, like OSPF, IS-IS, or BGP.
: The service layer represents the service view of the connectivity, that can differ for
  different types of services and for different providers/solutions.
: The top layer represents the application/flow view of service connectivity.

The document defines the following terms:

Digital Map:
: Digital Map is a data model that provides a view of the operator's networks and services,
  including how it is connected to other models/data (e.g. inventory, observability sources, and
  operational knowledge). It specifically provides an approach to model multi-layered topology
  and appropriate mechanism to navigate amongs layers and correlate between them.
  This model is applicable to multiple domains (access, core, data centers, etc.) and
  technologies (Optical, IP, etc.).

Digital Map modelling:
: The set of principles, guidelines, and conventions to model the
  Digital Map using the IETF {{?RFC8345}} approach.  They cover the
  network types (layers and sublayers), entity types, entity roles
  (network, node, termination point or link), entity properties,
  relationship types between entities and relationships to other entities.

Digital Map model:
: Defines the core topological entities, their role in the network,
  core topological properties and relationships both inside each layer and
  between the layers.
: It is the basic topological model with the links to other models and connects them all:
  configuration, maintenance, assurance (KPIs, status, health, symptoms, etc.), traffic engineering,
  different behaviors, simulation, emulation, mathematical abstractions, AI algorithms, etc.

Digital Map data:
: Consists of instances of network and service topologies at
   different layers.  This includes instances of networks, nodes,
   links and termination points, topological relationships between
   nodes, links and termination points inside a network,
   relationships between instances belonging to different networks,
   links to functional data for the instances, including
   configuration, health, symptoms.
: The data can be historical, real-time, or future data for 'what-if' scenarios.

# Sample Digital Map Use Cases

The following are sample use cases that require Digital Map:

* Generic inventory queries
+ Service placement feasibility checks
+ Service-> subservice -> resource
+ Resource -> subservice -> service
+ Intent/service assurance
+ Service E2E and per-link KPIs on the Digital Map (connectivity status, high-availability, delay, jitter, and loss)
+ Capacity planning
+ Network design
+ Simulation
+ Closed loop
- Digital Twin

Overall, the Digital Map is needed to provide the mechanism to connect data islands from the core multi-layered topology.
It is a solution feasible and useful in the short-term for the existing operations use cases, but it is also a
requirement for the Digital Twin.

The following sections shows some example use case descriptions to initiate the discussion what type of info is needed
to describe the use cases in the context of Digital Map.
The next version of the draft will include more info on these use cases, from the perspective of what is the value of
the digital map for each use case and how the Digital Map API can be used.
This will also clarify if only read and if/when write interface is needed per use case.

## Generic inventory queries
The application will be able to retrieve physical topology from the controller via Digital Map API and from the
response it will be able to retrieve physical inventory of individual devices and cables.

The application may request either one or multiple layers of topology via the Digital Map API and and from the response
it will be able to retrieve both physical and logical inventory.

## Service placement feasibility checks

## Service-> subservice -> resource
The application will be able to retrieve all services from the Digital Map API for selected network type.
The application will be able to retrieve the topology for selected services via Digital Map API and from the response
it will be able to navigate via the supporting relationship top-down to the lower layers. That way, it will be able to
determine what logical resources are used by the service. The supporting relations to the lowest layer will help
application to determine what physical resources are used by the service.

## Resource -> subservice -> service
The application will be able to navigate from the Physical, L2 or L3 topology to the services that use specific
resources. For example, the application will be able to select the resouce and by navigation the supporting relationship
bottom-up come to the service and its nodes, tps and links.

## Intent/service assurance
The application will be able to retrieve topology layer and any network/node/tp/link instances from the controller
via Digital Map API and from the response it will be able to determine the health of each instance by navigating to the
SAIN subservices and its symptoms.

## Service E2E and per-link KPIs
The application will be able to retieve the topology at any layer from the controller via Digital Map API and from the
response it will be able to navigate any retrieve any KPIs for selected topology entity.

## Capacity planning

## Network design

## Simulation

## Closed Loop

## Digital Twin

# Digital Map Requirements

## Core Requirements

The following are the core requirements for the Digital Map (note that some of them are supported by
default by {{!RFC8345}}):

REQ-BASIC-MODEL-SUPPORT:
: Basic model with network, node, link, and interface entity types.
: This means that users of the Digital Map model
must be able to understand topology model at any layer via these core concepts only,
without having to go to the details of the specific augmentations to understand the topology.

REQ-LAYERED-MODEL:
: Layered Digital Map, from physical network (ideally optical, layer 2, layer 3) up to  service and intent views.

REQ-PROG-OPEN-MODEL:
: Open and programmable Digital Map.
: This includes "read" operations to retrieve the view of the network, typically as application-facing interface of
Software Defined Networking (SDN) controllers or orchestrators.
: It also includes "write" operations, not for the ability to directly change the Digital Map data
(e.g., changing the network or service parameters), but for offline simulations, also known as what-if scenarios.
:  Running a "what-if" analysis requires the ability to take
snapshots and to switch easily between them.
: Note that there is a need to distinguish between a change on the Digital Map
for future simulation and a change that reflects the current reality of the network.

REQ-STD-API-BASED:
: Standard based Digital Map Models and APIs, for multi-vendor support.
:  Digital Map must provide the standard YANG APIs
that provide for read/write and queries.  These APIs must also provide the capability to retrieve the
links to external data/models.

REQ-COMMON-APP:
: Digital Map models and APIs must be common over different network domains (campus, core, data center, etc.).
: This means that clients of the Digital Map API must be able to understand the topology model of layers of any
domain without having to understand the details of any technologies and domains.

REQ-SEMANTIC:
: Digital Map must provide semantics for layered network topologies and for linking external models/data.

REQ-LAYER-NAVIGATE:
: Digital Map must provide intra-layer and inter-layer relationships.

REQ-EXTENSIBLE:
: Digital Map must be extensible with metadata.

REQ-PLUGG:
: Digital Map must be pluggable. That is,

     + Must connect to other YANG modules for inventory, configuration, assurance, etc.
     + Given that no all involved components can be available using YANG, there is a need to connect
       Digital Map YANG model with other modelling mechanisms.

REQ-GRAPH-TRAVERSAL:
: Digital Map must be optimized for graph traversal for paths. This means that only providing link nodes and
source and sink relationships to termination-points may not be sufficient, we may need to have the direct
relationship between the termination points or nodes.

## Design Requirements

The following are design requirements for modelling the Digital Map. Theey are derived from the core requerements
collected from the operators and although there is some duplication, these are focused on summarizing the requirements
for the design of the model and API:

REQ-TOPO-ONLY:
: Digital Map should contain only topological information.
:  Digital Map is not required to contain all models and data required for
all the management and use cases. However, it should be designed to support adequate pointers to other functional
data and models to ease navigating in the overall system. For example:

  + ACLs and Route Policies are not required to be supported in the Digital Map, they would be linked to Digital Map
  + Dynamic paths may either be outside of the Digital Map or part of traffic engineering data/models

REQ-PROPERTIES:
: Digital Map entities should mainly contain properties used to identify topological entities at different layers,
identify their roles, and topological relationships between them.

REQ-RELATIONSHIPS:
: Digital Map should contain all topological relationships inside each layer or between the layers (underlay/overlay)
: Digital Map should contain links to other models/data to enable generic navigation to other YANG models in
generic way.

REQ-CONDITIONAL:
: Provide capability for conditional retrieval of parts of Digital Map.

REQ-TEMPO-HISTO:
: Must support geo-spatial, temporal, and historical data.  The temporal and historical can also be supported
external to the Digital Map.

## Architectural Requirements

The following are the architectural requirements for the controller that provides Digital Map API:

REQ-DM-SCALES:
: Scale, performance, ease of integration.

REQ-DM-DISCOVERY:
: Initial discovery and dynamic (change only) synch with the physical network.

# Security Considerations

As this document covers the Digital Map concepts, requirements, and use cases, there is no specific security considerations.
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

##  Core Digital Map Components {#sec-core}

   The following specifications are core for the Digital Map:

   *  IETF network model and network topology model {{?RFC8345}}

   *  A YANG grouping for geographic location {{?RFC9179}}

   *  IETF modules that augment {{!RFC8345}} for different technologies:

       *  A YANG data model for Traffic Engineering (TE) Topologies {{?RFC8795}}

       *  A YANG data model for Layer 2 network topologies {{?RFC8944}}

       *  A YANG data model for OSFP topology  {{?I-D.ogondio-opsawg-ospf-topology}}

       *  A YANG data model for IS-IS topology {{?I-D.ogondio-nmop-isis-topology}}

##  Additional Digital Map Components {#sec-add}

The Digital Map may need to link to the following models, some are already augmenting {{?RFC8345}}:

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

Many thanks to Mohamed Boucadair (<mohamed.boucadair@orange.com>) for his valuable contributions, reviews, and comments.

Many thanks to Nigel Davis <ndavis@ciena.com> for the valuable discussions and his confirmation of the modelling requirements.
