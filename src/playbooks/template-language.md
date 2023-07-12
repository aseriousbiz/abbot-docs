# Abbot Templating Language

Inputs provided to Playbook Steps can be templates that will be rendered using the Abbot Templating Language, which is derived from [Handlebars](https://handlebarsjs.com).
Handlebars is a powerful templating language that allows you to use variables, conditionals, and loops to generate text.
Here, we'll focus on the basics and cover the variables that Abbot makes available to you, and how to use them.

## Where can you use templates?

Any playbook step that accepts text input can include a template.
For example, the "Message" input for the "Send Message" step: 

<img width="332" alt="image" src="https://github.com/aseriousbiz/abbot-docs/assets/7574/33b923cd-33de-4aa1-9920-02a3e9cdb9e5">

In addition, some drop-down inputs support pre-defined templates.
For example, the "Channel" input for the "Send Message" step supports "Channel from Triggers" which is a template that will be replaced with the channel that triggered the playbook:

<img width="290" alt="image" src="https://github.com/aseriousbiz/abbot-docs/assets/7574/808b0e25-03ee-43ef-af5c-5c9862e851d0">

Other steps _only_ support template values, such as the "Condition" in the "Continue If" step, which only allows a template from a predefined set of templates:

<img width="295" alt="image" src="https://github.com/aseriousbiz/abbot-docs/assets/7574/b749b9f4-7b4c-445f-a447-94d01369dfb5">

All of these are implemented with templates, though you only need to write templates when using free-form text inputs.

## Handlebars Basics

Handlebars is a fairly simple templating language.
Most text is passed through unchanged, but you can use special syntax to insert variables.
Variables are surrounded by double curly braces, like `{{this}}`.

We provide a template "context", which is a set of variables that you can use in your templates.
Many of these variables are objects that have their own properties, which you can access using dot notation.
For example `{{trigger.outputs.channel.name}}` will be replaced with the name of the channel that triggered the playbook.

## Template Context

The following variables are available in all templates:

| Variable | Description |
| -------- | ----------- |
| `trigger` | Results from the trigger that started the playbook. |
| `previous` | Results from the previous step. |
| `outputs` | A merged view of all the "outputs" from all previous steps. Outputs from later steps override outputs from earlier steps. |

### The `trigger` object

The `trigger` variable is a "Step Results" object and has the following properties:

| Variable | Description |
| -------- | ----------- |
| `trigger.id` | The ID of the trigger |
| `trigger.outcome` | The outcome of the trigger. Will always be `Succeeded`. |
| `trigger.outputs` | A table of output values from the trigger. The values in this table vary depending on the trigger. |

### The `previous` object

The `previous` variable is also "Step Results" object and has the following properties:

| Variable | Description |
| -------- | ----------- |
| `previous.id` | The ID of the previous step in the playbook |
| `previous.outcome` | The outcome of the previous step in the playbook. Will always be `Succeeded`. |
| `previous.outputs` | A table of output values from the previous step in the playbook. The values in this table vary depending on the step. |

> [!NOTE]
> For the first step in a playbook, `previous` will be identical to the `trigger`.

### The `outputs` object

The `outputs` variable is a table of all the outputs from all previous steps in the playbook.
Unlike, say, `trigger.outputs` which is **just** the outputs from the trigger, or `previous.outputs` which is **just** the outputs from the previous step, this is a merged view of all `outputs` from all steps that have executed.
If a later step has an output with the same name as an earlier step, the later step's output will override the earlier step's output.

## Some examples

Here are some examples of triggers and steps and how you can use their outputs in templates.
This isn't a full reference of all the outputs available, but it gives a brief overview of what to expect.

### Accessing the channel that triggered the playbook

If you use a trigger like "Conversation Started", "Reaction Added", "Abbot Added to Channel", or other triggers that are caused by events related to Slack messages, the Slack channel in which that event occurred is available in the `trigger.outputs.channel` object.
That object has the following properties:

| Variable | Description |
| -------- | ----------- |
| `trigger.outputs.channel.id` | The ID of the channel |
| `trigger.outputs.channel.name` | The name of the channel |

If you want to mention the Channel in a Slack message, use Slack's [linking syntax](https://api.slack.com/reference/surfaces/formatting#linking-channels):

```
Hey everyone, we got a new customer. They're in <#{{ trigger.outputs.channel.id }}>!
```

### Accessing the Customer that triggered the playbook

If you trigger a playbook using the "New Customer Created" trigger, a `customer` output is available that contains information about the customer:

| Variable | Description |
| -------- | ----------- |
| `trigger.outputs.customer.id` | The ID of the customer |
| `trigger.outputs.customer.name` | The name of the customer |
| `trigger.outputs.customer.segments` | A list of the Segments the customer is in |
| `trigger.outputs.customer.channels` | A list of the Slack channels associated with the customer |

In addition, all of the channels associated with the customer are available in the `trigger.outputs.channels` object.
The _first_ channel in the channel list is also available in the `trigger.outputs.channel` object, for convenience.

This allows you to send a message to a customer when you create their record in Abbot, for example:

<img width="984" alt="image" src="https://github.com/aseriousbiz/abbot-docs/assets/7574/a4b8743c-77fc-4e66-9259-15c2f754c37e">

The "Channel from triggers" here is equvalent to the template `trigger.outputs.channel.id`, so this will post a message to the first channel associated with the customer.
This example assumes you've assigned a channel to the customer when you created their record in Abbot.

There are also steps that _create_ channels, such as "Create Channel for Customer".
These make the channel they create available in the `outputs.channel` variable, so you can use that in subsequent steps:

<img width="1093" alt="image" src="https://github.com/aseriousbiz/abbot-docs/assets/7574/9d08e2ae-6819-4083-9e44-ec3d228a4205">

This example creates a new channel for the customer based on the prefix you specify and the name of the customer.
For example, given a Customer name of `Funny Business` and a prefix of `cust-`, the channel name will be `cust-funny-business`.
Then, in a subsequent step, you can use the `outputs.channel.id` template to mention the channel in a message.
The example playbook above posts a mention of the newly created channel in the `#biz` channel.

