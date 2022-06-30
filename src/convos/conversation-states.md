---
uid: conversation-states
---

# Conversation States

Throughout the life of the conversation, it can be in various states.
These are represented by which tab of the [Conversation List](https://app.ab.bot/conversations) they appear under.

## Needs Attention / Responded

A conversation that "Needs Attention" is one that likely needs someone from the customer success team to respond.
Newly-created conversations begin in this state.
Once someone from your organization responds, the conversation will move to the "Responded" state until someone from the customer organization responds.
The conversation moves back and forth between these states until the issues are resolved and someone from your team closes the conversation.

## Closed

A conversation by someone from your team at any time by opening the conversation [Timeline](xref:conversation-timeline) in the Abbot Dashboard and selecting "Close Conversation":

<img width="1139" alt="image" src="https://user-images.githubusercontent.com/7574/176789612-1c877040-e2b2-4e9d-b4dd-55470ea03131.png">

Once the conversation is closed, [response time notifications](xref:response-times) will no longer be triggered for it.
However, if anyone responds again on the thread, Abbot will wake it up and bring it back in to the "Needs Attention" or "Responded" state (depending on if the response comes from your team or your customer).

## Archived

If you don't expect the customer to respond any further (perhaps you've waited a few days to make sure they don't have any follow-ups), you can Archive a closed conversation by opening the conversation [Timeline](xref:conversation-timeline) in the Abbot Dashboard and selecting "Archive Conversation":

<img width="1138" alt="image" src="https://user-images.githubusercontent.com/7574/176789851-4ff4f825-a43b-4ec2-83ab-f0c87a27973d.png">

Abbot will mostly ignore an Archived conversation.
If anyone posts a new message into the conversation, Abbot will remember that message and add it to the [Timeline](xref:conversation-timeline), but Abbot won't trigger notifications or change the state of the conversation.

On the [Timeline](xref:conversation-timeline) you can also unarchive a previously-archived conversation.
This returns it to the [Closed](#closed) state.