---
ms.assetid: 
title: View System Center Operations Manager’s alerts in Azure Monitor
description: This article describes the recent feature addition that allows Azure Monitor SCOM Managed Instance customers to view Operations Manager’s alerts in Azure Monitor.
author: PriskeyJeronika-MS
ms.author: v-gjeronika
manager: jsuri
ms.date: 05/22/2024
ms.service: azure-monitor
ms.subservice: operations-manager-managed-instance
ms.topic: how-to
---

# View System Center Operations Manager’s alerts in Azure Monitor

This article describes the recent feature addition that allows Azure Monitor SCOM Managed Instance customers to view Operations Manager’s alerts in Azure Monitor.

> [!VIDEO 09a0b07e-c50c-4ee4-b0a7-43d8ca6bb847]

## Alert

An alert is an object that is generated in System Center Operations Manager when one of the rules or monitors that are set in an Operations Manager management pack is triggered. In System Center - Operations Manager, an alert can be generated by a rule or a monitor. For more information on rules and monitors, see [Operations Manager management pack](/system-center/scom/manage-overview-management-pack).

For detailed information on how an alert is produced in Operations Manager, see [How an alert is produced](/system-center/scom/manage-alert-generation-overview).

## View and manage alerts in Operations Manager on-premises

In System Center Operations Manager, alerts matching a specific criterion and related to an object or group of objects are presented in an alerts view. From this view, you can review alerts that have been generated by rules and monitors, which are still active and haven't been closed automatically or manually by an operator. For more information on how alerts are viewed, see [View Active Alerts and Details](/system-center/scom/manage-alert-view-alerts-details).

Each alert has specific properties that inform the user about different factors such as the history of the alert, how it's generated, what are the objects affected etc. For a complete list of all the properties of a rule/monitor alert, see [Examining Properties of Alerts, Rules, and Monitors](/system-center/scom/manage-examine-properties-of-workflows).

## View Operations Manager’s alerts in Azure Monitor

For SCOM Managed Instance, the alerts that are generated in the workload can now be seen in Azure Monitor.  

Log in to the Azure portal, access the Azure Monitor service, and select **Alerts** tab to see a list of all the alerts that the service has generated.

:::image type="content" source="media/view-operations-manager-alerts-azure-monitor/alerts-inline.png" alt-text="Screenshot of Alerts page." lightbox="media/view-operations-manager-alerts-azure-monitor/alerts-expanded.png":::

To view the alerts that are generated by your SCOM Managed Instance service, select **SCOM Managed Instance** in the **Monitor service** filter.

:::image type="content" source="media/view-operations-manager-alerts-azure-monitor/monitor-service-filter.png" alt-text="Screenshot showing Monitor service filter.":::

Select an alert to view its details. Details include:

- Severity of the alert
- Fired Time
- Affected Resource
- Hierarchy of the affected resource group
- User Response
- Alert Condition
- Is it a monitor alert
- Operations Manager resolution state
- Priority
- Last Modified Time
- Category
- Last Modified By
- Operations Manager severity
- Description
- Monitor service
- Alert ID
- Suppression Status
- Target Resource Type
- History of the alert

:::image type="content" source="media/view-operations-manager-alerts-azure-monitor/alert-details-inline.png" alt-text="Screenshot showing alert details." lightbox="media/view-operations-manager-alerts-azure-monitor/alert-details-expanded.png":::

## Compare Operations Manager Ops console and Azure Monitor alerts data

The alerts moving from System Center Operations Manager to Azure Monitor SCOM Managed Instance must match the Azure Monitor alert schema to be displayed properly in the portal.  

The following translations are made to the Operations Manager alert schema when moving to Azure Monitor alerts schema:

