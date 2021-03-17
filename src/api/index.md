# API References

Every skill receives a Bot object with a set of properties and methods. The set of properties and methods may differ slightly based on the language of the skill. The following are the languages Abbot supports and the reference docs for the Bot object.

* [C# Bot Reference](../api-csharp/Serious.Abbot.Scripting.html)
* Python Bot Reference _coming soon!_
* JavaScript Bot Reference _coming soon!_

## C# Bot Reference

Abbot skills written in C# receive a `Bot` property of type <xref:Serious.Abbot.Scripting.IBot>. From there the skill author can access everything Abbot provides. There are a couple of libraries that are not accessible via <xref:Serious.Abbot.Scripting.IBot>:

* [Noda Time](https://nodatime.org/)
* [Octokit.net](https://github.com/octokit/octokit.net)

To use these libraries, just add the respective `using` statement at the top of the skill. For example, the [`deploy` package](https://ab.bot/packages/aseriousbiz/deploy) shows an example of using Octokit.
