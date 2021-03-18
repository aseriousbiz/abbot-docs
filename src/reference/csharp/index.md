---
uid: csharp-bot-reference
---

## C# Bot Reference Overview

Abbot skills written in C# receive a `Bot` property of type <xref:Serious.Abbot.Scripting.IBot>. From there the skill author can access everything Abbot provides. There are a couple of libraries that are not accessible via <xref:Serious.Abbot.Scripting.IBot>:

* [Noda Time](https://nodatime.org/)
* [Octokit.net](https://github.com/octokit/octokit.net)

To use these libraries, just add the respective `using` statement at the top of the skill. For example, the [`deploy` package](https://ab.bot/packages/aseriousbiz/deploy) shows an example of using Octokit.
