---
uid: response-times
---

# Response Times

Abbot can automatically notify your [Responders](xref:frs) when a conversation isn't getting a response quickly enough.
You can set two different thresholds for your response times:

* Target - The ideal response time. If a customer hasn't received a response in this time, we'll send a notification to the First Responders.
* Deadline - The longest a conversation can go without getting a response from your team. This could represent a contractual obligation like a Service Level Agreement (SLA). We'll send a notification to First Responders and Escalation Responders when this time has elapsed without a response to the customer.

## Configuring a Response Time

To configure a Response Time for a room, you can use the [Abbot Dashboard](https://app.ab.bot).

> [!NOTE]
> You must be an Administrator in Abbot to manage Response Times.

To set a Response Time for a room, visit the [Room Settings](https://app.ab.bot/settings/organization/rooms) page.
Here you can search for the room you want to manage and click "Settings" to manage Response Times and other room settings.

<img width="1117" alt="image" src="https://user-images.githubusercontent.com/279389/207182679-bbfc8189-0fe5-4a44-ac03-37ccfbd628af.png">

You can select a time in minutes, hours, or days. Default settings can be set in [Room Settings](https://app.ab.bot/settings/rooms).

> [!NOTE]
> The "Deadline" threshold must always be longer than the "Target" threshold.

## Notifications

Once you've configured both [First Responders](xref:frs) and a Response Time for a room, Abbot will start sending notifications.
When the "Target" time threshold elapses for a conversation, Abbot will start a group DM with all the First Responders for a room and remind them to respond to the conversation. When the "Deadline" time threshold elapses for a conversation, both First Responders and [Escalation Responders](xref:frs) are sent a notification.
The message will include a direct link to make it easier for your FRs and ERs to respond.
The Responders can also use that group DM to coordinate a response.