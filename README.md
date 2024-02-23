# MonitorMe by StayHealthy, Inc

_The Patient Comes First_

## Preamble

In response to the growing demand for advanced medical monitoring solutions, StayHealthy, Inc., a renowned name in the medical device industry, is embarking on a venture to strengthen its market position by introducing a cutting-edge patient vital signs monitor. This software application, named MonitorMe, aims to deliver a reliable and comprehensive solution tailored for hospital environments.

## The Vision

The vision behind MonitorMe is to offer hospitals a sophisticated yet user-friendly vital signs monitoring system that empowers healthcare professionals to provide superior patient care. Built on the foundation of _The Patient Comes First,_ MonitorMe combines advanced technology with intuitive design to set a new standard in medical monitoring, enhancing patient outcomes and streamlining clinical workflows.

![An introduction to StayHealthy devices](/images/ContextDiagram.png)

## Requirements

#### Data Acquisition:

The system must read data from 8 different patient-monitoring equipment, including heart rate, blood pressure (BP), oxygen level (O2), blood sugar, respiration rate, electrocardiogram (ECG), body temperature, and sleep status (sleep or awake).

#### Data Transmission:

Data must be transmitted to a consolidated monitoring screen (one per nurse's station).

#### Responsiveness:

Achieve an average response time of 1 second or less, ensuring vital signs are available on the screen within 1 second of data emission.

#### Display Rotation:

The monitoring screen should display each patient’s vital signs, rotating between patients every 5 seconds.
When an alert is received, the monitoring screen should switch to that patient.

#### Patient Limit:

The system should support a maximum of 20 patients per nurse's station.

#### Deployment:

Deploy as an on-premises solution with a maximum of 500 patients per installation.

#### Data Storage:

Store the past 24 hours of all vital sign readings.

#### Alerting Mechanism:

Analyze each patient’s vital signs and alert if issues are detected, such as oxygen drops or temperature exceeding a preset threshold. The trend and threshold analysis may depend on sleep vs. awake status. Medical professionals receive alerts via a mobile app or on the monitoring screen.

#### Fault Tolerance:

In case of any vital sign device failure, the system should ensure continued monitoring of the other feeds.

#### Data Export:

The system should enable export of consolidated snapshots to MyMedicalData through a secure HTTP API.

#### Data Frequency:

- HR: Capture heart rate data every 500ms.
- BP: Capture blood pressure data every hour.
- O2: Capture oxygen level data every 5 seconds.
- Blood sugar: Capture blood sugar data every 2 minutes.
- Respiration: Capture respiration rate data every second.
- ECG: Capture electrocardiogram data every second.
- Body temp: Capture body temperature data every 5 minutes.
- Sleep status: Capture sleep status data every 2 minutes.

#### Compliance:

No specific compliance requirements are specified.

These requirements outline the functionality and performance expectations for the MonitorMe solution.

## Actors and Actions

#### Actor: Nurse

- Connect vital sign monitoring devices to the patient and associate them with the patient's identity
- Monitor patient data using consolidated monitoring screen
- Respond to alerts displayed on consolidated monitoring screen

#### Actor: Vital Sign Device

- Publish vital sign reading to MonitorMe

#### Actor: MonitorMe System

- Read vital sign reading from device
- Record reading in database
- Analyze reading based on threshold/trends
- Mark reading as Normal/Warning/Critical
- Notify health care professional if reading is in Alert state
- Record patient history
- Display patient history
- Display vital sign reading in Consolidated Monitoring Screen

#### Actor: Medical Professionals

- Review patient vitals history, filtering on time range as well as vial sign
- Receive alert push notifications for potential problems
- Generate holistic snapshots of patient vital signs
- Upload vital sign snapshots to MyMedicalData


## Architectural Characteristics

#### Data Integrity 

The patient trusts that the software will not fail and will consistently monitor their vital signs to ensure their well-being. Each component in the MonitorMe system must ensure integrity of vital sign readings and compensate for potential sources of error. To this effect, all device readings received over the network are validated for legibility, patient identification, and null values caused by disconnected devices. Prior to validation, readings are stored in a durable queue to ensure none are lost. After validation, readings are synchronously recorded in the database via writes to a high speed cache service before the next reading is processed. The cache can then ensure data integriry with asynchronous writes to the underlying database. 

#### Performance

The patient expects timely and accurate updates on their vital signs, ensuring prompt medical attention when needed. Each MonitorMe installation is designed to support a defined number of vital sign devices and number of patients. Processes with specific performance needs, such as the Current Readings Retriever which delivers patient vital sign information to the Nurse Station, are separated from other processes such as the Batch Readings Retriever. All Database connections with performance requirements are fronted with caches. 

#### Availability

The patient relies on the software to be readily accessible, ensuring continuous monitoring of their health status around the clock. Healthcare doesn't sleep and neither does MonitorMe, with availability targets of 99.99%. 

#### Fault Tolerance

The patient feels secure knowing that even if a component fails, the software will continue to monitor their vital signs without interruption. Faulty vital sign devices are identified by the Inbound Reading Validator and Nurses can replace the device at the point of care. MonitorMe services are installed on servers in an active/passive primary/secondary configuration, with automatic failover in case of server fault. StayHealthy recommends installing the secondary MonitorMe server in a physically diverse location from the primary server to ensure maximum fault tolerance from physical disruption, while still meeting network performance requirements.

#### Responsiveness

The patient appreciates the software's quick response to any changes in their vital signs, providing reassurance and peace of mind. Nurses appreciate quick information updates and alerts. The system need to meet defined requirements for speed of updates and alerts. 

#### Deployability

Hospital IT must be able to deploy this hardware, implement the MonitorMe system, and maintain the technology. 


## Nurse Station Consolidated Monitoring Screen Mockup

![Mockup of monitoring screen](/images/Mockup.jpg)

## Architecture Diagram

![Architecture diagram](/images/ArchitectureDiagram.png)

- Queues
  - Facilitates asynchronous communication of vital sign readings.
- Inbound Reading Validator
  - Filters noise using proprietary StayHealthy algorithms.
- Reading Recorder
  - Records a vital sign reading in the cache and the database.
- Cache
  - Stores single-point values of the latest vital sign readings.
  - Stores snapshots of point-in-time consolidated readings.
- Readings Context Classifier
  - Applies thresholds to readings and classifies readings as Normal, Warning, or Critical.
- Alert Notifier
  - Pushes alert notifications to subscribers.
- Current Readings Retriever
  - Retrieves the most recent vital sign readings for a specific patient, enabling real-time monitoring and assessment of their current health status.
- Batch Readings Retriever
  - Retrieves holistic snapshot request.
- Patient Device Assignment Recorder
  - Records the assignment of a patient to a device.
- Snapshot Consolidated Readings Generator
  - Generates a consolidate reading for exporting to MyMedicalData.
 
![Systems diagram](/images/SystemsDiagram.png)  

## Architecture Decision Records (ADRs)

- [Choosing an Architectural Communication Pattern](/ADRs/01-CommunicationPattern-UsingEventPubSub.md)
- [Preferred Communication Direction](/ADRs/02-CommunicationDirection-Push.md)
- [Choosing Communication Protocols](/ADRs/03-CommunicationProtocols-UNDECIDED.md)
- [To Cache or Not to Cache](/ADRs/04-Caching-Yes.md)
- [On the Use of Queues](/ADRs/05-Queuing-Yes.md)
- [Choosing a datastore, or more!](/ADRs/06-Datastores-UNDECIDED.md)
- [Decision on Service Architecture](/ADRs/07-ServiceSingularity-GenericService.md)
