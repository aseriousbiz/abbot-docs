---
uid: ui-elements
---

# User Interface Elements

Abbot supports a limited set of UI elements that can be leveraged in a skill. Discord doesn't support UI elements yet, so these elements only work in Slack and Teams. We may have a workaround for that in the future, but for now, this is the way it is.

Over time, Abbot will add support for more elements.

## Buttons

Buttons are a simple way to present the user a set of choices. They can be useful in workflows driven by chat such as an approval workflow. For example, suppose a skill needs to find out the user's favorite color. The following code will reply with some text and a set of buttons.

# [C#](#tab/tabid-cs)

```cs
var buttons = new[] {
    new Button("Red"),
    new Button("Blue"),
    new Button("Green")
};
await Bot.ReplyWithButtonsAsync("Please choose a color.", buttons);
```

# [JavaScript](#tab/tabid-js)

```javascript
var buttons = new[] {
    new Button("Red"),
    new Button("Blue"),
    new Button("Green")
};
bot.replyWithButtons("Please choose a color.", buttons);
```

# [Python](#tab/tabid-py)

```python
buttons = [Button("Red"), Button("Blue"), Button("Green")]
bot.reply_with_buttons("Please choose a color", buttons)
```

***

Here's what it looks like in Slack.

<img width="373" alt="Slack message with three buttons" src="https://user-images.githubusercontent.com/19977/121222637-94ccaf80-c83b-11eb-8ecf-3505362f3bbd.png">

So far, so good. But presenting a set of buttons isn't all that useful by itself. It's important to be able to respond to button clicks.

When the user clicks a button, the skill is called with a set of predefined arguments. The predefined arguments are specified by the `value` of the clicked button. For example, if a button has a value `property 123`, then when someone clicks that button, the skill is called with two arguments, `property` and `123`.

But what if somebody calls the skill from chat with the arguments `property 123`? Is there a way to distinguish that from the user clicking the button with the value `property 123`?

Yes there is. To distinguish between the user typing arguments vs clicking a button, there's a `Bot.IsInteraction` (C#), `bot.is_interaction` (Python), `bot.isInteraction` (JavaScript) property. This property indicates whether or not the incoming message is the result of clicking a button. Let's put these together to see how it works.

# [C#](#tab/tabid-cs)

```cs
if (!Bot.IsInteraction) {
    var buttons = new [] {
        new Button("Red"),
        new Button("Blue"),
        new Button("Green")
    };
    await Bot.ReplyWithButtonsAsync("Please choose a color.", buttons);
}
else {
    // Bot.IsInteraction is true
    await Bot.ReplyAsync($"You chose {Bot.Arguments[0]}");
}
```

# [JavaScript](#tab/tabid-js)

```javascript
if (!bot.isInteraction) {
    var buttons = [
        new Button("Red"),
        new Button("Blue"),
        new Button("Green")
    ];
    bot.replyWithButtons("Please choose a color.", buttons);
}
else {
    // bot.isInteraction is true
    bot.reply(`You chose ${bot.tokenizedArguments[0]}`);
}
```


# [Python](#tab/tabid-py)

```python
if not bot.is_interaction:
    buttons = [Button("Red"), Button("Blue"), Button("Green")]
    bot.reply_with_buttons("Please choose a color", buttons)
else:
    # bot.is_interaction is True
    bot.reply("You chose {}".format(bot.tokenized_arguments[0]))
```

***

Now when you run this skill, and then click on the button, say the Blue one, you'll receive a response that says "You chose Blue".

<img width="330" alt="Image showing three buttons and the result of clicking on Blue" src="https://user-images.githubusercontent.com/19977/121223380-52f03900-c83c-11eb-99fb-3e627b1bfb2a.png">


The value of a button is passed back to the skill as the arguments. The arguments for a button can be different from the label displayed to the user. And buttons can be given one of three styles, default, primary, or danger. However, these styles only work for Slack as Teams doesn't support them and just ignores these styles.

