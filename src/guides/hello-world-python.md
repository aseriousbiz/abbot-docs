---
Title: Hello World Python
---

# Writing your first Abbot skill

## Hello, World (and everyone else!)

_This guide is written for the Python skill environment. Click here for the [JavaScript guide](hello-world-javascript.md), or here for the [C# guide](hello-world-c-sharp.md)._

Let's start by showing Abbot how to say hello. To do this, we'll use the `bot` object ([full reference here](/help/reference.md)) to respond to the channel, as well as to determine what text was entered.

### Creating Skills

First, create a [new Python skill](/skills/create/python.md). The name of the skill is how users will use it in chat. Name the skill `hello`. Abbot skills must be uniquely named, so if there is already a skill named `hello` you will need to choose another name.

The `Description` and `Usage` fields help other Abbot users in chat figure out how to use your skills. Fill in some descriptive text in those fields now. 

### Editing Code

In the Code editor, type `bot.reply("hello")`, and save the skill. You can test this out by typing anything in the interactive shell below the editor and hitting enter (It should say `@Abbot hello`, and allow you to type some text in).

Abbot should respond in the interactive shell by saying "hello"). Congratulations, you've written your first skill! Of course, this might not be as exciting as you were hoping for, so let's add a few features to make it a little more useful.

### Working with User-Provided Data

Most skills will need to collect some information from the user in chat. The text that the chat user entered is available in `args` or `bot.arguments`. Let's use it now by changing our code to say: `bot.reply("hello" + args)`. You can test your code at any time using the interactive editor. We recommend saving regularly, but you don't have to do it while you're editing.

## A more involved example (writing skills is hard, let's go shopping!)

A skill that repeats what you said doesn't seem very useful. Let's try something a little more valuable -- a shared shopping list. Instead of forcing you to set up your own database, Abbot provides an easy-to-use persistence layer called the Brain. There's a full reference of the brain in [Abbot's reference documentation](/help/reference), but let's try out some ideas here.

If you've left the edit screen, return to your `hello` skill. Rename the skill to `shoppinglist` to follow along with the guide.

We'll support this chat interface for our users:

```
@Abbot shoppinglist add item // to add items to the shopping list.
@Abbot shoppinglist remove item // to remove items from the shopping list. 
@Abbot shoppinglist show // to show the items that are on the shopping list. 
```

Let's build the interface first. We need to stub out handlers for each of our use cases. We can do that by parsing the value in `args`. Stub out three functions (one for each task type) like this:

```python
def add(item):
    bot.reply("Adding " + item)

def remove(item):
    bot.reply("Removing " + item)

def show():
    bot.reply("Show the shopping list")
```

Now we can parse the user's command to see which function to call. To do that, we will look at the first word of the string from the user, as well as getting the rest of the string for an argument. Put this in your skill, after the `show()` method: 

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

We've written enough code that it may make sense to start testing if everything is wired up. In each of `add`, `remove`, and `show` methods, add a line like `bot.reply("In the Add method with these arguments: " + item);`. Once that's done we can test our skill's API before persisting data. To test that, try some commands in the command window like:

```
@Abbot shoppinglist add spaghetti
@Abbot shoppinglist add sauce
@Abbot shoppinglist show
@Abbot shoppinglist remove sauce
```

Assuming everything has been entered correctly, Abbot should return some of the debug statements we entered earlier back into the command window. If there are any errors, the editor screen should be able to highlight them for you. In the rare case that the skill isn't working, and the editor isn't showing any errors, take a look at the JavaScript console. Please let us know if there are any errors here by emailing <a href="mailto:help@aseriousbusiness.com">help@aseriousbusiness.com</a>. We will take a look as soon as we can!

### Data Persistence

Find the `add` method, and change it to this method:

```python
def add(item):
    bot.brain.write(item, item)
    return "Added {} to the shopping list.".format(item)
```

The first argument of `write` expects a key, and the second expects a value. Keys must be unique in Abbot (duplicates overwrite the previous value), so we will store the items in our list as keys. Later we can use the value for metadata, like whether or not the item was purchased, or who added the item. For now, let's just use keys. One more thing -- `write` acts as an upsert operation. If there is a pre-existing item with a matching key, `write` will overwrite that item.

Once the item has been added to Abbot's brain, we return a message so that we can report it back to the user later.

Next, change the `remove` method, to look like this:

```python
def remove(item):
    bot.brain.delete(item)
    return "Removed {} from the shopping list.".format(item)
```

This will allow us to remove items from our list. `delete` always expects a key in order to identify what data to remove. It will not raise an exception if the data is not found.

Finally, we need a way to list the shopping list. Change the `show` method to match this: 

```python
def show():
    response = "Here's what's on the list:" 
    items = bot.brain.list()
    all_items = ["* {}".format(item) for item in items]
    response += "\n".join(all_items)
    return response
```

This method will call `list` to fetch all of the keys in Abbot's brain for this skill, iterate through them, and return the result back to the caller. We format the response here, but could just as easily return a List back to the caller.

Now we can parse the user's command and call the correct method to modify the shopping list: 

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

As you can see, we create a variable called `output` and store the output of our method there. Finally, we need to report back to the user -- every skill should return something to your user so that they know it succeeded (or failed, as they sometimes do). 

To do that, we can simply call:

`bot.reply(output)`

And we're done! Try interacting with your skill from Abbot's console. You should be able to add and remove items, as well as view the shopping list. If you save this skill, you can interact with it in chat as well.

### Wrapping up

Here's the entire skill, if you'd like to view the whole thing:

```python
// A simple shopping list skill. 

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
```

## Next Steps

For a challenge, you could modify this skill to check if data already exists before removing it, or add a quantity field in `value`. There are many things you could try! We didn't get an opportunity to work with `Secret` values in this guide, but if we needed to interact with an external API, they would have been useful here. For now, we'll leave those as a topic for another guide. 
