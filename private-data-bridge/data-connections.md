# Data Connections

Verida's Data Connections framework makes it easy for users to connect and pull their personal data from platforms like Google, Telegram, or Facebook into their private [verida-vault.md](verida-vault.md "mention"). The framework has data connectors to tasks like signing in, verifying access, and syncing user data from these centralized platforms.

For example, if you want to extract your email or contacts from Google, you sign into the Verida Vault and use the Google connection to securely transfer that data into your private, encrypted database on the Verida network, where you have full control.

The Verida Data Connection framework source code is open source and [available on GitHub](https://github.com/verida/data-connector-server/). A full list of connectors can be found in the [providers folder](https://github.com/verida/data-connector-server/tree/main/src/providers).

### Connection Handlers

Connections (ie: Google) have multiple handlers (ie: Gmail, Calendar etc.) that process different types of data available for a given connection. These handlers typically require specific permission to be gratned when authorizing the connection. For example; A user will need to permit the Data Bridge access to the "Calendar" otherwise that data won't syncronize.

Handlers have built-in options specific to their functionality. For example; The Telegram handler has an option to only sync groups with less than 50 participants.

{% hint style="warning" %}
Handler options are currently implemented in the backend and will be made available in the user interface soon.
{% endhint %}

### Connectors Roadmap

You can view a list of completed, in progress and planned connectors here:

[https://roadmap.verida.ai/](https://roadmap.verida.ai/)

### Request a Connector

You can request a new connector via our roadmap here:

[https://roadmap.verida.ai/data-connectors](https://roadmap.verida.ai/data-connectors)

### Fetching Historical Data

The default setting for all handlers is to fetch data up to 3 months old. In the future users will be able to customize this to increase the historical data to fetch. This will obviously increase the amount of data storage and memory usage for that user's data.
