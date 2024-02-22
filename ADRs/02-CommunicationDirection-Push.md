## Title

Preferred Communication Direction

## Context

MonitorMe includes several connected components, and setting a preferred direction of communication in the architectural style can help advise decisions between different flavors of technology.

## Decision

Pushy! (Haha)

We believe the best decision with live monitoring data is to continue the flow of pushing information from the device to the care professional. This can manifest in areas such as Event Publication or WebSocket Messaging. We seek to avoid polling patterns.

## Consequences, Trade-Offs, and Impacts

Being pushy places a majority of the responsibility on the sender.
