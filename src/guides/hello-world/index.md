---
uid: hello-world-guide
Title: Writing your first Abbot skill
---

# Writing your first Abbot skill

## Hello, World (and everyone else!)

Let's start by showing Abbot how to say hello. To do this, we'll use the `Bot` object ([full reference here](/help/reference)) to respond to the channel, as well as to determine what text was entered.

### Creating Skills

First, create a new skill ([C# skill](https://ab.bot/skills/create/csharp), [JavaScript](https://ab.bot/skills/create/javascript), [Python](https://ab.bot/skills/create/python)). The name of the skill is how users will use it in chat. Name the skill `hello`. Abbot skills must be uniquely named, so if there is already a skill named `hello` you will need to choose another name.

The `Description` and `Usage` fields help other Abbot users in chat figure out how to use your skills. Fill in some descriptive text in those fields now.

### Editing Code

Abbot populates new skills with some default code that replies back with the passed in arguments. It should look something like:

# [C#](#tab/tabid-cs)

```cs
// Change the code below with the code for your skill! 
await Bot.ReplyAsync("Hello " + Bot.Arguments);
```

# [JavaScript](#tab/tabid-js)

```javascript
module.exports = (async () => { // We modularize your code in order to run it.
  // Change the code below with the code for your skill! 
  bot.reply("Hello " + bot.arguments);
})(); // Thanks for building with Abbot!
```


# [Python](#tab/tabid-py)

```python
# Change the code below with the code for your skill! 
bot.reply("Hello " + bot.arguments)
```

***

Go ahead and save the skill so you can test it. Below the editor is an interactive shell. The prompt should say `@abbot hello` (or `@abbot whatever-you-named-the-skill`). You can test the skill by typing "world" in the interactive shell.

Abbot should respond in the interactive shell by saying "world"). Congratulations, you've written your first skill! Of course, this might not be as exciting as you were hoping for, so let's add a few features to make it a little more useful.

### Working with User-Provided Data

Most skills will need to collect some information from the user in chat. The text that the chat user entered is available in the arguments property:

* In C# the <xref:Serious.Abbot.Scripting.IBot.Arguments> property is a special collection of words. See <xref:parsing-arguments-c-sharp> for more details.
* In JavaScript, `bot.tokenizedArguments` contains the arguments as a collection of words, while `bot.arguments` is a string containing all the arguments the user typed (it does not include the skill name).
* In Python, `bot.tokenized_arguments` contais the arguments as a collection of words, while `bot.arguments` is a string containing all the arguments the user typed (it does not include the skill name).

The default code when creating a new skill demonstrates accessing user provided data.

## A more involved example (writing skills is hard, let's go shopping!)

A skill that repeats what you said doesn't seem very useful. Let's try something a little more valuable -- a shared shopping list. Instead of forcing you to set up your own database, Abbot provides an easy-to-use persistence layer called the Brain. There's a full reference of the brain in [Abbot's reference documentation](/help/reference), but let's try out some ideas here.

If you've left the edit screen, return to your `hello` skill. Rename the skill to `shoppinglist` and hit `Save` to follow along with the guide.

> [!TIP]
> When editing a skill, you can save at any time using the keyboard. `CMD+S` on a mac. `CTRL+S` on Windows.

We'll support this chat interface for our users:

```text
@Abbot shoppinglist add item // to add items to the shopping list.
@Abbot shoppinglist remove item // to remove items from the shopping list. 
@Abbot shoppinglist show // to show the items that are on the shopping list. 
```

Let's build the interface first. We need to stub out handlers for each of our use cases. We can do that by retrieving the value from the arguments. Stub out three functions (one for each task type) like this:

# [C#](#tab/tabid-cs)

```cs
Task Add(string item) {
    return Bot.ReplyAsync("Adding " + item);
}

Task Remove(string item) {
    return Bot.ReplyAsync("Removing " + item);
}

Task Show() {
    return Bot.ReplyAsync("Show the shopping list");
}
```

# [JavaScript](#tab/tabid-js)

```javascript
function add(item) {
    bot.reply("Adding " + item);
}

function remove(item) {
    bot.reply("Removing " + item);
}

function show() {
    bot.reply("Show the shopping list.");
}
```

# [Python](#tab/tabid-py)

```python
def add(item):
    bot.reply("Adding " + item)

def remove(item):
    bot.reply("Removing " + item)

def show():
    bot.reply("Show the shopping list")
```

***

Now we can parse the user's command to see which function to call. To do that, we can deconstruct the arguments into two values, the first word as the "command" and the rest of the string as the "argument". Put this in at the top of your skill before the methods you stubbed out:

# [C#](#tab/tabid-cs)

```cs
var (command, argument) = Bot.Arguments;

switch (command.Value) {
    case "add":
        Add(argument.Value);
        break;
    case "remove":
        Remove(argument.Value);
        break;
    case "show":
        Show();
        break;
    default:
        await Bot.ReplyAsync("I don't know what to do!");
        break;
}
```

