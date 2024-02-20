# Title
Choosing a datastore, or more!

## Context
MonitorMe temporarily stores biometric vital sign information for needs such as displaying, alerting, correlating, and reporting. The datastore must be highly durable and performant. The vital sign information is mostly textual and time-series. 

## Decision



## Consequences, Trade-Offs, and Impacts

Since the data is never editable, always a snapshot type of data this is a great candidate for nosql.

But if you have to allow capability to go back 24 hrs and visualize that data...a traditional table based DB will be better off - sql lite.