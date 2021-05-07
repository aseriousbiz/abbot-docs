---
uid: privacy-and-security
---

# Privacy and Security

## What can Abbot see?

We request the minimum set of permissions required to allow Abbot to work in your chat room. Abbot can only respond to messages where it is directly mentioned, or to direct messages in platforms that support direct messaging. With the exception of some special built-in skills, Abbot skills always require Abbot to be mentioned first, followed by the skill name, followed by any parameters passed to the skill.

Abbot is not able to proactively message any user. Abbot can only respond in rooms where it's been invited, and only after it's been triggered at least once. Users can trigger Abbot by calling a skill, or attaching a trigger of any kind.

Any message that Abbot responds to is logged, and available in the Audit Log. We try to only show pertinent information in the logs, but be aware that messages sent via Direct Message can be viewed in the audit log.

## What can A Serious Business see?

Staff have the ability to review audit log entries. This is both to help us identify issues and help users troubleshoot problems. We are able to view skill code if requested by our users, but any time we view sensitive data (like code), it's logged and will show up in your audit log (it will say something like `(STAFF) Paul viewed code for foo-bar`). We are happy to show you what this looks like if you are curious. Email us at [help@ab.bot](help@ab.bot) and we can help you out.

## How secrets are stored

Skill secrets are kept in Azure Key Vault. We have no way to directly view your secrets, but be aware that users with edit access to your skills may read a secret and then change a skill to cause Abbot to return a secret (in the console, or otherwise). If you have skills where this may be an issue, we suggest using Access Controls to lock the skill down to only trusted users. Another alternative is to use ProxyLinks to encode sensitive endpoint data. A ProxyLink can be created in one skill and reused in another, so it's possible to have a skill that only admins can view which contains ProxyLinks for other skils to consume.

## Our commitment to privacy

We believe that users have the right to private and secure computing. We continue to refine our approach to ensure that we can give our users the most flexibility possible while also maintaining an environment free of abuse. We are always happy to answer questions about how we handle your data -- send us an email at [help@ab.bot](help@ab.bot) if you have any questions.

## I've found a problem and want to report it! 

Please email us at [help@ab.bot](help@ab.bot) if you have found an issue that you'd like to report. 