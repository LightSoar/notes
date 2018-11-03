
# OPC-UA summary

> OPC is the one -- universally accepted -- standard delivering the ability to exchange data between different industrial automation system in manufacturing and process industry.<sup>[ref](#OpcUa-book)</sup>

## OPC Classic
The major OPC specifications:
1. **DA**, Data Access
    * Interface to current process data (read, write, monitor)
    * Tranfer data from PLCs/DCSs to HMIs in real time
    * Explicit variable access by id (address space hierarchy)
1. **A&E**, Alarm & Events
    * Event notifications (client can subscribe and set up a filter)
    * Alarm notifications (client can subscribe and set up a filter)
         * Acknowledgement of alarms
1. **HDA**, Historical Data Access
    * Archived data


### Information exchange
1. OPC server
    * offer process data
    * assign data type to items in the hierarchy
1. OPC DA client
    * connect to OPC DA server by creating an OPCServer object
    * read/write data (subject to access rights), using an OCPGroup object
        * update rate is defined per-group
        * server updates client only on value change events
        * delivered data has timestamp and quality (accurate/good, not available/bad, or unknown/uncertain)
1. OPC A&E client
    * connects to OPC A&E server using OPCEventServer and OPCEventSubscription objects
1. OPC HDA client
    * read/write logged data
    * connects to OPC HDA server using OPCHDAServer and OPCHDABrowser objects

### Typical use cases
1. A single machine running DA client, A&E server and HDA server:
    * DA client obtains data from DA servers running on PLC, DCS, etc.
    * A&E server offers data to an A&E client.
    * HDA server offers data to an HDA client (HMI).

## Address space
TODO

## OPC UA (aka IEC 62541)

### Fundamental components
1. transport mechanisms
    * protocol-independent abstract communication model
1. data modeling
    * DA, AC, HA, Prog
    * 3rd party specs and extensions (e.g. PLCopen)
1. Data transport
2. Infromation modeling
    * Information Model specification
    * Object oriented techniques (type hierarchies, inheritance)
    * Done on server side

### Specifications
The key documents are in bold:

1. Concepts
1. Security Model
1. **Address Space Model**
1. **Services**
1. Information Model
1. Service Mappings
1. Profiles
1. Data Access
1. Alarm and Conditions
1. Programs
1. Historical Access
1. Discovery
1. Aggregates



> Applications consuming and providing data can be both client and server.[OPC-UA book][OpcUa-book]

## References
1. <a name="OpcUa-book">[OPC-UA book]</a> W. Mahnke, S.-H. Leitner, and M. Damm, _[OPC Unified Architecture](http://www.amazon.com/OPC-Unified-Architecture-Wolfgang-Mahnke/dp/3540688986/ref=sr_1_1?ie=UTF8&s=books&qid=1209506074&sr=8-1)_, 2009 edition. Berlin: Springer, 2009.

