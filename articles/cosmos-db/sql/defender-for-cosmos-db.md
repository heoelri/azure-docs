---
title: 'Microsoft Defender for Azure Cosmos DB'
description: Learn how Microsoft Defender provides advanced threat protection on Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 02/03/2022
ms.author: thweiss
author: ThomasWeiss
---

# Microsoft Defender for Cosmos DB (Preview)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

Microsoft Defender for Cosmos DB provides an extra layer of security intelligence that detects unusual and potentially harmful attempts to access or exploit Azure Cosmos DB accounts. This layer of protection allows you to address threats, even without being a security expert, and integrate them with central security monitoring systems.

Security alerts are triggered when anomalies in activity occur. These security alerts show up in [Microsoft Defender for Cloud](https://azure.microsoft.com/services/security-center/). Subscription administrators also get these alerts over email, with details of the suspicious activity and recommendations on how to investigate and remediate the threats.

> [!NOTE]
>
> * Microsoft Defender for Cosmos DB is currently available only for the Core (SQL) API.
> * Microsoft Defender for Cosmos DB is not currently available in Azure government and sovereign cloud regions.

For a full investigation experience of the security alerts, we recommended enabling [diagnostic logging in Azure Cosmos DB](../monitor-cosmos-db.md), which logs operations on the database itself, including CRUD operations on all documents, containers, and databases.

## Threat types

Microsoft Defender for Cosmos DB detects anomalous activities indicating unusual and potentially harmful attempts to access or exploit databases. It can currently trigger the following alerts:

- **Access from unusual locations**: This alert is triggered when there is a change in the access pattern to an Azure Cosmos DB account, where someone has connected to the Azure Cosmos DB endpoint from an unusual geographical location. In some cases, the alert detects a legitimate action, meaning a new application or developer’s maintenance operation. In other cases, the alert detects a malicious action from a former employee, external attacker, etc.

- **Unusual data extraction**: This alert is triggered when a client is extracting an unusual amount of data from an Azure Cosmos DB account. It can be the symptom of some data exfiltration performed to transfer all the data stored in the account to an external data store.

## Configure Microsoft Defender for Cosmos DB

You can configure Microsoft Defender protection in any of several ways, described in the following sections.

# [Portal](#tab/azure-portal)

1. Launch the Azure portal at  [https://portal.azure.com](https://portal.azure.com/).

2. From the Azure Cosmos DB account, from the **Settings** menu, select **Microsoft Defender for Cloud**.

    :::image type="content" source="./media/defender-for-cosmos-db/cosmos-db-atp.png" alt-text="Set up Azure Defender for Cosmos DB" border="true":::

3. In the **Microsoft Defender for Cloud** configuration blade:

    * Change the option from **OFF** to **ON**.
    * Click **Save**.

# [REST API](#tab/rest-api)

Use Rest API commands to create, update, or get the Azure Defender setting for a specific Azure Cosmos DB account.

* [Advanced Threat Protection - Create](/rest/api/securitycenter/advancedthreatprotection/create)
* [Advanced Threat Protection - Get](/rest/api/securitycenter/advancedthreatprotection/get)

# [PowerShell](#tab/azure-powershell)

Use the following PowerShell cmdlets:

* [Enable Advanced Threat Protection](/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
* [Get Advanced Threat Protection](/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
* [Disable Advanced Threat Protection](/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

# [ARM template](#tab/arm-template)

Use an Azure Resource Manager (ARM) template to set up Azure Cosmos DB with Azure Defender protection enabled. For more information, see
[Create a CosmosDB Account with Advanced Threat Protection](https://azure.microsoft.com/resources/templates/cosmosdb-advanced-threat-protection-create-account/).

# [Azure Policy](#tab/azure-policy)

Use an Azure Policy to enable Azure Defender for Cosmos DB.

1. Launch the Azure **Policy - Definitions** page, and search for the **Deploy Advanced Threat Protection for Cosmos DB** policy.

    :::image type="content" source="./media/defender-for-cosmos-db/cosmos-db.png" alt-text="Search Policy"::: 

1. Click on the **Deploy Advanced Threat Protection for CosmosDB** policy, and then click **Assign**.

    :::image type="content" source="./media/defender-for-cosmos-db/cosmos-db-atp-policy.png" alt-text="Select Subscription Or Group":::

1. From the **Scope** field, click the three dots, select an Azure subscription or resource group, and then click **Select**.

    :::image type="content" source="./media/defender-for-cosmos-db/cosmos-db-atp-details.png" alt-text="Policy Definitions Page":::

1. Enter the other parameters, and click **Assign**.

---

## Manage security alerts

When Azure Cosmos DB activity anomalies occur, a security alert is triggered with information about the suspicious security event. 

 From Microsoft Defender for Cloud, you can review and manage your current [security alerts](../../security-center/security-center-alerts-overview.md).  Click on a specific alert in [Defender for Cloud](https://ms.portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/0) to view possible causes and recommended actions to investigate and mitigate the potential threat. The following image shows an example of alert details provided in Defender for Cloud.

 :::image type="content" source="./media/defender-for-cosmos-db/cosmos-db-alert-details.png" alt-text="Threat details":::

An email notification is also sent with the alert details and recommended actions. The following image shows an example of an alert email.

 :::image type="content" source="./media/defender-for-cosmos-db/cosmos-db-alert.png" alt-text="Alert details":::

## Azure Cosmos DB alerts

 To see a list of the alerts generated when monitoring Azure Cosmos DB accounts, see the [Azure Cosmos DB alerts](../../security-center/alerts-reference.md#alerts-azurecosmos) section in the Microsoft Defender for Cloud documentation.

## Next steps

* Learn more about [Diagnostic logging in Azure Cosmos DB](../cosmosdb-monitor-resource-logs.md)
* Learn more about [Microsoft Defender for Cloud](../../security-center/security-center-introduction.md)
