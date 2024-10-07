# Confidential Compute

{% hint style="warning" %}
Verida Confidential Compute is currently in private alpha
{% endhint %}

Verida offers a self-sovereign compute infrastructure stack that exists on top of confidential compute infrastructure.

The self-sovereign compute infrastructure provides the following guarantees:

1. User data is not accessible by infrastructure node operators.
2. Runtime code can be verified to ensure it is running the expected code.
3. Users are in complete control over their private data and can grant / revoke access to third parties at any time.
4. Third-party developers can build and deploy code that will operate on user data in a confidential manner.
5. Users are in complete control over the compute services that can operate on their data and can grant / revoke access to third parties at any time.

<figure><img src="../../.gitbook/assets/Screenshot 2024-10-03 at 3.39.44 PM.png" alt=""><figcaption></figcaption></figure>

There are two distinct types of compute that have different infrastructure requirements; Stateless Confidential Compute and Stateful Confidential Compute.

## Stateless (Generic) Confidential Compute <a href="#id-4d14" id="id-4d14"></a>

This type of computation is stateless, it retains no user data between API requests. However, it can request user data from other APIs and process that user data in a confidential manner.

Here are some examples of Generic Stateless Compute that would operate on the network.

**Private Data Bridge** facilitates users connecting to third-party platform APIs (ie: Meta, Google, Amazon, etc.). These nodes must operate in a confidential manner as they store API secrets, handle end user access / refresh tokens to the third-party platforms, pull sensitive user data from those platforms, and then use private user keys to store that data in users’ private databases on the Verida network.

**LLM APIs** accept user prompts that contain sensitive user data, so they must operate in a confidential compute environment.

**AI APIs** such as AI prompt services and AI agent services provide the “glue” to interact between user data and LLMs. An AI service can use the User Data APIs (see below) to directly access user data. This enables it to facilitate retrieval-augmented generation (RAG) via the LLM APIs, leveraging user data. These APIs may also save data back to users’ databases as a result of a request (i.e., saving data into a vector database for future RAG queries).

## Stateful (User) Confidential Compute <a href="#ceb7" id="ceb7"></a>

This type of computation is stateful, where user data remains available (in memory) for an extended period of time. This enhances performance and, ultimately, the user experience for end users.

A User Data API will enable authorized third party applications (such as private AI agents) to easily and quickly access decrypted private user data. It is assumed there is a single User Data API, however in reality it is likely there will be multiple API services that operate on different infrastructure.

Here are some examples of the types of data that would be available for access:

1. Chat history across multiple platforms (Telegram, Signal, Slack, Whatsapp, etc.)
2. Web browser history
3. Corporate knowledge base (ie: Notion, Google Drive, etc)
4. Emails
5. Financial transactions
6. Product purchases
7. Health data

Each of these data types have different volumes and sizes, which will also differ between users. It’s expected the total storage required for an individual user would be somewhere between 100MB and 2GB, whereas enterprise knowledge bases will be much larger.
