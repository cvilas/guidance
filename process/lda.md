# Technical Note: Architecture for layered distributed systems

Vilas Chitrakaran, April 2018

- [Introduction](#introduction)
- [Layered Databus Architecture](#layered-databus-architecture)
- [Five design principles to improve reliability of IIoT applications](#five-design-principles-to-improve-reliability-of-iiot-applications)
  - [Communicate state](#communicate-state)
  - [Communicate time](#communicate-time)
  - [Externalise local state](#externalise-local-state)
  - [Implement idempotent processes](#implement-idempotent-processes)
  - [Implement commutative processes](#implement-commutative-processes)
- [References](#references)

## Introduction

This technical note describes a foundational architectural pattern for designing large distributed systems (sometimes called
industrial internet of things - IIoT). The key idea of 'Layered Databus Architecture' advocated by
[RTI](https://www.rti.com/blog/2017/01/31/2nd-version-of-the-industrial-internet-reference-architecture-is-out-with-layered-databus)
is described, followed by five specific ways to improve reliability of systems that use such an architecture. The
contents of this technical note are derived from my notes from a webinar organised by RTI.

## Layered Databus Architecture

The layered databus architecture is a means to design system-of-systems that are scalable, highly available (zero down
time) and fault tolerant. The system is designed around a hierarchical set of databuses where each databus is a logical
space of devices and applications. At the lowest level, it implements local control, automation and analytics
and higher levels implement supervisory control and monitoring.

![Layered Databus Architecture](../media/lda_layered_databus_architecture.png "layered databus architecture")

Each layer of the databus has the following implementation characteristics:

- Connectionless: Devices and applications do not rely on point-to-point connectivity (think UDP rather than TCP). The
  connectivity graph is dynamic with devices appearing/disappearing/reappearing across the system sharing a common databus.
  (Example: A networked patient monitoring system that continues to function as the patient is wheeled from one end of
  the hospital to another even through regions with no connectivity)
- Applications and devices have no knowledge of each other. They function by publishing and subscribing data over
  'topics' that are well known throughout the system.
- Components are loosely coupled and the databus automatically discovers them during runtime.

Mature communication standards such as [DDS](https://en.wikipedia.org/wiki/Data_Distribution_Service) as well as newly evolving new frameworks such as [Zenoh](https://zenoh.io/) support these characteristics.

![Databus Architecture](../media/lda_databus.png "databus architecture")

In a databus-oriented architecture, multiple devices (eg: displays, sensors) can be added or removed without affecting the
performance of the system. The architecture allows for implementing systems that grow in complexity over time, or
to even begin implementation of a system before all specifications and functionality are fully nailed down. For such a system
to be truly scalable, the system architecture has to also be 'data-centric'. In a data-centric architecture,

- Applications connect to data, not to each other; i.e., the data *is* the interface.
- There is no direct communication mechanism between applications; i.e. no explicit client-server type relationship
- The data has an associated 'datatype' that is well known to all (reinforcing the idea of the data itself being the
  interface)

![Connect to Data](../media/lda_connect_to_data.png "connect to data")

Data is not stored anywhere but cached with every publisher and subscriber. The databus shares data and the DDS protocol
provides filtering mechanisms to query and extract the right data in real-time. This enables fast local access to latest
data without having to ask for it from the source device. There is practically no latency since a request-reply
transaction is avoided.

Where multiple devices that transmit the same datatype exist, 'keys' are used to uniquely identify a particular source
of data. For instance, in a distributed system containing multiple mobile robots publishing their pose data, the key
could be the unique ID assigned to each robot.

The layered databus approach applies not just to hierarchical system-of-systems. It also provides a common architecture
pattern applicable across multiple industries; eg: network of medical devices in a hospital, or multiple ground
control stations monitoring a spacecraft. Inspite of widely different requirements, both applications can be implemented
with the same databus architecture; what's different is the quality of service (QoS) guarantees individual components
of the system have to meet. For instance, in an inter-planetary space application, reliability of communication in the
face of limited bandwidth, multi-second latecy and dropouts is the main consideration. In a surgical robot application
where devices are remotely teleoperated via haptic feedback, precise closed loop control may require high data rates (sometimes
as high as 3kHz). QoS specifications balance throughput and latency across interconnects ('Reliable' vs 'BestEffort'),
time awareness (deadlines, timestamping), availability of historical data (for late joiners) and control over
subscribed data (filter by time, content, etc). [NASA's Orion programme](https://www.nasa.gov/exploration/systems/orion/index.html)
require monitoring over 300,000 data points on the spacecraft in real time, and implements the aforementioned
architecture to be able to do so. Even if parts of these systems are built on top of DDS implementations from different
vendors, interoperability is possible because DDS is an industry standard. Back on earth, consider an application
consisting of a fleet of autonomous mobile robots performing tasks in an industrial environment. There are
multiple data sources on any single robot:

![Data sources in a wheeled mobile robot](../media/lda_data_sources_amr.png "data sources - wheeled mobile robot")

High rate sensor measurements (eg: video stream from a camera) could potentially be tuned for 'best effort' rather than
reliability, since system performance is not affected if a few sensor readings are missed. On the other hand, signals
involved in feedback control have to be typically delivered at predictable and deterministic rates and are tuned for
reliability and throughput (review DDS QoS policies).

The higher level architecture of such a fleet of autonomous mobile robots could potentially be implemented as follows:

![LDA for a fleet of mobile robots](../media/lda_amr_fleet_arch.png "architecture for a fleet of AMRs")

At the lowest level, the robot-specific databus links local sensing, decision and actuation mechanisms for navigation,
control and payload operations. Supervisory control such as fleet management are implemented at site-level. Multiple
sites could further be hooked up to a global cloud-based service to monitor resource usage and performance across sites,
enabling robot-as-a-service (RaaS) type businesses to scale easily with increase in the number of client sites. The databus
architecture enables loose coupling of components at every level - components may go offline or reappear randomly.
However, this needn't imply low reliability and low availability. In fact, just the opposite if five design principles
described in the next section are followed.

## Five design principles to improve reliability of IIoT applications

Briefly they are:

1. Communicate state
2. Communicate time
3. Externalise local state
4. Implement idempotent processes
5. Implement commutative processes

Lets look at them one at a time to see how they deliver scalability, reliability, availability and fault tolerance.

### Communicate state

Design a data model to represent system state and then communicate that state, the entire state, over the bus.
Do *not* communicate messages *about* the state i.e. incremental changes to state. Resist the temptation
to conserve bandwidth by only communicating state changes.

Example: A micro-air-vehicle should publish current global pose to Ground Control rather than change from previous pose.
This allows Ground Control to have a consistent view of where the MAV is even if intermediate messages from the MAV are lost.

We trade bandwidth for intrinsic reliability from convergent state. Convergent state means both
ends (the publisher and the subscriber) eventually converge to the same view of the state in the presence of data loss.
The data link can be tuned for predictable performance (via throughput control QoS settings) as the published messages
are of a constant size (full state) streamed at a constant rate and not of variable size and rate depending on what
part of the state changed and when.

### Communicate time

Consider time to be an integral part of the system state. Timestamp all data (for sensors, this is the instant of
observation, not the instant at which data about that observation was produced). Including time in the data model
allows detection of missed datapoints. If a data message is missed by some nodes and not others in the system, at that
instant the information may no longer be *equivalent* across nodes but it is still *consistent*; the data is
valid, just that it applies to different points in time at different nodes.

Example: Upon missing state updates from the MAV, the Ground Control station need not infer that the MAV has
stopped moving (hanging in mid-air). The GCS display can choose to update its prediction of where the vehicle might be
based on extrapolating the previous string of timestamped state messages, or it can choose not to update at all as long
as it is clear to the user that they are looking at old (but valid) data.

### Externalise local state

Internal state of application processes is state too. Make applications publish and subscribe to own state. This results
in stateless processes. Benefits of stateless processes built on top of DDS include

- fault tolerance
- dynamic (re-)deployment
- monitoring and debugging.

How these benefits are achieved is explained with a motivating example.

Example: Consider a Kalman filter where `X` is a vector of motion sensor measurements, `Y` is the output state
consisting of speed and pose of tracked objects and the internal local state is `S` which could be the state
covariance estimate. A stateless Kalman filter would publish both output `Y` and internal state
`S`, and subscribe to `X` and also `S`, eliminating the need to keep a copy of the internal state `S` within the process
for subsequent updates. If now the Kalman filter process fails for some reason, the process can be restarted,
possibly in some other location in the distributed system. Since the process subscribes to all data it needs and holds
no internal state, it can immediately recover by reading the latest state from the data bus and continue from there.

Furthermore, a backup Kalman filter could remain deployed on active standby, already subscribed to the states and
measurements. It is then ready to take over immediately if the primary filter fails, providing robust fault tolerance.
The technique can also be used to actively switch across from one system to another during maintenance shutdowns
without causing any down time, and for load balancing by dynamically moving processes from one host to another without
disruption (i.e. dynamic re-deployment). Monitoring and debugging can be performed by a logging and visualisation
application that simply subscribes to the state of the process.

### Implement idempotent processes

An idempotent process produces identical results if the same inputs are offered repeatedly. Duplicates don't cause
errors or side-effects on such a system. Duplicate messages can happen due to alternate routing, multiple routes,
router failover, etc. In the previous Kalman filter example, duplicate sensor messages (that include a timestamp - see
previous design principle) should not cause the output state to diverge as the state update would merely be recomputed
for a previous timepoint.

Idempotent process can be made fault tolerant through redundancy (duplicate processes running in parallel) and allows
publishing state updates periodically using 'Best Effort' rather than 'reliable' QoS. Reliable QoS messages are
expensive on the network due to additional acknack metatraffic and repair messages. A lossy high latency network
can potentially be brought to its knees overwhelmed by repair messages and metatraffic. 'Best Effort' messages
transmitted at a fixed rate in these situations deliver predictable worst-case behaviour at the expense of bandwidth
usage. This is because it is easier to model the load on the system for fixed-rate fixed-size messages.

*Note* Unit test your applications on this property by feeding it duplicate inputs to make sure they are indeed
idempotent.

### Implement commutative processes

A commutative process produces identical results regardless of the order in which input data is received and processed.
Again, this requires *time* to be a part of the data model. Timestamp on the data allows past/stale messages to be
either ignored entirely, or used to improve state estimate (as in the previous Kalman filter example). This results in
a more responsive system.

*Note* As in the previous case, unit test your applications on this property by feeding it inputs in random order to
make sure they are indeed commutative.

## References

- A description of the three common architecture patterns (including the layered databus) implemented by organisations
  that use DDS are described in section 7.1 of "The Industrial Internet of Things," Volume G1: Reference
  Architecture, IIC:PUB:G1:V1.80:20170131, <https://www.iiconsortium.org/IIC_PUB_G1_V1.80_2017-01-31.pdf>

- Read the blog for an intro to LDA: <https://www.rti.com/blog/2017/01/31/2nd-version-of-the-industrial-internet-reference-architecture-is-out-with-layered-databus>
