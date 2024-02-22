## Title

Choosing an Architectural Communication Pattern

## Context

MonitorMe is intended to integrate with other systems that are already in use, so that makes communication one of the most important decisions to address. Let's start here!

After reviewing all of the provided materials like StayHealthy company information and MonitorMe requirements, we have a general understanding of the technical scope for this project. We used this information to play out several key workflows and identify their actors and actions. We then assessed the feasibility of several applicable communication patterns to find the best fit.

## Decision

Event-Driven Pub-Sub

We believe the type, volume, speed, durability, and multi-destination nature of live biometric data such as vital signs benefit most from an Event-Driven architecture with a Publisher-Subscriber message model.

## Consequences, Trade-Offs, and Impacts

Several other architectural communication patterns were analyzed:

Request/Response - The nature of vital sign data is unidirectional, so the ability for delivering responses to a requester is not of much value here.

Choreography - We chose not to pursue this model due to the complexity of failure handling where some vital metrics still need to function if others aren't working. We want the communication pathways to be as simple and high speed as possible in this design, to support live patient monitoring.

Orchestration - We chose not to create an architecture with an orchestrator because it is less important to process every message and we would not want outdated or backlogged orchestration actions to be taken before newly received data was processed. The system's state can be updated based on receipt of new information and does not need internal orchestration at this time based on current requirements.

CQRS - We feel the nature of the medical devices in use here already sufficiently separate reads from writes and don't require further optimization at this time.

Sagas - Avoiding this complexity at the start, and focusing on event stream context for assessing data consistency.
