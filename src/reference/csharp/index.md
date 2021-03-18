---
uid: csharp-bot-reference
---

## C# Bot Reference Overview

Abbot skills written in C# receive a `Bot` property of type <xref:Serious.Abbot.Scripting.IBot>. `Bot` is the starting point to access everything Abbot provides. There are a couple of libraries that are not accessible via <xref:Serious.Abbot.Scripting.IBot>:

* [Noda Time](https://nodatime.org/)
* [Octokit.net](https://github.com/octokit/octokit.net)

To use these libraries, just add the respective `using` statement at the top of the skill. For example, the [`deploy` package](https://ab.bot/packages/aseriousbiz/deploy) shows an example of using Octokit.

### Parsing arguments

Abbot provides assistance in parsing arguments with a special implementation of <xref:Serious.Abbot.Scripting.IArguments>. For more details, be sure to read the guide <xref:parsing-arguments-c-sharp>.