# [C#](#tab/tabid-cs)

```cs
if (!Bot.IsInteraction) {
    var buttons = new[] {
        new Button("Red", "pick red", ButtonStyle.Primary),
        new Button("Blue", "pick blue"),
        new Button("Green", "pick green")
    };
    await Bot.ReplyWithButtonsAsync("Please choose a color.", buttons);
}
else {
    // Bot.IsInteraction is true
    await Bot.ReplyAsync($"You chose {Bot.Arguments[1]}");
}
```

# [JavaScript](#tab/tabid-js)

```javascript
if (!bot.isInteraction) {
    var buttons = [
        new Button("Red", "pick red", 'primary'),
        new Button("Blue", "pick blue"),
        new Button("Green", "pick green")
    ];
    bot.replyWithButtons("Please choose a color.", buttons);
}
else {
    // bot.isInteraction is true
    bot.reply(`You chose ${bot.tokenizedArguments[1]}`);
}
```


# [Python](#tab/tabid-py)

```python
if not bot.is_interaction:
    buttons = [
      Button("Red", "pick red", "primary"),
      Button("Blue", "pick blue"),
      Button("Green", "pick green")
    ]
    bot.reply_with_buttons("Please choose a color", buttons)
else:
    # bot.is_interaction is True
    bot.reply("You chose {}".format(bot.tokenized_arguments[1]))
```

***

Note that in this example, because the value has two words, we need to grab the second tokenized argument.

<img width="468" alt="Buttons with a bit more style" src="https://user-images.githubusercontent.com/19977/121223658-9d71b580-c83c-11eb-8b03-06ce8d2c5666.png">

## Hero Cards

Sometimes you want to present the buttons with a bit more style. Abbot also supports presenting a set of buttons along with an image and title.

# [C#](#tab/tabid-cs)

```cs
if (!Bot.IsInteraction) {
    var buttons = new[] {
        new Button("Red", "pick red", ButtonStyle.Primary),
        new Button("Blue", "pick blue"),
        new Button("Green", "pick green")
    };
    await Bot.ReplyWithButtonsAsync(
        "Please choose a color.",
        buttons,
        "Pick one",
        new Uri("https://ab.bot/img/abbot-full-wave.png"),
        "The big choice",
        "#660000");
}
else {
    // Bot.IsInteraction is true
    await Bot.ReplyAsync($"You chose {Bot.Arguments[1]}");
}
```

# [JavaScript](#tab/tabid-js)

```javascript
if (!bot.isInteraction) {
    var buttons = [
        new Button("Red", "pick red", 'primary'),
        new Button("Blue", "pick blue"),
        new Button("Green", "pick green")
    ];
    bot.replyWithButtons("Please choose a color.",
        buttons,
        "Pick one",
        "https://ab.bot/img/abbot-full-wave.png",
        "The big choice",
        "#660000");
}
else {
    // bot.isInteraction is true
    bot.reply(`You chose ${bot.tokenizedArguments[1]}`);
}
```


# [Python](#tab/tabid-py)

```python
if not bot.is_interaction:
    buttons = [
      Button("Red", "pick red", "primary"),
      Button("Blue", "pick blue"),
      Button("Green", "pick green")
    ]
    bot.reply_with_buttons("Please choose a color",
        buttons,
        "Pick one",
        "https://ab.bot/img/abbot-full-wave.png",
        "The big choice",
        "#660000")
else:
    # bot.is_interaction is True
    bot.reply("You chose {}".format(bot.tokenized_arguments[1]))
```

Note that the color parameter (`#660000`) defines the color of the left side border and only applies to Slack. It's ignored by Teams.

<img width="438" alt="Image of chat message with an image and three buttons" src="https://user-images.githubusercontent.com/19977/121223831-c72adc80-c83c-11eb-8f13-a9ebd5993526.png">
