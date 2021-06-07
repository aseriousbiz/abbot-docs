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
module.exports = (async () => { // We modularize your code in order to run it.
    var buttons = new[] {
        new Button("Red"),
        new Button("Blue"),
        new Button("Green")
    };
    bot.replyWithButtons("Please choose a color.", buttons);
})(); // Thanks for building with Abbot!
```

# [Python](#tab/tabid-py)

```python
buttons = [Button("Red"), Button("Blue"), Button("Green")]
bot.reply_with_buttons("Please choose a color", buttons)
```

***

When you run this skill, you should see a set of three buttons. So far, so good. But presenting a set of buttons isn't all that useful by itself. It's important to be able to respond to button clicks.

When the user clicks a button, the same skill is called with the value of the button. There's also a new `Bot.IsInteraction` (C#), `bot.is_interaction` (Python), `bot.isInteraction` (JavaScript) property that indicates whether or not the incoming message is the result of clicking on a button. Let's put these together.

# [C#](#tab/tabid-cs)

```cs
if (!Bot.IsInteraction) {
    var buttons = [
        new Button("Red"),
        new Button("Blue"),
        new Button("Green")
    ];
    await Bot.ReplyWithButtonsAsync("Please choose a color.", buttons);
}
else {
    // Bot.IsInteraction is true
    await Bot.ReplyAsync($"You chose {Bot.Arguments[0]}");
}
```

# [JavaScript](#tab/tabid-js)

```javascript
module.exports = (async () => { // We modularize your code in order to run it.
    if (!bot.isInteraction) {
        var buttons = [
            new Button("Red"),
            new Button("Blue"),
            new Button("Green")
        ];
        bot.replyWithButtons("Please choose a color.", buttons);
    }
    else {
        // Bot.IsInteraction is true
        bot.reply(`You chose ${bot.tokenizedArguments[0]}`);
    }
})(); // Thanks for building with Abbot!
```


# [Python](#tab/tabid-py)

```python
if not bot.is_interaction:
    buttons = [Button("Red"), Button("Blue"), Button("Green")]
    bot.reply_with_buttons("Please choose a color", buttons)
else:
    # bot.is_interaction false
    bot.reply("You chose {}".format(bot.tokenized_arguments[0]))
```

***

Now when you run this skill, and then click on the button, say the Blue one, you'll receive a response that says "You chose Blue".

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
module.exports = (async () => { // We modularize your code in order to run it.
    if (!bot.isInteraction) {
        var buttons = [
            new Button("Red", "pick red", 'primary'),
            new Button("Blue", "pick blue"),
            new Button("Green", "pick green")
        ];
        bot.replyWithButtons("Please choose a color.", buttons);
    }
    else {
        // Bot.IsInteraction is true
        bot.reply(`You chose ${bot.tokenizedArguments[1]}`);
    }
})(); // Thanks for building with Abbot!
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
    # bot.is_interaction false
    bot.reply("You chose {}".format(bot.tokenized_arguments[1]))
```

***

Note that in this example, because the value has two words, I needed to grab the second tokenized argument.

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
module.exports = (async () => { // We modularize your code in order to run it.
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
        // Bot.IsInteraction is true
        bot.reply(`You chose ${bot.tokenizedArguments[1]}`);
    }
})(); // Thanks for building with Abbot!
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
    # bot.is_interaction false
    bot.reply("You chose {}".format(bot.tokenized_arguments[1]))
```

Note that the final color parameter is slack only. It's ignored by Teams.
