# Updates

It hasn't been to long so I don't have any updates, just more details on how I want my dashboard set up.

## Dashboard

To start off, I want to give more context as to what kind of dashboard I want.

I want a dashboard capable of displaying deep, in-depth analytics about all parts of the system. This includes a live display of resources being used, VM/LXC uptime and resources consumed by them, hardware temperatures, important disk health and data integrity metrics pulled from SMART data, routine tests, and scrubs, and various graphs/charts to give me better visualizations of overall system performance. This is a lot of data, so a lot of it will be abstracted from the dashboard being displayed. On top of this, I also want notifications in the event that a metric hits its recommended limit or a service goes down.

Based on these requirements, I'm thinking of using these tools:
 - Prometheus: To collect data from various sources, including node exporter, and saves them in a time-series database
 - Uptime Kuma: To monitor the uptime status of my VMs and LXCs. Also offers alerting if something goes down
 - Grafana: I'm going to use this to combine the data provided by Uptime Kuma and Prometheus to build my dashboard

Now let's talk a bit a bit on what I've learned about enterprise dashboards

Enterprise dashboards are built for both observability and monitoring. In short, the difference between the two is that monitoring tells the user that the service is down, while observability tells you why it is down. Both work in tandem to manage system health.

Furthermore, observability splits into 3 different pillars:
 - Metrics: The actual numerical values over time. Examples include current CPU utilization, disk usage, hard drive temperature.
 - Logs: Discrete event records. In other words, each log includes an event, a timestamp, and an optional change of state in an area of the system.
 - Traces: The start-to-end path that requests take across multiple services. For my specific situation, these are less relevant and will most likely not be implemented.

Now, each enterprise observability stack follows the same pipeline: Data source > Collection service > Storage > Query layer > Visualization > Alerting.

### Data Source
Consists of VMs, LXCs, servers, databases, applications - anything that generates data needed to needed to measure the state of a system.

### Collection Service
Processes that run near the data source that gather metrics and ship them forward. In my use case, this would be the node_exporter part of Prometheus

### Storage
Time-series databases (TSDB) are used for metrics storage and inverted-index text stores for logs storage instead of the more familiar relational databases. In my case, I'll most likely be using Prometheus's TSDB and Grafana Loki for logs. Enterprises split data retention into 2 tiers of storage: hot storage (30 days data retention) and cold storage (90+ days).

### Query Layer
Where raw telemetry is used to investigate system behaviour, debug incidents, and understand performance.

### Visualization
Turning the queried data into a visualization understandable to the user. In my context, this would be in the form of Grafana dashboards.

### Alerting
Rules evaluate metrics continuously to trigger the sending of notifications or automated remediation. For my case, this would be Grafana's built-in alerting or Prometheus's AlertManager. 


Some other dashboard ideas:
 - Overview Dashboard: High level dashboard meant to give the reader a quick rundown of system performance. Includes colours like green/yellow/red to represent uptime, some brief charts/graphs, minimal numbers. Clicking on a section of of the dashboard could reveal more detailed insights on that specific part of the dashboard.
 - Service-specific Dashboards: One dashboard per service. These dashboards go into more depth for each service, but should stay relatively brief while still providing the reader with insights on overall performance and health.

To end it off, these are some of the metrics and data that I'd like to keep track of:
 - CPU utilization
 - Memory usage
 - Disk I/O (throughput, IOPS, queue depth/latency)
 - Disk space
 - Network throughput, packet loss, errors
 - Any temperature/thermal data where available
 - ZFS pool health, scrub status, SMART data
 - VM and LXC uptime status
 - Any other data that falls under latency, traffic, errors, and saturation
