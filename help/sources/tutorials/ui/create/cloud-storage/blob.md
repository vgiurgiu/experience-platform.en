---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Create an Azure Blob source connector in the UI
topic: overview
---

# Create an [!DNL Azure Blob] source connector in the UI

Source connectors in Adobe Experience Platform provide the ability to ingest externally sourced data on a scheduled basis. This tutorial provides steps for creating an [!DNL Azure Blob] (hereinafter referred to as "Blob") using the [!DNL Platform] user interface.

## Getting started

This tutorial requires a working understanding of the following components of Adobe Experience Platform:

-   [Experience Data Model (XDM) System](../../../../../xdm/home.md): The standardized framework by which Experience Platform organizes customer experience data.
    -   [Basics of schema composition](../../../../../xdm/schema/composition.md): Learn about the basic building blocks of XDM schemas, including key principles and best practices in schema composition.
    -   [Schema Editor tutorial](../../../../../xdm/tutorials/create-schema-ui.md): Learn how to create custom schemas using the Schema Editor UI.
-   [Real-time Customer Profile](../../../../../profile/home.md): Provides a unified, real-time consumer profile based on aggregated data from multiple sources.

If you already have a Blob base connection, you may skip the remainder of this document and proceed to the tutorial on [configuring a dataflow](../../dataflow/batch/cloud-storage.md).

### Supported file formats

[!DNL Experience Platform] supports the following file formats to be ingested from external storages:

-   Delimiter-separated values (DSV): Support for DSV formatted data files is currently limited to comma-separated values. The value of field headers within DSV formatted files must only consist of alphanumeric characters and underscores. Support for general DSV files will be provided in the future.
-   JavaScript Object Notation (JSON): JSON formatted data files must be XDM compliant.
-   Apache Parquet: Parquet formatted data files must be XDM compliant.

### Gather required credentials

In order to access your Blob storage on [!DNL Platform], you must provide a valid value for the following credential:

| Credential | Description |
| ---------- | ----------- |
| `connectionString` | The connection string required to access data in your Blob storage. The Blob connection string pattern is: `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. |

For more information on getting started, visit [this Azure Blob document](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string).

## Connect your Blob account

Once you have gathered your required credentials, you can follow the steps below to create a new Blob account to connect to [!DNL Platform].

Log in to [Adobe Experience Platform](https://platform.adobe.com) and then select **[!UICONTROL Sources]** from the left navigation bar to access the *[!UICONTROL Sources]* workspace. The *[!UICONTROL Catalog]* screen displays a variety of sources for which you can create an inbound account with, and each source shows the number of existing accounts and dataflows associated with them.

You can select the appropriate category from the catalog on the left-hand side of your screen. Alternatively, you can find the specific source you wish to work with using the search option.

Under the *[!UICONTROL Databases]* category, select **[!UICONTROL Azure Blob Storage]** followed by **[!UICONTROL Add data]** to create a new [!DNL Blob]connector.

![catalog](../../../../images/tutorials/create/blob/catalog.png)

The *[!UICONTROL Connect to Azure Blob Storage]* page appears. On this page, you can either use new credentials or existing credentials.

### New account

If you are using new credentials, select **[!UICONTROL New account]**. On the input form that appears, provide the connection with a name, an optional description, and your [!DNL Blob] credentials. When finished, select **[!UICONTROL Connect]** and then allow some time for the new account to establish.

![connect](../../../../images/tutorials/create/blob/new.png)

### Existing account

To connect an existing account, select the [!DNL Blob] account you want to connect with, then select **[!UICONTROL Next]** to proceed.

![existing](../../../../images/tutorials/create/blob/existing.png)

## Next steps and additional resources

By following this tutorial, you have established a connection to your [!DNL Blob] account. You can now continue on to the next tutorial and [configure a dataflow to bring data from your cloud storage into Platform](../../dataflow/batch/cloud-storage.md).