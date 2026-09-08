# Updates

It hasn't been to long so I don't have any updates, just more details on how I want my dashboard set up.

## Dashboard

To start off, I want to give more context as to what kind of dashboard I want.

I want a dashboard capable of displaying deep, in-depth analytics about all parts of the system. This includes a live display of resources being used, VM/LXC uptime and resources consumed by them, hardware temperatures, important disk health and data integrity metrics pulled from SMART data, routine tests, and scrubs, and various graphs/charts to give me better visualizations of overall system performance. This is a lot of data, so a lot of it will be abstracted from the dashboard being displayed. On top of this, I also want notifications in the event that a metric hits its recommended limit or a service goes down.

Now let's talk a bit a bit on what I've learned about enterprise dashboards

Enterprise dashboards are built for both observability and monitoring. In short, the difference between the two is that monitoring tells the user that the service is down, while observability tells you why it is down. Both work in tandem to manage system health.

Furthermore, observability splits into 3 different pillars:
 - Metrics: The actual numerical values over time. Examples include current CPU utilization, disk usage, hard drive temperature.
 - Logs: Discrete event records. In other words, each log includes an event, a timestamp, and an optional change of state in an area of the system.
 - Traces: The start-to-end path that requests take across multiple services. For my specific situation, these are less relevant and will most likely not be implemented.

Now, each enterprise observability stack follows the same pipeline: Data source > Collection service > Storage > Query layer > Visualization > Alerting.

### Data Source
### Collection Service
### Storage
### Query Layer
### Visualization
### Alerting

Based on these requirements, I'm thinking of using these tools:
 - Node Exporter: To collect and export raw metrics data from the system
 - Prometheus: To collect data from various sources, including node exporter, and saves them in a time-series database
 - Uptime Kuma: To monitor the uptime status of my VMs and LXCs. Also offers alerting if something goes down
 - Grafana: I'm going to use this to combine the data provided by Uptime Kuma and Prometheus to build my dashboard