|Alert property|Representation in Operations Console|Representation in Azure Monitor|
|------|------|------|
| State | Seven predefined Alert states, which can be extended to 255 user-defined states. <br/> <br/> New, Acknowledged, Scheduled, Assigned to Engineering, Awaiting Evidence, Resolved and Closed.| Two distinct properties: Alert monitoring Condition and User state. <br/> <br/> The new alert from Operations Manager to Azure is represented as **Fired** and **New**, respectively. <br/> <br/> The closed alert from SCOM Managed Instance is represented as **Resolved** and **New** or **Resolved** and **Closed**. |
|Severity|Critical, Warning, Informational|Critical, Error, Warning, Informational and Verbose <br/> <br/> SCOM Managed Instance alerts are represented with the corresponding Alert severity in Azure.|
|Signal Type| |All SCOM Managed Instance alerts are represented as Custom signal type in Azure.|
|Monitoring Service| |**SCOM Managed Instance**|
|Effected resource| |If the alert is from Azure native/Arc resource, then it is represented with its corresponding ARM resource ID. <br/> <br/> If the alert is from on-premises workload, it is represented with SCOM Managed Instance resource ARM ID.|
|Additional properties|Priority, Category, Owner, Repeat count, alert context, and parameters. <br/> <br/> The management pack discovered object for the alert.|All these properties are represented in Azure alert context to enhance it with SCOM Managed Instance alerts information.|

## Integrate Azure Monitor alerts with ITSM tools

Azure Monitor allows integration with ITSM tools such as ServiceNow so that alerts can be forwarded to the tools in the form of incidents. Using Azure Monitor Alerts’ concept of [Action Groups](/azure/azure-monitor/alerts/action-groups) and [Alert Processing Rules](/azure/azure-monitor/alerts/alerts-processing-rules?tabs=portal), you can create the necessary actions to link alerts from SCOM Managed Instance with an ITSM connector such as ServiceNow.  

For more information, see [Connect ServiceNow with IT Service Management Connector](/azure/azure-monitor/alerts/itsmc-connections-servicenow).

Once the ITSM connector is created and connected to the ServiceNow instance, follow these steps:

1. After you create the ITSM connector, create an **Action Group** in the **Azure Monitor Alerts** page with an ITSM action type created.

      :::image type="content" source="media/view-operations-manager-alerts-azure-monitor/action-group-inline.png" alt-text="Screenshot showing Action group." lightbox="media/view-operations-manager-alerts-azure-monitor/action-group-expanded.png":::

2. Create an Alert Processing Rule with the filter **Monitor Service equals SCOM Managed Instance**.
      :::image type="content" source="media/view-operations-manager-alerts-azure-monitor/alert-processing-rule-inline.png" alt-text="Screenshot showing Alert processing rule." lightbox="media/view-operations-manager-alerts-azure-monitor/alert-processing-rule-expanded.png":::

3. In the **Rule settings** tab, under **Rule type**, select **Apply action group** option.

      :::image type="content" source="media/view-operations-manager-alerts-azure-monitor/rule-settings-inline.png" alt-text="Screenshot showing Rule settings tab." lightbox="media/view-operations-manager-alerts-azure-monitor/rule-settings-expanded.png":::

Now, the connection to the ServiceNow instance is successfully established and the alerts reflect in the portal as incidents.  

In the ServiceNow portal, you can see your list of incidents under the **Microsoft OMS Integrator – OMS Incidents** tab.

:::image type="content" source="media/view-operations-manager-alerts-azure-monitor/oms-incidents-inline.png" alt-text="Screenshot showing incidents tab." lightbox="media/view-operations-manager-alerts-azure-monitor/oms-incidents-expanded.png":::

## Status of an alert when it’s closed

Irrespective of the alert being a rule or monitor alert, if the alert is closed in the System Center Operations Manager Ops Console, the closed state of the alert will be reflected in the Azure Monitor portal with the **Alert condition** changing from **Fired** to **Resolved**.

:::image type="content" source="media/view-operations-manager-alerts-azure-monitor/summary.png" alt-text="Screenshot showing summary of Alerts.":::

>[!NOTE]
>- We don’t recommend closing a monitor alert manually in the Azure Monitor portal. For more information, see [How to Close an Alert Generated by a Monitor](/system-center/scom/manage-alert-created-by-monitor).
>- If you close a rule-based alert in the Azure Monitor portal, the change will not be reflected in the System Center Operations Manager Ops Console.