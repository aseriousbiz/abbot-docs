---
uid: privacy-and-security
---

# Privacy and Security

## What can Abbot see?

We request the minimum set of permissions required to allow Abbot to work in your chat room. Abbot can only respond to messages where it is directly mentioned, or to direct messages in platforms that support direct messaging. With the exception of some special built-in skills, Abbot skills always require Abbot to be mentioned first, followed by the skill name, followed by any arguments passed to the skill.

Abbot is not able to proactively message any user. Abbot can only respond in rooms where it's been invited, and only after it's been triggered at least once. Users can trigger Abbot by calling a skill, or attaching a trigger of any kind.

Any message that Abbot responds to is logged, and available in the Activity Log. We try to only show pertinent information in the logs, but be aware that messages sent via Direct Message to Abbot can be viewed in the Activity Log.

## What can A Serious Business see?

Staff have the ability to review Activity Log entries. This is both to help us identify issues and help users troubleshoot problems. We are able to view skill code if requested by our users, but any time we view sensitive data (like code), it's logged and will show up in your audit log (it will say something like `(STAFF) Paul viewed code for foo-bar`). We are happy to show you what this looks like if you are curious. Email us at [help@ab.bot](mailto:help@ab.bot) and we can help you out.

## Who can access Abbot

To run commands, any authenticated chat user in your organization can interact with Abbot.

Even without Abbot, if someone gains access to a chat account in your organization, they can do immense damage. So it's very important for to enforce good account security policies such as requiring 2-factor auth, etc. If you use Abbot, that's even more important.

To access https://ab.bot/ and write skills, it depends on an organization Admin setting. Your Abbot Administrator can restrict users to those they approve, or they can let anyone in your chat organization access the site.

## How can I restrict access to Abbot skills?

When editing a skill, you can set it to be restricted to a limited set of users in the UI. Then in chat you can give users access to the skill with the `can` skill. For example, `@abbot @haacked can use cool-skill` gives @haacked access to run the skill named `cool-skill`.

## How secrets are stored

Skill secrets are kept in Azure Key Vault. It is not possible to view the values of a secret from the website. Secret values may only be updated or deleted. Be aware that users with edit access to your skills may read a secret in the skill code to cause Abbot to return a secret (in the console, or otherwise). If you have skills where this may be an issue, we suggest using Access Controls to lock the skill down to only trusted users. Another alternative is to use ProxyLinks to encode sensitive endpoint data. A ProxyLink can be created in one skill and reused in another, so it's possible to have a skill that only admins can view which contains ProxyLinks for other skils to consume.

## Our commitment to privacy

We believe that users have the right to private and secure computing. We continue to refine our approach to ensure that we can give our users the most flexibility possible while also maintaining an environment free of abuse. We are always happy to answer questions about how we handle your data -- send us an email at [help@ab.bot](mailto:help@ab.bot) if you have any questions.

## I've found a problem and want to report it! 

Please email us at [help@ab.bot](mailto:help@ab.bot) if you have found an issue that you'd like to report. 