When deconstructing the arguments in this way, the first value contains the first word sent by the user. The second value contains the rest of the words sent by the user. If the user only sent one word, the second argument would have a type `IMissingArgument` and a `Value` of empty string.

The `switch` statement tells the skill runner which method to run, along with the arguments provided by the user, if applicable. Finally, the skill will reply to the user "I don't know what to do!" if no arguments were provided. You can provide a more useful error message on your own, or change the default behavior to something more interesting.

# [JavaScript](#tab/tabid-js)

```javascript
const words = bot.arguments.split(" ");
const command = words.shift();
const userArguments = words.join(" ");

switch (command) {
  case "add":
    add(userArguments);
    break;
  case "remove": 
    remove(userArguments);
    break;
  case "list": 
    show();
    break;
  default:
    bot.reply("I don't know what to do!");
}
```

You can see that `words` contains an array of all of the words sent by the user, and `userArguments` contains the rest of the line. The `case` statements tell the skill runner which method to run, along with the arguments provided by the user, if applicable. Finally, the skill will reply to the user "I don't know what to do!" if no arguments were provided. You can provide a more useful error message on your own, or change the default behavior to something more interesting.

# [Python](#tab/tabid-py)

```python
words = args.split(" ")
user_arguments = " ".join(words[1::])

if words[0] == "add":
    add(user_arguments)
elif words[0] == "remove":
    remove(user_arguments)
elif words[0] == "show":
    show()
else:
    bot.reply("I don't know what to do!")
```

You can see that `words` contains an array of all of the words sent by the user, and `user_arguments` contains the rest of the line. The `if` statements tell the skill runner which method to run, along with the arguments provided by the user, if applicable. Finally, the skill will reply to the user "I don't know what to do!" if no arguments were provided. You can provide a more useful error message on your own, or change the default behavior to something more interesting.

***

We've written enough code that it may make sense to start testing if everything is wired up. In each of `Add`, `Remove`, and `Show` methods, add a line to reply with a message in each method that indicates we're in that method. Once that's done we can test our skill's API before persisting data. To test that, try some commands in the command window like:

```text
@Abbot shoppinglist add spaghetti
@Abbot shoppinglist add sauce
@Abbot shoppinglist show
@Abbot shoppinglist remove sauce
```

Assuming everything has been entered correctly, Abbot should return some of the debug statements we entered earlier back into the command window. If there are any errors, the editor screen should be able to highlight them for you. In the rare case that the skill isn't working, and the editor isn't showing any errors, take a look at the JavaScript console. Please let us know if there are any errors here by emailing help@ab.bot. We will take a look as soon as we can!

### Data Persistence

Find the method to add an item and change it like so:

# [C#](#tab/tabid-cs)

```cs
public async Task<string> Add(string item) {    
    await Bot.Brain.WriteAsync(item, item);
    return $"Added {item} to the shopping list.";    
}
```

# [JavaScript](#tab/tabid-js)

```javascript
async function add(item) {
  await bot.brain.write(item, item);
  return `Added ${item} to the shopping list.`;
}
```

# [Python](#tab/tabid-py)

```python
def add(item):
    bot.brain.write(item, item)
    return "Added {} to the shopping list.".format(item)
```

***

