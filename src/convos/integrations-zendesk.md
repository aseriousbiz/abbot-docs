---
uid: integrations-zendesk
---

# Integrations

Abbot provides integrations with third-party services.

## Zendesk

> [!NOTE]
> Zendesk integration is currently in private preview. Please [contact us](mailto:help@ab.bot) if you're interested!

You can use Abbot to create a Zendesk ticket from a Conversation.
Once a Zendesk ticket is created, Abbot will keep replies in-sync between Slack and Zendesk.

### Configuring the integration

To configure the integration, you need to create a Zendesk API token.
The token must be associated with an Admin user on Zendesk.
To generate the token, follow [Zendesk's guidance on creating API tokens](https://support.zendesk.com/hc/en-us/articles/4408889192858-Generating-a-new-API-token).

Once you have a token, you can go to the [Integration Settings](https://app.ab.bot/settings/organization/integrations) and click "Configure" on the "Zendesk" integration:

<img width="1122" alt="image" src="https://user-images.githubusercontent.com/7574/176792851-48533141-6ced-4b3e-afcb-09e4ee2b26ff.png">

On the Zendesk integration page, click "Enable Integration" and then "Edit Settings" to enter your credentials:

<img width="1114" alt="image" src="https://user-images.githubusercontent.com/7574/176792987-b06b2811-e7dc-4310-b0ec-b53371416902.png">

You can test the credentials using the "Test Credentials" button.
Make sure you click "Save Credentials" when you're done!

### Creating a Zendesk ticket for a conversation

To create a Zendesk ticket from a conversation, select any message in the Slack thread for the conversation, open the "triple-dot" menu, and select "Manage Conversation"

<img width="311" alt="Screen Shot 2022-06-30 at 3 50 41 PM" src="https://user-images.githubusercontent.com/7574/176791390-3e2bbdca-aeaa-4118-a5b7-136d9b520bdf.png">

On the dialog that appears, select "Create Ticket":

<img width="533" alt="image" src="https://user-images.githubusercontent.com/7574/176793199-7e4a3bb5-a4d8-49e9-bb52-f564f539aeb1.png">

A new dialog will appear to allow you to enter a Subject and Description for the ticket.
In addition, you'll see the "Requester" that will be used on Zendesk, and the [Zendesk Organization linked to the current room](#linking-zendesk-organizations-to-rooms), if any.

<img width="535" alt="image" src="https://user-images.githubusercontent.com/7574/176793352-41be35a1-eb9a-4b57-a7cd-ebe0870a6ac9.png">

Click the "Create" button and Abbot will start creating the ticket in the background.

<img width="537" alt="image" src="https://user-images.githubusercontent.com/7574/176793527-4801f24b-b0b9-4fb5-bc20-a0a257c4987d.png">

When the process is complete, Abbot will send you a direct message with a link to the new ticket

<img width="684" alt="image" src="https://user-images.githubusercontent.com/7574/176793586-aa235271-ef40-4b42-b763-251156c1a0f7.png">

If the Slack message is a thread with replies, Abbot imports every reply in the thread as a comment on the Zendesk ticket that it creates.

### Keeping replies in sync

After creating the initial ticket, Abbot will take any replies posted in Slack and push them to the Zendesk ticket.
In addition, any Public Reply posted on the Zendesk ticket will be pushed back to the Slack thread.

By default, Zendesk closes tickets after 28 days of inactivity. A closed Zendesk ticket cannot receive comments (Zendesk will create a new ticket). Therefore, Abbot will not push Slack replies to a closed Zendesk ticket.

### Keeping Status in sync

When a Zendesk Ticket is marked as `Solved`, the linked Abbot `Conversation` will also be `Closed`. If a customer (foreign member of a shared channel or a guest account) replies to the thread in Slack, the `Conversation` *and* the linked Zendesk ticket will be reopened.

### Linking Zendesk organizations to rooms
You can configure a Zendesk organization for each room Abbot is monitoring on the [Zendesk Integration Settings](https://app.ab.bot/settings/organization/integrations/zendesk) page.
This helps ensure that the Zendesk tickets Abbot creates end up attached to the correct organization in Zendesk.
