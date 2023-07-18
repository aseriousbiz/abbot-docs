---
uid: hubs
---

# Hubs

A Hub is a special channel in your Slack that Abbot controls to help manage customer conversations. Abbot shares new customer conversations in the Hub, plus an ongoing summary of these conversations. Your team can discuss and plan in threads connected to these messages, away from shared customer channels. Hubs reduce the need to visit Abbot's website, because Abbot shares all the important information in the Hub.

When Hubs are enabled, any team member can join the Hub to check on customer conversations. Responders will get a mention in a thread when notifications are sent. **When Hubs are enabled, Abbot will no longer send Direct Messages to responders.**

Also, conversations can be snoozed or closed directly from the Hub by adding the :eyes: or :white_check_mark: emojis.

### In short
You can get in-chat help for Hubs. Type @abbot help hubs or .help hubs in a room with Abbot for a short guide on Hubs.

## Setting up a Hub
If you started using Abbot recently, we made a default Hub for you when you added Abbot to your Slack. We usually name this channel `#hub-customers`, although your administrator may have renamed it. If you're not sure if you have a Hub, you can see the list of Hubs for your workspace at https://app.ab.bot/settings/rooms?tab=Hubs.

If you don't have a Hub yet, you need to make a new channel in Slack to set one up. Invite Abbot to the channel once it's been created. Then type `@abbot hubs create` in the channel. This turns the channel into a Hub. You can make as many Hubs as you want, but for now, let's make this one the default. Do this by typing `@abbot hubs default set` in the Hub channel.

Once this is done, all monitored channels in your workspace will send their notifications here.

The summaries and tags that you may have seen in the Conversations List at https://app.ab.bot will show up here once Abbot processes them. Abbot will keep updating the main message with the latest summary, tags, and suggested next actions.

## Using Multiple Hubs
If your team has many customer channels, one Hub might be too crowded. You can easily make more Hubs. Make a new room and invite Abbot to it. Then type `@abbot hubs create` to change the room into a Hub. You can link specific rooms to the Hub by typing `@abbot hubs attach #room` to connect rooms to the new Hub. For instance, if you want to connect a room called `#team-valued-customer` to the new Hub, you would type `@abbot hubs attach #team-valued-customer` while in the Hub.

You can make as many Hubs as you want, but a room can only be connected to one Hub at a time.

## Hubs and Playbooks

If a Hub is configured, the "Notify Responders" step in Playbooks will send the notifications to the Hub instead of direct messaging the responders. This makes sure notifications are visible outside of direct messages.

You can also use the "Send Message" step to send messages directly to your Hub. This is useful if you want to send status updates to your Hub, or send messages based on other activities. 

## In Conclusion

We made Hubs to make it easier to work with your customers. Hubs are areas where your team can collaborate on customer questions and issues. Anyone in your company can join a Hub if the room is public. This way everyone can watch customer conversations without having to join customer channels.