The first argument of `write` (or `WriteAsync` in C#) expects a key, and the second expects a value. Keys must be unique in Abbot (duplicates overwrite the previous value), so we will store the items in our list as keys. Later we can use the value for metadata, like whether or not the item was purchased, or who added the item. For now, let's just use keys. One more thing -- `write` (or `WriteAsync`) acts as an upsert operation. If there is a pre-existing item with a matching key, the method will overwrite that item.

Once the item has been added to Abbot's brain, we return a message so that we can report it back to the user later.

Next, change the method to remove an item, to look like this:

# [C#](#tab/tabid-cs)

```cs
public async Task<string> Remove(string item) {
    await Bot.Brain.DeleteAsync(item);
    return $"Removed {item} from the shopping list.";
}
```

# [JavaScript](#tab/tabid-js)

```javascript
async function remove(item) {
  await bot.brain.delete(item);
  return `Removed ${item} from the shopping list.`;
}
```

# [Python](#tab/tabid-py)

```python
def remove(item):
    bot.brain.delete(item)
    return "Removed {} from the shopping list.".format(item)
```

***

This will allow us to remove items from our list. `delete` (or `DeleteAsync` in C#) always expects a key in order to identify what data to remove. It will not raise an exception if the data is not found.

Finally, we need a way to list the shopping list. Change the method to show items to match this:

# [C#](#tab/tabid-cs)

```c#
public async Task<string> Show() {
    var response = "Here's what's on the list:";
    var items = await Bot.Brain.GetKeysAsync();
    foreach(var item in items) {
         response += $"\n* {item}";
    }
    
    return response;
}
```

# [JavaScript](#tab/tabid-js)

```javascript
async function show() {
  const list = await bot.brain.list();
  var response = "Here's what's on the list: \n";
  list.forEach(function(item) {
    response += `* ${item.key} \n`;
  })
  return response;
}
```

# [Python](#tab/tabid-py)

```python
def show():
    response = "Here's what's on the list:" 
    items = bot.brain.list()
    all_items = ["* {}".format(item) for item in items]
    response += "\n".join(all_items)
    return response
```

***

This method will call `list` (or `GetKeysAsync`) to fetch all of the keys in Abbot's brain for this skill, iterate through them, and return the result back to the caller. We format the response here, but could just as easily return a List back to the caller.

Now we can parse the user's command and call the correct method to modify the shopping list:

# [C#](#tab/tabid-cs)

```cs
var (command, argument) = Bot.Arguments;

var output = string.Empty;
switch (command.Value) {
    case "add":
        output = await Add(argument.Value);
        break;
    case "remove":
        output = await Remove(argument.Value);
        break;
    case "show":
        output = await Show();
        break;
    default:
        output = "I don't know what to do!";
        break;
}
```

# [JavaScript](#tab/tabid-js)

```javascript
const words = bot.arguments.split(" ");
const command = words.shift();
const userArguments = words.join(" ");

var output = "";

switch (command) {
  case "add":
    output = await add(userArguments);
    break;
  case "remove": 
    output = await remove(userArguments);
    break;
  case "list": 
    output = await show();
    break;
  default:
    output = "I don't know what to do!";
}
```

# [Python](#tab/tabid-py)

```python
words = args.split(" ")
user_arguments = " ".join(words[1::])
output = ""

if words[0] == "add":
    output = add(user_arguments)
elif words[0] == "remove":
    output = remove(user_arguments)
elif words[0] == "show":
    output = show()
else:
    output = "I don't know what to do!"
```

***

As you can see, we create a variable called `output` and store the output of our method there. Finally, we need to report back to the user -- every skill should return something to your user so that they know it succeeded (or failed, as they sometimes do).

To do that, we can simply call the reply method: `bot.reply(output)` in JavaScript and Python or `await Bot.ReplyAsync(output)` in C#.

And we're done! Try interacting with your skill from Abbot's console. You should be able to add and remove items, as well as view the shopping list. If you save this skill, you can interact with it in chat as well.

### Wrapping up

Here's the entire skill, if you'd like to view the whole thing:

# [C#](#tab/tabid-cs)

```cs
// A simple shopping list skill.
var (command, argument) = Bot.Arguments;

var output = string.Empty;
switch (command.Value) {
    case "add":
        output = await Add(argument.Value);
        break;
    case "remove":
        output = await Remove(argument.Value);
        break;
    case "show":
        output = await Show();
        break;
    default:
        output = "I don't know what to do!";
        break;
}

await Bot.ReplyAsync(output);

public async Task<string> Add(string item) {    
    await Bot.Brain.WriteAsync(item, item);
    return $"Added {item} to the shopping list.";    
}

public async Task<string> Remove(string item) {
    await Bot.Brain.DeleteAsync(item);
    return $"Removed {item} from the shopping list.";
}

public async Task<string> Show() {
    var response = "Here's what's on the list:";
    var items = await Bot.Brain.GetKeysAsync();
    foreach(var item in items) {
         response += $"\n* {item}";
    }
    
    return response;
}
```

# [JavaScript](#tab/tabid-js)

```javascript
// A simple shoppping list skill.

const words = bot.arguments.split(" ");
const command = words.shift();
const userArguments = words.join(" ");

var output = "";

switch (command) {
  case "add":
    output = await add(userArguments);
    break;
  case "remove": 
    output = await remove(userArguments);
    break;
  case "list": 
    output = await show();
    break;
  default:
    output = "I don't know what to do!";
}

bot.reply(output);
async function add(item) {
  await bot.brain.write(item, item);
  return `Added ${item} to the shopping list.`;
}

async function remove(item) {
  await bot.brain.delete(item);
  return `Removed ${item} from the shopping list.`;
}

async function show() {
  const list = await bot.brain.list();
  var response = "Here's what's on the list: \n";
  list.forEach(function(item) {
    response += `* ${item.key} \n`;
  })
  return response;
}
```

# [Python](#tab/tabid-py)

```python
# A simple shopping list skill. 

words = args.split(" ")
user_arguments = " ".join(words[1::])
output = ""

if words[0] == "add":
    output = add(user_arguments)
elif words[0] == "remove":
    output = remove(user_arguments)
elif words[0] == "show":
    output = show()
else:
    output = "I don't know what to do!"

bot.reply(output)

def add(item):
    bot.brain.write(item, item)
    return "Added {} to the shopping list.".format(item)
  
def remove(item):
    bot.brain.delete(item)
    return "Removed {} from the shopping list.".format(item)

def show():
    response = "Here's what's on the list: \n" 
    items = bot.brain.list()
    all_items = ["* {}".format(item) for item in items]
    response += "\n".join(all_items)
    return response
```

***

## Next Steps

For a challenge, you could modify this skill to check if data already exists before removing it, or add a quantity field in `Value`. There are many things you could try! We didn't get an opportunity to work with `Secret` values in this guide, but if we needed to interact with an external API, they would have been useful here. For now, we'll leave those as a topic for another guide.
