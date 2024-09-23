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
 - Digital twim
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

This document defines the concept of Digital Map, explains its connection to the Digital Twin, and identifies a set of Digital Map requirements and use cases.

The document intends to be used as a reference for the assessment effort of the various topology modules to meet Digital Map requirements.

--- middle

# Introduction

## Digital Twin Overview

The network digital twin (referred to simply as Digital Twin) concepts and a reference architecture are discussed in
the "Digital Twin Network: Concepts and Reference Architecture" {{?I-D.irtf-nmrg-network-digital-twin-arch}}.
That reference document defines the core elements of Digital Twin: Data, Models, Interfaces, and Mapping.

The Digital Twin, constructed based on the four core technology elements, is intended to analyze, diagnose, emulate,
and control the physical network in its whole lifecycle with the help of optimization algorithms, management methods,
and expert knowledge.

Also, that document {{?I-D.irtf-nmrg-network-digital-twin-arch}} states that a Digital Twin can be seen as an indispensable part of the overall network system
and provides a general architecture involving the whole lifecycle of physical networks in the future, serving the
application of innovative network technologies (e.g., network planning, construction, maintenance and optimization,
improving the automation and intelligence level of the network).

## Digital Map

Digital Map provides the core multi-layer topology model and data for the Digital Twin and connects them to the
other Digital Twin models and data. This includes layers from physical topology to service topology.

The Digital Map modelling defines the core topological entities (network, node, link, and interface) at each layer, their role in
the network, core properties, and relationships both inside each layer and between the layers.

The Digital Map model can be approached as a topological model that is linked to other functional parts of the Digital Twin and
connects them all: configuration, maintenance, assurance (KPIs, status, health, and symptoms), Traffic-Engineering (TE),
different behaviors and actions, simulation, emulation, mathematical abstractions, AI algorithms, etc.

