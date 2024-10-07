# Data Sharing

The Verida protocol offers a lot of flexibility and controls around data access and encryption.

This guide provides a deeper insight on how these tools can be helpful in building powerful and secure decentralized applications on the Verida network.

## Sending private data to a user (Incoming data) <a href="#sending-private-data-to-a-user-incoming-data" id="sending-private-data-to-a-user-incoming-data"></a>

**Scenario:** _Your application has personal data about an individual that you want to securely share with them as a once off._

**Example:** _You may generate a contact (name, phone) for a user in a centralized system and wish to securely send the contact to an individual so they can message them in the future._

**Assumptions:** _The personal data being shared is a single record and could be stored anywhere (either in a Verida storage node or a centralized database)_

### Developers <a href="#developers" id="developers"></a>

* [Data message documentation](../client-sdk/messaging.md)

{% hint style="success" %}
This model can be used to share more than one record but is only suitable for a small amount of data as it involves copying each data record via an encrypted Verida message.
{% endhint %}

## Requesting private data from a user (Data request)​ <a href="#requesting-private-data-from-a-user-data-request" id="requesting-private-data-from-a-user-data-request"></a>

**Scenario:** A user has personal data about themselves that your applications wants consent to obtain a copy of.\_

**Example:** You may request a copy of a driver's license credential from a user.\_

**Assumptions:** The personal data is a single record and is stored in a Verida storage node on the Verida network.\_

### Developers <a href="#developers-1" id="developers-1"></a>

* [Requesting data documentation](../client-sdk/messaging.md#requesting-data)

### Examples <a href="#examples" id="examples"></a>

* Request a copy of a credential (ie: proof of address, KYC)
* Request a user’s profile/preference record (dietary preferences, contact details, health preferences etc.)

## Creating an application data silo (Context data) <a href="#creating-an-application-data-silo-context-data" id="creating-an-application-data-silo-context-data"></a>

**Scenario:** _An application wants to create a secure collection of databases where the data is owned and controlled by the end-user_

**Example:** _A developer creates a decentralized game and wants to create a custom database of game metadata to store for the user. The game metadata can only be unlocked by the user via a consent sign-in message in the Verida Wallet_

**Assumptions:** _The data is proprietary to the application and a user is unlikely to want that data to be shared/used by other applications._

Access to this data is typically “unlocked” in real-time by sending a Connection Request to the user's Wallet, but the data isn’t visible by default in the Verida Wallet.

### Developers

Documentation examples will be coming shortly

## Sharing access to lots of private data (Data synchronization) <a href="#sharing-access-to-lots-of-private-data-data-synchronization" id="sharing-access-to-lots-of-private-data-data-synchronization"></a>

**Scenario:** _A user has a lot of personal data stored in their Verida Wallet that your application wants consent to securely read and write._

**Example:** _You may operate a medical practice that stores a patient's medical records and you wish to regularly read and write the patient’s medical data._

### Developers <a href="#developers-3" id="developers-3"></a>

Documentation examples will be coming shortly.

### Examples <a href="#examples-1" id="examples-1"></a>

* A medical practitioner can synchronize a patient’s last 6 months of health records
* A decentralized advertising network can request read-only access to a live stream of a user’s social media posts and personal preferences
* A user can update their phone number and it will be automatically updated with all third-party applications that have synchronized access to the user’s private profile

{% hint style="success" %}
This strategy can also be used to setup a real-time synchronization of private data from one application to another. This can also be extended to synchronize a sub-set of data by applying a realtime filter on the synchronized data.
{% endhint %}

### Data synchronization options <a href="#data-synchronization-options" id="data-synchronization-options"></a>

There is a lot of flexibility in how this data synchronization is configured. An application can request access to:

* A read and/or write stream of all data of a particular type
* Once off data synchronization
* Permanent sync (until the user disables)
* A filtered stream of data based on a query
