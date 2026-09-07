# Updates

It hasn't been to long so I don't have any updates, just more details on how I want my dashboard set up.

## Dashboard

To start off, I want to give more context as to what kind of dashboard I want so that a better image can be painted.

I want a dashboard capable of displaying deep, in-depth analytics about all parts of the system. This includes a live display of resources being used, VM/LXC uptime and resources consumed by them, hardware temperatures, important disk health and data integrity metrics pulled from SMART data, routine tests, and scrubs, and various graphs/charts to give me better visualizations of overall system performance. This is a lot of data, so a lot of it will be abstracted from the dashboard being displayed.

On top of this, I also want notifications in the event that a metric hits its recommended limit or a service goes down.

### The Presentation Layer
I see 2 options for this layer. 
