# Title
Preferred communication direction

## Context
MonitorMe includes several connected components and setting a preferred direction of communication in the architecture style can help advise decisions between different flavors of technology.

## Decision

Pushy! (haha) 

We believe the best decision with live monitoring data is to continue the flow of pushing information from device to care professional. This can manifest in areas such as Event Publication or WebSocket Messaging. We seek to avoid polling patterns.

## Consequences, Trade-Offs, and Impacts

Being pushy places a majority of the responisbility on the sender. 