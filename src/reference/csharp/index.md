---
uid: csharp-bot-reference
---

# C# Bot Reference Overview

Abbot skills written in C# receive a `Bot` property of type <xref:Serious.Abbot.Scripting.IBot>. `Bot` is the starting point to access everything Abbot provides.

## Parsing arguments

Abbot provides assistance in parsing arguments with a special implementation of <xref:Serious.Abbot.Scripting.IArguments>. For more details, be sure to read the guide <xref:parsing-arguments-c-sharp>.

## Managing Data

* Abbot includes a simple persistence layer that makes it easy for your skills to store and retrieve data. You can access Abbot's brain with [`Bot.Brain`](xref:Serious.Abbot.Scripting.IBot.Brain). The methods that are included in [`Bot.Brain`](xref:Serious.Abbot.Scripting.IBrain) are:
* [`WriteAsync(Key, Value)`](xref:Serious.Abbot.Scripting.IBrain.WriteAsync(System.String,System.Object)): Save `Value` with a key of `Key`.
* [`GetAsync(Key)`](xref:Serious.Abbot.Scripting.IBrain.GetAsync(System.String)): Get the value stored with key `Key`.
* [`GetKeysAsync(Key?)`](xref:Serious.Abbot.Scripting.IBrain.GetKeysAsync(System.String)): Get all keys that match `Key`. `Key` can be empty and will return all keys.
  * note: This is not currently implemented in Python or JavaScript.
* [`GetAllAsync(Key?)`](xref:Serious.Abbot.Scripting.IBrain.GetAllAsync(System.String)): Get all records where keys match `Key`. This supports fuzzy matching, so partial matches will be returned. `Key` can be empty and will return all keys and values.
  * note: This is not currently implemented in Python or JavaScript.
* [`DeleteAsync(Key)`](xref:Serious.Abbot.Scripting.IBrain.DeleteAsync(System.String)): Delete the value stored with key `Key`.

## Managing Secrets

Secrets are a special kind of data, and can be used to store things like authentication tokens or other configuration items that you prefer to exclude from your skill. Secrets can only be set from https://ab.bot, and are specific to a single skill. Since developers can read data from your secrets, be careful about the data that you store there -- passwords should never be stored in a Secret, for example. [`Secrets`](xref:Serious.Abbot.Scripting.ISecrets) can be read using a similar interface to Abbot's brain:

* [`GetAsync(Key)`](Serious.Abbot.Scripting.ISecrets.GetAsync(System.String)): Get the Secret with the key of `Key`

## The Mentions Collection

[`Bot.Mentions`](xref:Serious.Abbot.Scripting.IBot.Mentions) contains a list of all mentions that were found in the user's text. The `ToString()` method on each mention will return an appropriately formatted username mention to the chat system (for example, `<@U92394113>` in Slack). Skill developers can also use any of the other fields available in the Mention object in their skills.

### The Mention Object

The [`Mention`](xref:Serious.Abbot.Scripting.IChatUser) object has these fields:

* [`Id`](xref:Serious.Abbot.Scripting.IPlatformUser.Id): The id of the person or bot that was mentioned. This id is unique to the chat platform that was being used, and is not an Abbot user id.
* [`UserName`](xref:Serious.Abbot.Scripting.IPlatformUser.UserName): The user name of the person or bot that was mentioned. This name is determined by the chat platform, and is not an Abbot user name.
* [`Name`](xref:Serious.Abbot.Scripting.IPlatformUser.Name): The display name of the person or bot that was mentioned. This is set by the user in the chat platform and may change over time.
If you are writing skills that rely on the Mention object, the `Id` is the only reliable field to use in keys and for comparison.

## Preloaded C# libraries

* All libraries available in .NET Core 3.1
* [NodaTime 3.0.3](https://nodatime.org/)
* [Octokit.net](https://github.com/octokit/octokit.net)
* NewtonSoft.Json 12.0.3
* HtmlAgilityPack 1.11.28

To use these libraries, just add the respective `using` statement at the top of the skill. For example, the [`deploy` package](https://ab.bot/packages/aseriousbiz/deploy) shows an example of using Octokit.
