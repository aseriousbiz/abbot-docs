---
uid: using-your-own-editor
---

# Writing Abbot Skills with Your Own Editor

While we've worked hard to make the Abbot editing experience in the browser great, it will never be as good as the editing experience in your favorite editor. To that end, we're working on a great local editing experience. This document will walk you through how to use your favorite editor to write Abbot skills using [`abbot-cli`](https://github.com/aseriousbiz/abbot-cli) (Abbot Command Line Interface).

[`abbot-cli`](https://github.com/aseriousbiz/abbot-cli) is an open source command line tool that makes it easy to to work on Abbot skills on your local machine. The tool has the following primary uses:

1. Get Abbot skill code from [Abbot](https://ab.bot/) onto your local machine.
2. Run your local changes in the Abbot skill runner.
3. Deploy your changes to [Abbot](https://ab.bot/).

## Installation

As of right now, installation of the Abbot CLI is a manual, albeit simple, process. Visit the [releases page on GitHub](https://github.com/aseriousbiz/abbot-cli/releases) to download the latest release for your platform. Once you've downloaded the release, extract the contents of the zip file into your local machine. Copy the `abbot` file somewhere on your machine that's in your `PATH`. For Mac users, I recommend `/usr/local/bin/`.

## Abbot Workspace

Perhaps the best way to learn about the tool is to walk through an example session. At any time, you can also run `abbot help` to see a list of commands.

The first thing we need to do is set up an Abbot workspace on your local machine. This is a folder where you will edit your skills. In my terminal it would look something like this:

```bash
$ mkdir my-skills && cd my-skills
```

Now I can run `abbot status`:

```bash
$ abbot status
Running abbot-cli version 0.2.2.0
The directory /Users/haacked/my-skills is not an Abbot Workspace.
Run `abbot auth` to set up the directory as an Abbot Workspace and authenticate it with your Abbot account.
```

Of course the `my-skills` directory isn't an Abbot Workspace, we haven't done anything yet! We need to run `abbot auth` to set up the local workspace and connect it to our Abbot account.

```bash
$ abbot auth

    Please visit https://ab.bot/account/apikeys to generate an authentication token. I will attempt to open your browser for you.

Type in the API Key token and hit ENTER: 
```

At this point, `abbot-cli` will launch your browser to a page with your personal Abbot API Keys. __IMPORTANT: If you have multiple ab.bot accounts, make sure you're in the correct one when generating an API key.__.

![Abbot API Keys Page](https://user-images.githubusercontent.com/19977/131372288-7adbca68-513e-4678-8c48-30c56403b068.png)

As you can see, I already have an API key. I can choose to create a new one, or regenerate the existing one. This page only allows you to grab the key when you create or regenerate it. After that, it never shows the key again.

If I happen to hit `ENTER` without typing in the key, or if I already know the key and want to set it, I can just run `abbot auth --token {MY-SECRET-API-KEY}` to set it without launching the browser.

Now I can run `abbot status` again to make sure it's all good.

```bash
$ abbot status
Running abbot-cli version 0.2.2.0
The organization A Serious Business, Inc. (T0123456) has the API disabled. This setting can be changed by an Administrator at https://ab.bot/account/admin/settings
```

Whoops! As you can see from this message, the current workspace is set up and authenticated correctly, but my Abbot organization hasn't enabled API Access for the organization. Once this access is enabled, run the `abbot status` command again and you should see:

```bash
$ abbot status
Running abbot-cli version 0.2.2.0
The directory /Users/haacked/my-skills is an authenticated Workspace.
Organization: Serious Business (Slack T0123456)
User: Phil Haack (U01234567)
```

This lets you know which Abbot account this Workspace is connected to.

## Editing a skill

Now that we have an authenticated Abbot Workspace set up, we're ready to work on a skill. The first thing we do is use `abbot get` to bring a skill's code to our local machine. We need to run this command from within the Abbot Workspace directory. In this example, I want to work on the `tz` skill (available in [the Abbot Package Directory](https://ab.bot/packages/aseriousbiz/tz)).

```bash
$ abbot get tz
Created skill directory /Users/haacked/dev/exp/my-skills/tz
Edit the code in the directory. When you are ready to deploy it, run

    abbot deploy tz

```

This creates an Abbot Skill Workspace for the `tz` skill. This is a directory named `tz` with a code file `main.csx` and some other supporting files. For C# skills, these other files make it possible for us to provide Intellisense for the skill editing experience. More on that in another post for those interested.

At this point, you can open up your editor to the Skill Workspace directory. If your editor supports Omnisharp, you'll get Intellisense. In my case, I'll open up VS Code to the `tz` directory.

```bash
$ code tz
```

![Screen shot of VS Code showing Intellisense for Bot](https://user-images.githubusercontent.com/19977/131401537-533115bd-545f-4cf6-8b38-14000258e9e1.png)

As you can see from the screenshot, you have full Intellisense for the `Bot` instance, even though that's an instance injected by the Abbot Runtime. In order to make that work, we had to engage in some dark magic. You may notice the first line of the skill in the editor has a special line we ask you not to touch.

We're working with the Omnisharp team for a better long-term solution.

C# is our first Intellisense-supported language, we're working on adding it to JavaScript and Python as well.

## Testing a skill

Now that you can edit the skill in your editor of choice, you're probably going to want to test those local changes before deploying them. You can do that using `abbot run`. For this example, assume we create a new C# skill named `test` with the default implementation:

```bash
$ abbot run test "one two three"
Hello one two three
```

Note that the set of arguments to the skill need to be quoted when called this way. In the example above, three arguments ("one", "two", and "three") are passed to the skill `test`. If you need to pass an argument that itself has spaces, escape the quotes. So `abbot run test "\"one two\" three"` would pass two arguments to the skill `test` ("one two" and "three").

Having to run a command every time you want to test a change can be tedious. You can also start a REPL (Read-Eval-Print Loop) for a skill to run the skill repeatedly.

![Screenshot of a REPL session](https://user-images.githubusercontent.com/19977/131403268-a14728e9-15f0-4c54-9b94-062a5a4755b7.png)

Note that when you are in a REPL session, you test your skill by typing in the arguments and hitting `ENTER`. You don't need to quote the set of arguments. You can leave the REPL running while you make changes to the skill code. Each time you call the skill in the REPL, it'll run the latest saved version of the skill on your local machine.

## Deploying a skill

At some point, you'll want to deploy your local changes to Abbot. To do that, run `abbot deploy` with the name of the skill. This will update the skill at https://ab.bot/ with your local changes, making them live.

```bash
$ abbot deploy test
Skill test updated
```

## Next Steps

We're still in the early stages of building the Abbot CLI, but we have a nice foundation for local editing we can build on. With this tool, it wouldn't be too hard to set up a GitHub workflow for editing skills. For example, add your Abbot workspace to a GitHub repository and set up a GitHub action to run `abbot deploy` every time you merge changes to your main branch. While it is easy to shell out to `abbot deploy`, we'd like to create a proper GitHub Action for Abbot development.

Other changes we want to make include improving the local editing experience for JavaScript and Python skills. And lastly, we'd like to hear your feedback to help us prioritize future development. As I mentioned earlier, the [Abbot CLI is open source and hosted on GitHub](https://github.com/aseriousbiz/abbot-cli). If you have any feedback, please open an issue or pull request on GitHub.
