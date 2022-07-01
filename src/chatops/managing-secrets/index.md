---
uid: managing-secrets
---

# Managing Secrets
Many skills access external APIs or otherwise need to authenticate to external services. Or, you may be interested in publishing your skill and want to make it easy for end-users to provide their own credentials without modifying the package's code. 

We provide a simple service for you to store and access secrets in your skills. To create a secret for your skill, click the link that says "Manage skill secrets" on the left side of the screen. This will take you to the secrets management page. Here, you can add a secret by clicking on the "Create Secret" button.

## Creating a secret
Secrets are referred to by their name in your code. Choose a name that will make sense when reading it in your skill code. A secret named "secret" may be confusing, but a secret named "api-token" might make sense if the skill only calls a single API. You should include a description for each secret to make it easier to reason about the secrets that any skill uses.

The "Value" field is what's returned by your secret when calling it from a skill. It is not possible to see the value from the website, so it's usually best to paste the secret value in from another location, especially in the case of API keys.

Once you click save, Abbot will store the secret in Azure Key Vault and it will be available in your skill immediately. The secret list page will show you the language-specific code that's required to access your secret from the vault. 

### A note about securing secrets
While it's not possible to view a secret from the website, it is possible for a user to emit the value from a secret in the console replying with the secret's value. If this is a concern for you, consider restricting the skill so that only trusted users can make edits to the skill's code. This will allow users to run the skill (if they've been given permissions to run it), without editing the skill. 

## Proxy Links
There are times when you may want to include an API key in your skill or package. Proxy Links make it possible to share skills with embedded sensitive data without leaking sensitive data, like tokens. A Proxy Link is a reverse proxy to a link you specify. Proxy Links can include embedded query strings. 

For example, let's say we are going to distribute a skill that includes an API token. We don't want anyone using our API key for anything but this skill, so we create a Proxy Link by clicking "Manage proxy links" on the left side of the "Edit Skill" screen. We click the "Create a link" button to get to the screen where we can add a Proxy Link. 

The URL for the external API (including our API key) looks like this: 

https://api.aserious.biz/?key=AN1C3K3YW0Ws0Co0l

We give the link a description like "A Serious API Key" and click "Create Link". We'll be returned to the list of Proxy Links, where we will see the link that was just created for us. It will look like this: https://link.ab.bot/api/AB3kF. That link is a reverse proxy to our API link. We can add more query string items the same way we would with any other link, and the Proxy Link service will construct the link correctly as it proxies. So if we wanted to pass a key called "things" with a value called "foo" we would use https://link.ab.bot/api/AB3kF?things=foo. You can use this link the same way you would any other link in your skill. 

Proxy Links can be distributed to anyone along with your skill. This makes it very easy to include custom API links in your packages.

Proxy Links can be created in one skill and used in another. If you have a lot of internal API keys you don't want shared you could create a restricted skill to hold nothing but Proxy Links, then limit access to the skill using restricted skills. 