The Digital Map data consists of virtual instances of network and service topologies at different layers.
The Digital Map provides the access to this data via standard APIs for both read and write operations
(write operations for offline simulations), with query capabilities and links to other YANG modules
(e.g., Service Assurance for Intent-based Networking (SAIN) {{?RFC9417}}, Service Attachement Points (SAPs) {{?RFC9408}},
Inventory {{?I-D.ietf-ivy-network-inventory-yang}}, and non-YANG models.

## Digital Map as a Prerequisite for Digital Twin

One of the important requirements for Digital Twin is to ease correlating all models and
data to topological entities at different layers in the layered twin network.  The Digital Map aims at providing a
virtual instance of the topological information of the network, based on this Digital Map Model.
**Building a Digital Map is prerequisite towards the Digital Twin.**

The Digital Map model/data provide this missing correlation between the topology models/data and all other
models/data: KPIs, alarms, incidents, inventory (with UUIDs), configuration, traffic engineering, planning,
simulation ("what-if"), emulations, actions, and behaviors.

Some of these models/data provide a device view, some provide a network or subnetwork view, while others focus
more on the customer service perspective.  All these views are needed for both inner and outer closed-loops.

It is debatable what is part of the Digital Map itself versus what are pointers from the Digital Map to some
other sources of information.  As an example, a Digital Map should not specifically include all information
about the device inventory (product name, vendor, product series, embedded software, and
hardware/software versions, as specified in a Network Inventory. A pointer to another inventory system
might be sufficient or support of means that ease the mapping between inventory and topology.

Similarly, Digital Map should not specifically contain incidents, configuration,
data plane monitoring, or even assurance information, simply to name a few, but should provide pointers to the different data sources.

## Scope

This document defines the concept of Digital Map, explains its connection to the Digital Twin, and identifies a set of Digital Map requirements and use cases.

The document intends to be used as a reference for the assessment effort of the various topology modules to meet Digital Map requirements.

# Terminology

The document defines the following terms:

Digital Twin:
: Virtual instance of a physical system (twin) that is continually
  updated with the latter's performance, maintenance and health
 status data throughout the physical system's life cycle (as
 defined in  {{Section 2.2 of ?I-D.irtf-nmrg-network-digital-twin-arch}}

Topology:
: Network topology defines how physical or logical nodes, links and
  interfaces are related and arranges.  Service topology defines how
  service components (e.g., VPN instances, customer interfaces, and
  customer links) between customer sites are interrelated and
  arranged.  There are at least 8 types of topologies: point to
  point, bus, ring, star, tree, mesh, hybrid and daisy chain.
  Topologies may be unidirectional or bidirectional (bus, some
  rings).

Topology layer:
: Defines a layer in the multilayer topology.  A multilayer topology
  models relationships between different layers of connectivity,
  where each layer represents a connectivity aspect of the network
  and service that needs to be configured, controlled and monitored.
: The topology layer can also represent what needs to be managed by a
  specific user, for example IGP layer can be of interest to the operator
  troubleshooting or optimizating the routing, while the optical layer may be
  of interest to the user managing the optical network.
:  Some topology layers may relate closely to OSI layers, like L1 topology
   for physical topology, Layer 2 for link topology and Layer 3 for IPv4 and
   IPv6 topologies.
: Some topology layers represent the control
   aspects of Layer 3, like OSPF, IS-IS, or BGP.
:  The service layer represents
   the service view of the connectivity, that can differ for
   different types of services and for different providers/solutions.
:  The top layer represents the application/flow view of connectivity.

Digital Map:
: Basis for the Digital Twin that provides a virtual instance of the
    topological information of the network.  It provides the core
    multi-layer topology model and data for the Digital Twin and
    connects them to the other Digital Twin models and data.

Digital Map modelling:
: The set of principles, guidelines, and conventions to model the
      Digital Map using the IETF {{?RFC8345}} approach.  They cover the
      network types (layers and sublayers), entity types, entity roles
      (network, node, termination point or link), entity properties and
      relationship types between entities.

Digital Map model:
: Defines the core topological entities, their role in the network,
  core properties and relationships both inside each layer and
  between the layers.
:  It is the basic topological model that is
   linked to other functional parts of the Digital Twin and connects
   them all: configuration, maintenance, assurance (KPIs, status,
   health, symptoms, etc.), traffic engineering, different behaviors,
   simulation, emulation, mathematical abstractions, AI algorithms,
   etc.

Digital Map data:
: Consists of instances of network and service topologies at
   different layers.  This includes instances of networks, nodes,
   links and termination points, topological relationships between
   nodes, links and termination points inside a network,
   relationships between instances belonging to different networks,
   links to functional data for the instances, including
   configuration, health, symptoms.
 :The data can be historical, real-time, or future data for 'what-if' scenarios.

# Sample Digital Map Use Cases

The following are sample Digital Twin use cases that require Digital Map:

* Generic inventory queries
+ Service placement feasibility checks
+ Service-> subservice -> resource
+ Resource -> subservice -> service
+ Intent/service assurance
+ Service E2E and per-link KPIs on the Digital Map (delay, jitter, and loss)
+ Capacity planning
+ Network design
+ Simulation
- Closed loop

Overall, the Digital Map is needed to break down data islands and have a Digital Twin for emulation and closed loop,
which is a catalyst for autonomous networking.


# Digital Map Requirements

## Core Requirements

The following are the core requirements for the Digital Map (note that some of them are supported by default by {{!RFC8345}}):

REQ-BASIC-MODEL-SUPPORT:
: Basic model with network, node, link, and interface entity types. This means that users of the Digital Map model must be able to understand topology model at any layer via these core concepts only, without having to go to the details of the specific augmentations to understand the topology.

REQ-LAYERED-MODEL:
: Layered Digital Map, from physical network (ideally optical, layer 2, layer 3) up to  service and intent views.

REQ-PROG-OPEN-MODEL:
: Open and programmable Digital Map.
: This includes "read" operations to retrieve the view of the network, typically as application-facing interface of Software Defined Networking (SDN) controllers or orchestrators.
: It also includes "write" operations, not for the ability to directly change the Digital Map data (e.g., changing the network or service parameters),
but for offline simulations, also known as what-if scenarios.
:  Running a "what-if" analysis requires the ability to take
snapshots and to switch easily between them.
: Note that there is a need to distinguish between a change on the Digital Map
for future simulation and a change that reflects the current reality of the network.

REQ-STD-API-BASED:
: Standard based Digital Map Models and APIs, for multi-vendor support.
:  Digital Map must provide the standard YANG APIs
that provide for read/write and queries.  These APIs must also provide the capability to retrieve the links to external data/models.

REQ-COMMON-APP:
: Digital Map models and APIs must be common over different network domains (campus, core, data center, etc.). This means that clients of the Digital Map API must be able to understand the topology model of layers of any domain without having to understand the details of any technologies and domains.

REQ-SEMANTIC:
: Digital Map must provide semantics for layered network topologies and for linking external models/data.

REQ-LAYER-NAVIGATE:
: Digital Map must provide intra-layer and inter-layer relationships.

REQ-EXTENSIBLE:
: Digital Map must be extensible with metadata.

REQ-PLUGG:
: Digital Map must be pluggable. That is,

     + Must connect to other YANG modules for inventory, configuration, assurance, etc.
     + Given that no all involved components can be available using YANG, there is a need to connect Digital Map YANG model with other modelling mechanisms.

REQ-GRAPH-TRAVERSAL:
: Digital Map must be optimized for graph traversal for paths. This means that only providing link nodes and
source and sink relationships to termination-points may not be sufficient, we may need to have the direct
relationship between the termination points or nodes.

## Design Requirements

The following are design requirements for modelling the Digital Map:

REQ-TOPO-ONLY:
: Digital Map should contain only topological information.
:  Digital Map is not required to contain all data required for
all the management and use cases. However, it should be designed to support adequate pointers to other functional Digital
Twin data and models to ease navigating in the overall system. For example:

  + ACLs and Route Policies are not required to be supported in the Digital Map, they would be linked to Digital Map
  + Dynamic paths may either be outside of the Digital Map or part of traffic engineering data/models

REQ-PROPERTIES:
: Digital Map entities should mainly contain properties used to identify topological entities at different layers,
identify their roles, and topological relationships between them.

REQ-RELATIONSHIPS:
: Digital Map should contain only topological relationships inside each layer or between the layers (underlay/overlay).

REQ-CONDITIONAL:
: Provide capability for conditional retrieval of parts of Digital Map.

REQ-TEMPO-HISTO:
: Must support geo-spatial, temporal, and historical data.  The temporal and historical can be supported external
to the Digital Map.

## Architectural Requirements

The following are the architectural requirements for the Digital Map:

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

*  Network Inventory Model {{?I-D.ietf-ivy-network-inventory-yang}} focuses on physical and virtual inventory. Logical inventory is currently outside of the scope.
It does not augment RFC8345 like the two Internet-Drafts that it evolved from {{?I-D.ietf-ccamp-network-inventory-yang}}
and {{?I-D.wzwb-opsawg-network-inventory-management}}. {{?I-D.ietf-ivy-network-inventory-topology}}
correlates the network inventory with the general topology via RFC8345 augmentations that reference inventory.

*  KPIs: delay, jitter, loss

*  Attachment Circuits (ACs) {{?I-D.ietf-opsawg-ntw-attachment-circuit}} and {{?I-D.ietf-opsawg-teas-attachment-circuit}}

*  Configuration: L2SM {{?RFC8466}}, L3SM {{?RFC8299}}, L2NM {{?RFC9291}}, and L3NM {{?RFC9182}}

*  Incident Management for Network Services {{?I-D.draft-ietf-nmop-network-incident-yang}}

# Acknowledgments
{:numbered="false"}

Many thanks to Mohamed Boucadair (<mohamed.boucadair@orange.com>) for his valuable contributions, reviews, and comments.

Many thanks to Nigel Davis <ndavis@ciena.com> for the valuable discussions and his confirmation of the modelling requirements.
