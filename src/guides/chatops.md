# What are ChatOps?

We say that Abbot is a "shared command line for your team", and talk a lot about the "power of ChatOps", but what are they? Why do they matter? 

ChatOps is a way to describe working together to do things by running commands in chat. Teams use ChatOps to do things like merge Pull Requests, get graphs from network appliances, and cheer each other on for doing cool stuff. This is really useful because new people can see how to use all the internal tools by example. 

When a new person joins a team using Abbot, they can see how that team interacts with the multitude of systems in place. If there's an incident, Abbot can be in #ops along with the rest of the ops team, updating statuses (`@abbot status yellow We are experiencing some delays in the widgetizer`), sending Tweets (`@abbot tweet The widgetizer is back online, sorry for the noise!`), or getting statuses from other services (`@abbot ping widgetizer.ourwebsite.com`).

By making common tasks accessible from chat, everyone can see who has run commands, how to run them, and run their own commands. For example, if there's an outage people can see exactly what's happening by watching their teammates interacting with Abbot to solve the problem. 

Abbot makes it easy to implement your own ChatOps by providing a platform where you can script your own integrations. This allows you and your team to move away from running scripts on your own command line (everyone has them!) and into a shared location where everyone can leverage the tools everyone else has built. 