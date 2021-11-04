# Help

If you're having a bot emergency, contact us at [help@ab.bot](mailto:help@ab.bot). You can also join us in [Discord](https://discord.gg/FN4t8NNdQG). We work in Pacific Time and are in there consistently most days.

## Check out our latest video

> [!Video https://www.youtube.com/embed/6NHMyyWZtrU]

## Getting Started

The easiest way to get started with Abbot is by following our [Getting Started](xref:guides) guide.

For a more concise, reference-based view; please refer to [Abbot's reference documentation](xref:legacy-reference).

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


### I'm experiencing an issue, and would like to report it!

First, we're sorry that you're running into issues. Please take a look at our list of [known issues](xref:known-issues) to make sure we're not already aware of it. If not, please drop us a line at [help@ab.bot](mailto:help@ab.bot) or send us feedback directly using `@abbot feedback` from chat or the Bot Console.
