---
title: Microsoft 365 external sharing and B2B collaboration - Azure AD
description: Discusses sharing resources with external partners using Microsoft 365 and Azure Active Directory B2B collaboration.

services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/04/2021

ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal

ms.collection: M365-identity-device-management
---

# Microsoft 365 external sharing and Azure Active Directory (Azure AD) B2B collaboration

In both Azure AD B2B collaboration and Microsoft 365 external sharing (OneDrive, SharePoint Online, Unified Groups, etc.), external users are authenticated by using Azure AD B2B.

## How does Azure AD B2B differ from external sharing in SharePoint Online?

OneDrive/SharePoint Online has a separate invitation manager. Support for external sharing in OneDrive/SharePoint Online started before Azure AD developed its support. Over time, OneDrive/SharePoint Online external sharing has accrued several features and many millions of users who use the product's in-built sharing pattern. However, there are some subtle differences between how OneDrive/SharePoint Online external sharing works and how Azure AD B2B collaboration works. You can learn more about OneDrive/SharePoint Online external sharing in [External sharing overview](/sharepoint/external-sharing-overview). The process generally differs from Azure AD B2B in these ways:

- OneDrive/SharePoint Online adds users to the directory after users have redeemed their invitations. So, before redemption, you don't see the user in Azure AD portal. If another site invites a user in the meantime, a new invitation is generated. However, when you use Azure AD B2B collaboration, users are added immediately on invitation so that they show up everywhere.

- The redemption experience in OneDrive/SharePoint Online looks different from the experience in Azure AD B2B collaboration. After a user redeems an invitation, the experiences look alike.

- Azure AD B2B collaboration invited users can be picked from OneDrive/SharePoint Online sharing dialog boxes. OneDrive/SharePoint Online invited users also show up in Azure AD after they redeem their invitations.

- The licensing requirements differ. To learn more about licensing, see [Azure AD External Identities licensing](./external-identities-pricing.md) and [the SharePoint Online external sharing overview](/sharepoint/external-sharing-overview).
To manage external sharing in OneDrive/SharePoint Online with Azure AD B2B collaboration, set the OneDrive/SharePoint Online external sharing setting to **Allow sharing only with the external users that already exist in your organization's directory**. Users can go to externally shared sites and pick from external collaborators that the admin has added. The admin can add the external collaborators through the B2B collaboration invitation APIs.


![The OneDrive/SharePoint external sharing setting](media/o365-external-user/odsp-sharing-setting.png)

After enabling external sharing, the ability to search for existing guest users in the SharePoint Online (SPO) people picker is OFF by default to match legacy behavior.

You can enable this feature by using the setting 'ShowPeoplePickerSuggestionsForGuestUsers' at the tenant and site collection level. You can set the feature using the Set-SPOTenant and Set-SPOSite cmdlets, which allow members to search all existing guest users in the directory. Changes in the tenant scope do not affect already provisioned SPO sites.

## Next steps

* [What is Azure AD B2B collaboration?](what-is-b2b.md)
* [Adding a B2B collaboration user to a role](./add-users-administrator.md)
* [Delegate B2B collaboration invitations](external-collaboration-settings-configure.md)
* [Dynamic groups and B2B collaboration](use-dynamic-groups.md)
* [Troubleshooting Azure Active Directory B2B collaboration](troubleshoot.md)