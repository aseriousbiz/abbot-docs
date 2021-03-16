---
Title: Hello World C#
---

# Writing your first Abbot skill

## Hello, World (and everyone else!)

_This guide is written for the C# skill environment. Click here for the [Python guide](hello-world-python.md), or here for the [JavaScript guide](hello-world-javascript.md)._

Let's start by showing Abbot how to say hello. To do this, we'll use the `Bot` object ([full reference here](/help/reference)) to respond to the channel, as well as to determine what text was entered.

### Creating Skills

First, create a [new C# skill](/skills/create/csharp). The name of the skill is how users will use it in chat. Name the skill `hello`. Abbot skills must be uniquely named, so if there is already a skill named `hello` you will need to choose another name.

The `Description` and `Usage` fields help other Abbot users in chat figure out how to use your skills. Fill in some descriptive text in those fields now. 

### Editing Code

In the Code editor, type `await Bot.ReplyAsync("hello");`, and save the skill. You can test this out by typing anything in the interactive shell below the editor and hitting enter (It should say `@Abbot hello`, and allow you to type some text in).

Abbot should respond in the interactive shell by saying "hello"). Congratulations, you've written your first skill! Of course, this might not be as exciting as you were hoping for, so let's add a few features to make it a little more useful.

### Working with User-Provided Data

Most skills will need to collect some information from the user in chat. The text that the chat user entered is available in `Bot.Arguments` as a collection of words. Let's use it now by changing our code to say: `await Bot.ReplyAsync("hello" + Bot.Arguments.Value);` to view all the arguments supplied by the user. You can test your code at any time using the interactive editor. We recommend saving regularly, but you don't have to do it while you're editing.

## A more involved example (writing skills is hard, let's go shopping!)

A skill that repeats what you said doesn't seem very useful. Let's try something a little more valuable -- a shared shopping list. Instead of forcing you to set up your own database, Abbot provides an easy-to-use persistence layer called the Brain. There's a full reference of the brain in [Abbot's reference documentation](/help/reference), but let's try out some ideas here.

If you've left the edit screen, return to your `hello` skill. Rename the skill to `shoppinglist` to follow along with the guide.

We'll support this chat interface for our users:

```
@Abbot shoppinglist add item // to add items to the shopping list.
@Abbot shoppinglist remove item // to remove items from the shopping list. 
@Abbot shoppinglist show // to show the items that are on the shopping list. 
```

Let's build the interface first. We need to stub out handlers for each of our use cases. We can do that by retrieving the value in `Bot.Arguments`. Stub out three functions (one for each task type) like this:

```c#
void Add(string item) {
    await Bot.ReplyAsync("Adding " + item);
}

void Remove(string item) {
    await Bot.ReplyAsync("Removing " + item);
}

void Show() {
    await Bot.ReplyAsync("Show the shopping list");
}
```

Now we can parse the user's command to see which function to call. To do that, we can deconstruct `Bot.Arguments` into two values, the first word as the "command" and the rest of the string as the "argument". Put this in your skill, after the `Show()` method:

```c#
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

We've written enough code that it may make sense to start testing if everything is wired up. In each of `Add`, `Remove`, and `Show` methods, add a line like `await Bot.ReplyAsync("In the Add method with these arguments: " + item);`. Once that's done we can test our skill's API before persisting data. To test that, try some commands in the command window like:

```
@Abbot shoppinglist add spaghetti
@Abbot shoppinglist add sauce
@Abbot shoppinglist show
@Abbot shoppinglist remove sauce
```

Assuming everything has been entered correctly, Abbot should return some of the debug statements we entered earlier back into the command window. If there are any errors, the editor screen should be able to highlight them for you. In the rare case that the skill isn't working, and the editor isn't showing any errors, take a look at the JavaScript console. Please let us know if there are any errors here by emailing help@aseriousbusiness.com. We will take a look as soon as we can!

### Data Persistence

Find the `Add` method, and change it to this method:

```c#
public async Task<string> Add(string item) {    
    await Bot.Brain.WriteAsync(item, item);
    return $"Added {item} to the shopping list.";    
}
```

The first argument of `WriteAsync` expects a key, and the second expects a value. Keys must be unique in Abbot (duplicates overwrite the previous value), so we will store the items in our list as keys. Later we can use the value for metadata, like whether or not the item was purchased, or who added the item. For now, let's just use keys. One more thing -- `WriteAsync` acts as an upsert operation. If there is a pre-existing item with a matching key, `WriteAsync` will overwrite that item.

Once the item has been added to Abbot's brain, we return a message so that we can report it back to the user later.

Next, change the `Remove` method, to look like this:

```c#
public async Task<string> Remove(string item) {
    await Bot.Brain.DeleteAsync(item);
    return $"Removed {item} from the shopping list.";
}
```

This will allow us to remove items from our list. `DeleteAsync` always expects a key in order to identify what data to remove. It will not raise an exception if the data is not found.

Finally, we need a way to list the shopping list. Change the `Show` method to match this: 

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

This method will call `GetKeysAsync` to fetch all of the keys in Abbot's brain for this skill, iterate through them, and return the result back to the caller. We format the response here, but could just as easily return a List back to the caller.

Now we can parse the user's command and call the correct method to modify the shopping list: 

```c#
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

As you can see, we create a variable called `output` and store the output of our method there. Finally, we need to report back to the user -- every skill should return something to your user so that they know it succeeded (or failed, as they sometimes do). 

To do that, we can simply call:

`await Bot.ReplyAsync(output)`

And we're done! Try interacting with your skill from Abbot's console. You should be able to add and remove items, as well as view the shopping list. If you save this skill, you can interact with it in chat as well.

### Wrapping up

Here's the entire skill, if you'd like to view the whole thing:

```c#
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

await Bot.ReplyAsync(output);
```

## Next Steps

For a challenge, you could modify this skill to check if data already exists before removing it, or add a quantity field in `Value`. There are many things you could try! We didn't get an opportunity to work with `Secret` values in this guide, but if we needed to interact with an external API, they would have been useful here. For now, we'll leave those as a topic for another guide. 
