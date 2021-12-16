---
uid: sending-messages
---

# Sending messages

One of the most important things your skill needs to do is to communicate with users.
The easiest way to send a message is to call the `Reply` API

## [C#](#tab/tabid-cs)

```csharp
await Bot.ReplyAsync("Thanks!");
```

## [JavaScript](#tab/tabid-js)

```js
bot.reply("Thanks!");
```

## [Python](#tab/tabid-py)

```python
bot.reply("Thanks")
```

***

The reply API will, by default, send the reply back to the place in which Abbot was invoked:

1. If Abbot was invoked by a top-level message in a room, Abbot will respond with another top-level message in that room.
2. If Abbot was invoked by a message in a thread, Abbot will respond in that thread.
3. If Abbot was invoked by a trigger, Abbot will respond with a top-level message in the channel attached to that trigger.

## Sending DMs to users

You can customize the destination message using the `to` option. This option can take a User, Room, or Thread. For example, if the skill was triggered via a message, you can send a DM to the user who sent that message using code like this:

## [C#](#tab/tabid-cs)

```csharp
await Bot.ReplyAsync("Thanks!", new MessageOptions() { To = Bot.From });
```

## [JavaScript](#tab/tabid-js)

```js
bot.reply("Thanks!", { to: bot.from });
```

## [Python](#tab/tabid-py)

```python
bot.reply("Thanks", to=bot.from_user)
```

***

Abbot parses user mentions (`@username`) and exposes the result in the tokenized arguments. These values can also be used to send messages. For example:

## [C#](#tab/tabid-cs)

```csharp
var (to, rest) = Bot.Arguments;

if (to is IMentionArgument mention)
{
    await Bot.ReplyAsync($"Hello there {mention.Mentioned.Name}!", new MessageOptions() { To = mention.Mentioned });
}
```

## [JavaScript](#tab/tabid-js)

```js
let [to, ...rest] = bot.tokenizedArguments;

if (to.mentioned) {
    bot.reply(`Hello there ${to.mentioned.name}!`, { to: to.mentioned });
}
```

## [Python](#tab/tabid-py)

```python
(to, *rest) = bot.tokenized_arguments

if isinstance(to, MentionArgument):
    bot.reply(f"Hello there {to.mentioned.name}!", to=to.mentioned)
```

***

Or, you can use the "Users" API to fetch a specific user, if you know their platform-specific ID (for example, the `Unnnnnnn` ID for a Slack User).

## [C#](#tab/tabid-cs)

```csharp
var user = Bot.Users.GetConversation("U0123456789");
await Bot.ReplyAsync($"Hello!", new MessageOptions() { To = user });
```

## [JavaScript](#tab/tabid-js)

```js
let user = bot.users.getConversation("U0123456789");
bot.reply("Hello!", { to: user });
```

## [Python](#tab/tabid-py)

```python
user = bot.users.get_conversation("U0123456789")
bot.reply("Hello!", to=user)
```

***

> [!NOTE]
> The object returned by the "Get Conversation" API is just a handle that can be used with the "to" option.
> It doesn't contain any of the user's profile information (name, email, etc.)

## Sending to rooms

You can also send messages to any room that Abbot is in.

> [!NOTE]
> Abbot **must** be invited to the room before they can send any messages to it!

If you absolutely want to ensure you send a top-level message to a room, even if Abbot was invoked in a thread, you can do so by sending directly to the room in which the skill was invoked:

## [C#](#tab/tabid-cs)

```csharp
await Bot.ReplyAsync("Thanks!", new MessageOptions() { To = Bot.Room });
```

## [JavaScript](#tab/tabid-js)

```js
bot.reply("Thanks!", { to: bot.room });
```

## [Python](#tab/tabid-py)

```python
bot.reply("Thanks", to=bot.room)
```

***

Abbot parses room mentions (`#room`) and exposes the result in the tokenized arguments. These values can also be used to send messages. For example:

## [C#](#tab/tabid-cs)

