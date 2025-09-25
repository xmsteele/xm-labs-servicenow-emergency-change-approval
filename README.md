# ServiceNow Emergency Change Approval

The ServiceNow Emergency Change Approval workflow alerts members of your Change Approval group via xMatters. Notifications are delivered via voice, SMS, mobile app, email, and other xMatters devices to ensure approvals don’t delay resolution of critical incidents. By leveraging xMatters with your existing ServiceNow Emergency Change Approval process, you can eliminate manual callouts and reduce the time it takes to approve or decline emergency change requests.

<kbd>
  <a href="https://support.xmatters.com/hc/en-us/community/topics">
    <img alt="xMatters Labs disclaimer" src="https://raw.githubusercontent.com/xmatters/xMatters-Labs/master/media/disclaimer.png">
  </a>
</kbd>

## Pre-requisites
- ServiceNow
- Everbridge Flow Designer app ([ServiceNow Store](https://store.servicenow.com/store/app/4f5cfd441b172e50c43e65b2604bcbad))
- ServiceNow xMatters workflow (Flow Designer)
- xMatters account — if you don't have one, [get one](https://www.xmatters.com)

## Files
- Emergency Change Approval xMatters workflow: [ServiceNowEmergencyChangeApproval.zip](./ServiceNowEmergencyChangeApproval.zip)

## Installation

### Import the workflow
1. Log in to xMatters.
2. Click Workflows from the left-hand navigation menu.
3. Import the ServiceNow Emergency Change Approval workflow from this repo.

### Configure the ServiceNow endpoint
1. Edit the ServiceNow Emergency Change Approval workflow you just imported.
2. Go to the Flow Designer tab.
3. Click Components → Endpoints.
4. Click ServiceNow.
5. Configure the ServiceNow endpoint (see [Flow Designer Endpoints](https://help.xmatters.com/ondemand/flowdesigner/components.htm?cshid=FlowEndpoints#Endpoints)).

### Create ServiceNow Record Alerts trigger profile (Everbridge Flow Designer)
Follow the documentation and complete the configuration as described below: [ServiceNow Record Alerts Trigger](https://help.xmatters.com/ondemand/flowdesigner/servicenow-record-alerts.htm?cshid=SNOWRecordAlertsTrigger)

To create a trigger profile:
1. In ServiceNow, go to Everbridge Flow Designer → Global Settings → Trigger Profiles.
2. Click New (upper-right corner).
3. Fill in the following fields:
   - **Name**: e.g., "Change Approval" — a descriptive name for this trigger profile.
   - **Credentials**: Select the xMatters user credentials configured for this integration.
     - If using URL auth, leave this field blank.
   - **Workflow**: e.g., "ServiceNow (Flow Designer) v2". After selecting credentials, the list populates with available Flow Designer workflows.
     - If using URL auth, leave the "-- Enter trigger URL manually --" option selected.
   - **Trigger**: e.g., "ServiceNow Records Alerts Approval [sysapproval_approver]". After selecting a workflow, choose the trigger used to initiate the flow.
   - **Trigger URL**: Automatically filled once a trigger is selected.
     - If using URL auth, enter the trigger URL manually.
   - **Default Alert Priority**: Select a value to send to Flow Designer (the integration may override based on incident priority).
   - **Default Signal Mode**: Optional free text value sent to Flow Designer; the integration may override based on create/update behavior.
   - **Additional Recipients**: e.g., "CAB Approval" — groups or users from xMatters or Everbridge to notify in addition to the Assignment group/Assigned to.
   - **ServiceNow API User**: Select the ServiceNow user that Flow Designer uses to send updates in ServiceNow.

### Optional: add Emergency Change Approval to an existing ServiceNow workflow
If you want the Emergency Change Approval workflow inside your existing ServiceNow workflow:
1. Open your existing ServiceNow workflow.
2. Go to the Flow Designer tab.
3. Create a new Flow Canvas.
4. Name the canvas “Emergency Change Approval”.
5. Click Create.
6. From the Trigger tab, search for “ServiceNow Record Alerts”.
7. Drag “ServiceNow Record Alerts” onto the canvas.
8. Double-click the “ServiceNow Record Alerts” trigger step now on the canvas.
9. Select the “ServiceNow” endpoint.
10. Select the Approvals `[sysapproval_approver]` table.
11. Select the following output mapping sources:
   - Approver
   - Approval for
   - Approval source
   - Due date
   - State
12. From the Flow Designer TOOLS tab, search for “Create Alert”.
13. Drag the “Create Alert” step onto the canvas and attach it to the “ServiceNow Record Alerts Approval `[sysapproval_approver]`” trigger step you created.
14. Configure the “Create Alert” step as desired. You can use the imported ServiceNow Emergency Change Approval workflow as a reference.

## How it works
At a high level, ServiceNow approval records trigger xMatters to notify your Change Approval group. Responders can approve or decline via their chosen device, and responses are synchronized back to ServiceNow.

## Testing
- Create a test approval record in ServiceNow that targets the configured Change Approval group.
- Verify xMatters creates and notifies an event.
- Respond to the notification and confirm the state is updated in ServiceNow.

## Troubleshooting
- Verify the ServiceNow endpoint credentials and URL in xMatters.
- Check the xMatters Flow Designer Activity Stream for errors.
- Ensure the Everbridge/xMatters app in ServiceNow is installed and configured. 
