# Playbooks

Playbooks are a powerful workflow and automation tool for managing your relationships with customers. Use Playbooks to automate customer onboarding steps, set up trial rooms in Slack, and more.

A Playbook consists of one or more triggers used to start the Playbook, and a series of Action steps that are executed when the Playbook is triggered. Each step can be configured to perform a specific action, such as creating a new Slack channel, sending a message to a channel, or even inviting a customer to a shared Slack Connect channel.

## Triggers

Triggers are the events that start a Playbook. For example, a Playbook can be triggered when a reaction is added to a message, a customer is created, or via a webhook.

The following triggers are currently supported:

* **Abbot Added to Channel** - Runs when Abbot joins a channel.
* **Conversation Overdue** - A conversation is overdue for a reply.
* **Conversation Started** - A conversation was started by a customer.
* **New Customer Created** - A new customer record is created in Abbot.
* **Run Playbook Button** - Add a Run Playbook button to the Playbook list page to run this Playbook manually.
* **Reaction Added** - Runs when a reaction is added to a message.
* **Schedule** - Runs on the specified schedule.
* **Negative Sentiment** - Runs when AI classifies a conversation as having negative sentiment.
* **Ticket Linked** - Runs when a ticket (Zendesk, HubSpot, etc.) is linked to a conversation.
* **Ticket Status Changed** - Runs when a linked ticket status changes.
* **Webhook** - Runs when a POST or PUT request is sent to the Playbook's webhook URL.

## Actions

Actions are the steps that are executed when a Playbook is triggered. Actions are executed in order.

* **Notify Responders** - Notifies the first responders for a channel in its Hub or in a DM.
* **Create Channel for Customer** - Creates a channel for a customer using the customer name and chosen prefix.
* **Invite to Slack Connect** - Creates a Slack Connect channel and invites a user to it.
* **Send Slack Poll** - Sends a Slack poll to a channel.
* **Request Approval in Slack** - Requests an approval from one of any number of approvers in Slack.
* **Send Message** - Posts a message in a channel.
* **Continue If** -  Runs the next step of the Playbook if a certain condition is met.
* **Create Customer** - Creates a Customer in Abbot.
* **If** - Branches the Playbook based on a condition.
* **Run Skill** - Runs a Skill in a channel.
* **Wait** - Pauses the Playbook for the specified duration.