```csharp
var (to, rest) = Bot.Arguments;

if (to is IRoomArgument arg)
{
    await Bot.ReplyAsync("Hello", new MessageOptions() { To = arg.Room });
}
```

## [JavaScript](#tab/tabid-js)

```js
let [to, ...rest] = bot.tokenizedArguments;

if (to.room) {
    bot.reply("Hello", { to: to.room });
}
```

## [Python](#tab/tabid-py)

```python
(to, *rest) = bot.tokenized_arguments

if isinstance(to, RoomArgument):
    bot.reply("Hello", to=to.room)
```

***

Or, you can use the "Rooms" API to fetch a specific room, if you know their platform-specific ID (for example, the `Cnnnnnnn` ID for a Slack Channel).

## [C#](#tab/tabid-cs)

```csharp
var room = Bot.Rooms.GetConversation("C0123456789");
await Bot.ReplyAsync("Hello!", new MessageOptions() { To = room });
```

## [JavaScript](#tab/tabid-js)

```js
let room = bot.rooms.getConversation("C0123456789");
bot.reply("Hello!", { to: room });
```

## [Python](#tab/tabid-py)

```python
room = bot.rooms.get_conversation("C0123456789")
bot.reply("Hello!", to=room)
```

***

> [!NOTE]
> The object returned by the "Get Conversation" API is just a handle that can be used with the "to" option.
> It doesn't contain any of the room's other metadata (name, etc.)


## Sending on Threads

By default, when sending to a user or room, Abbot sends a top-level message. The only exception to that is when Abbot was invoked _within_ a thread. However, you can also tell Abbot respond in a particular thread if you know the ID of that thread.

If you want to respond in a new thread when a user invokes your skill, even if they invoke it as a top-level message, you can use the "Thread" property on the Bot object:

## [C#](#tab/tabid-cs)

```csharp
await Bot.ReplyAsync("Hello from a thread!", new MessageOptions() { To = Bot.Thread });
```

## [JavaScript](#tab/tabid-js)

```js
bot.reply("Hello from a thread!", { to: bot.thread });
```

## [Python](#tab/tabid-py)

```python
bot.reply("Hello from a thread!", to=bot.thread)
```

***

Or, if you know the platform-specific ID for a thread, you can look it up using the "Get Thread" API on any User or Room (including values returned by the "Get Conversation" API):


## [C#](#tab/tabid-cs)

```csharp
var thread = Bot.Rooms.GetConversation("C1234").GetThread("1234.5678");
await Bot.ReplyAsync("Hello from a thread!", new MessageOptions() { To = thread });
```

## [JavaScript](#tab/tabid-js)

```js
let thread = bot.rooms.getConversation("C1234").getThread("1234.5678");
bot.reply("Hello from a thread!", { to: thread });
```

## [Python](#tab/tabid-py)

```python
thread = bot.rooms.get_conversation("C1234").get_thread("1234.5678")
bot.reply("Hello from a thread!", to=thread)
```

***

## Parsing Slack URLs

Abbot has another neat trick up their sleeve. Abbot can parse Slack URLs (the kind generated when you choose "Copy Link" when sharing a message, channel, user, etc.) and produce a handle to whatever the URL represented that you can use to send messages:

## [C#](#tab/tabid-cs)

```csharp
if (Bot.Utilities.TryParseSlackUrl("https://exampleorg.slack/team/U0000000000", out var conversation))
{
    await Bot.ReplyAsync("Hello!", new MessageOptions() { To = conversation });
}
```

## [JavaScript](#tab/tabid-js)

```js
let conversation = bot.utils.parseSlackUrl("https://exampleorg.slack/team/U0000000000");
if (conversation) {
    bot.reply("Hello!", { to: conversation });
}
```

## [Python](#tab/tabid-py)

```python
conversation = bot.utils.parse_slack_url("https://exampleorg.slack/team/U0000000000")
if conversation is not None:
    bot.reply("Hello!", to=conversation)
```

***