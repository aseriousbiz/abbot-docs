---
uid: chatops-intro
---

# What are ChatOps?

We say that Abbot is a "shared command line for your team", and talk a lot about the "power of ChatOps", but what are they? Why do they matter?

ChatOps is a way to describe working together to do things by running commands in chat. Teams use ChatOps to do things like merge Pull Requests, get graphs from network appliances, and cheer each other on for doing cool stuff. This is really useful because new people can see how to use all the internal tools by example.

When a new person joins a team using Abbot, they can see how that team interacts with the multitude of systems in place. If there's an incident, Abbot can be in #ops along with the rest of the ops team, updating statuses (`@abbot status yellow We are experiencing some delays in the widgetizer`), sending Tweets (`@abbot tweet The widgetizer is back online, sorry for the noise!`), or getting statuses from other services (`@abbot ping widgetizer.ourwebsite.com`).

By making common tasks accessible from chat, everyone can see who has run commands, how to run them, and run their own commands. For example, if there's an outage people can see exactly what's happening by watching their teammates interacting with Abbot to solve the problem.

Abbot makes it easy to implement your own ChatOps by providing a platform where you can script your own integrations. This allows you and your team to move away from running scripts on your own command line (everyone has them!) and into a shared location where everyone can leverage the tools everyone else has built.

## Getting started with Abbot

So you've just installed Abbot (congratulations!) and want to write a skill.

Abbot tries to make building skills as painless as possible, and offers some convenience methods to you so that you don't have to deal with them.

You can write skills in C#, Python, or JavaScript. We are adding new features and language support all the time! To get started...

* [Try the Hello World Guide](xref:hello-world-guide)

Unsure which language to start with? Abbot allows you to use all of these languages in a single bot, so you can experiment with each language and see what works best for you.
If you really aren't sure, Abbot is built in C#, and it's what we use to build most of our skills. We like using C# for skills because static typing makes it easy to reason about our code, and the editing experience on https://ab.bot is pretty good!

Please note that once you save a skill in Abbot's web editor, that skill is live in your chat. You can test locally before saving, if you'd like.

For a more concise, reference-based view; please refer to [Abbot's reference documentation](xref:legacy-reference).

## More Guides

* [Argument parsing in C#](xref:parsing-arguments-c-sharp)
* Creating and using [List Skills](xref:list-skills)
* Scheduled Skills and reacting to events with [Triggers](xref:triggers)
* Writing Javascript skills, the [async way](xref:js-async)

## Frequently Asked Questions

### What languages are supported?

You can write skills using C# 9, Python 3.7, or JavaScript in Node 10.
Later we will support WASM assemblies in a paid version of Abbot.

### What is the right way to save data from my script?

Abbot provides a convenient way to save data from your skills. Please take a look at [Abbot's reference documentation](xref:legacy-reference) for details on how to use Abbot's brain.

### How do skills deal with sensitive data?

There is a Secret management system built in to Abbot. This allows your script to work with sensitive data without including in your script. Please note that developers can still get access to the data since we provide a scripting environment; so you should never store passwords in Secrets (and you should be careful about who you grant access to Abbot). The easiest way to see how to access Secrets in an Abbot skill is via [Abbot's reference documentation](xref:legacy-reference).

### What Libraries are available in my skills?

We provide a set of pre-installed libraries for you to use in your skills.
Custom dependencies will be available later in a paid version of Abbot. If you need a dependency for your skill that isn't listed here, please reach out to us at [help@ab.bot](mailto:help@ab.bot) with some information about the dependency you'd like for us to add.

#### Preloaded C# libraries

* All libraries available in .NET Core 3.1
* NodaTime 3.0.3
* NewtonSoft.Json 12.0.3
* HtmlAgilityPack 1.11.28
* Dapper 2.0.90
* MysqlConnector 1.3.10
* Npgsql 5.0.7

#### Available JavaScript libraries

* lodash 4.17.19
* axios 0.21.1
* request-promise 4.2.6
* node-fetch 2.6.1
* passport 0.4.1
* passport-auth 1.0.0
* knex 0.95.6
* mysql 2.18.1
* pg 8.6.0

#### Available Python libraries

* arrow 1.1.0
* BeautifulSoup 4.9.3
* boto3 1.17.64
* jsonpickle 2.0.0
* numpy 1.19.4
* octokit.py 0.15.0
* mysql-connector-python 8.0.25
* pandas 1.0.5
* psycopg2 2.9.1
* requests 2.25.1
* requests-oauth 1.3.0
* soupsieve 2.0.1
* SQLAlchemy 1.4.19