---
uid: self-hosted-runners
Title: Self-Hosted Skill Runners
---

# Self-Hosted Skill Runners

If you need more functionality than we provide on the default hosted skill runners, you can host your skill runners.
Currently, only the Python runner can be self-hosted; contact us at `support@ab.bot` if you're interested in self-hosting other runners.

> [!NOTE]
> Custom Runners are not available to all accounts by default. Please contact `support@ab.bot` if you want to enable this feature on your account.

The Python runner is open-source at [aseriousbiz/abbot-py](https://github.com/aseriousbiz/abbot-py), and you can build it yourself there using the `Dockerfile`.
Alternatively, there are public docker images available which you can deploy directly, or use as a base image to install further dependencies (see below):

* Python: `abbotpublic.azurecr.io/runner/python`

## Where to deploy

You can launch Abbot Runners anywhere a Docker image can be hosted.
For example, we provide tutorials for these environments:

* <xref:runner-on-fly>

You can also host the runner on your own infrastructure.

## Deployment prerequisites

To launch a self-hosted runner, you must set the following environment variables:

* `ABBOT_SANDBOX_POLICY` must be set to the sandboxing policy you want to use in your runner (see "Sandboxing" below)
* `ABBOT_SKILL_RUNNER_TOKEN` must be set to a 64 character (or longer) random string that will be used as the "Api Token" value when configuring your custom runner.
* `PORT` (Optional) can be used to specify the port _within the container_ on which the runner will listen. You need to configure your hosting environment to bind this port to a public address. Defaults to `80`.

The runner does not support TLS connections.
We **strongly recommend** that you **only** host the runner in an environment that provides TLS termination, and only connect to the runner using an `https` URL.

## Sandboxing

Since the Skill Runner is designed to run skill code your users create, we provide _some minor_ sandboxing options within the runner itself to ensure skill code can't do things it shouldn't be able to do.

> [!NOTE]
> The sandboxing within the runner itself is **NOT SUFFICIENT** to ensure safety when running untrusted code from third-parties.
> You are still responsible for ensuring the skills you write don't abuse your own runner infrastructure.

There are three sandboxing policies available.
Select the policy you want to apply by setting the `ABBOT_SANDBOX_POLICY` environment variable:

* `none` - Absolutely no sandboxing is performed. The skill code runs unrestricted.
* `permissive` - (Default) Permissive sandboxing, generally good for preventing unintentional disruption to the runner:
  * Skills are forbidden from accessing certain Python modules that could allow access to the runner itself (`os`, `subprocess`, etc.).
* `restrictive` - Most restrictive sandboxing, limits skills to well-known modules and functionality:
  * Skills can only access known-good modules.
  * Skill code cannot access members of a Python class with a name starting with `_`.

## Customizing your image

You can customize your image in the `Dockerfile` by running additional commands after the initial `FROM`.
For example, you can add in additional dependencies to the Python runner using `RUN pip install` commands in the `Dockerfile`.

The below dockerfile installs the [`cuddle` module](https://github.com/djmattyg007/python-cuddle), for parsing [KDL](https://kdl.dev) documents.

```dockerfile
FROM abbotpublic.azurecr.io/runners/python:latest
ENV ABBOT_SANDBOX_POLICY=none

RUN pip install cuddle
```
