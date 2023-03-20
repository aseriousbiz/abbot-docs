# Self-Hosted Skill Runners

If you need more functionality than we provide on the default hosted skill runners, you can host your skill runners.
Currently, only the Python runner can be self-hosted, contact us at `support@ab.bot` if you're interested in self-hosting other runners.

> [!NOTE]
> Custom Runners are not available to all accounts by default, please contact `support@ab.bot` if you want to enable this feature on your account.

The Python runner is open-source at [aseriousbiz/abbot-py](https://github.com/aseriousbiz/abbot-py), and you can build it yourself there using the `Dockerfile`.
Alternatively, there are public docker images available which you can deploy directly, or use as a base image to install further dependencies (see below):

* Python: `abbotpublic.azurecr.io/runner/python`

## Tutorial: Deploying a runner to Fly.io

[Fly.io](https://fly.io) is a platform for quickly launching applications all over the world.
It's a great platform for hosting Abbot skill runners, since you can quickly launch any Docker image you want.

To begin, make sure you've signed up for an account at [Fly.io](https://fly.io).
Download the [Fly CLI](https://fly.io/docs/hands-on/install-flyctl/) and log in to it.

Create a new Fly application by moving to an empty folder on your machine and running the following command:

```
> flyctl launch
```

Enter any app name you'd like, select the organization and region (we recommend `sea`).
You **do not** need to create a Postgresql or Redis database to deploy the runner.
Choose **not** to deploy now (it's OK if you already deployed it, it just won't work until we set up the skill runner token).

Create a `Dockerfile` with the following text:

```dockerfile
FROM abbotpublic.azurecr.io/runners/python:latest
ENV HOST=0.0.0.0
ENV PORT=8080
ENV ABBOT_SANDBOX_POLICY=none # See "Sandboxing" below
ENV AbbotApiBaseUrl="https://app.ab.bot/api"
```

Generate a random token to use to authenticate Abbot and the runner.
This token ensures that only your organization can connect to and run skills on the runner.
To generate the token, just generate a random string, at least 64 characters long, and save it as a secret in Fly:

```shell
> ABBOT_SKILL_RUNNER_TOKEN=$(openssl rand -hex 64)
> flyctl secrets set ABBOT_SKILL_RUNNER_TOKEN="$ABBOT_SKILL_RUNNER_TOKEN"
# Don't lose ABBOT_SKILL_RUNNER_TOKEN! If you do, you'll need to regenerate it and redeploy.
```

Now, you can deploy the app:

```shell
> flyctl deploy
```

Validate that the runner is working by accessing the `https://your-app-name.fly.dev/api/v1/status` endpoint (replacing `your-app-name` with whatever app name you chose when creating the Fly app).
You should see a `"status": "ok"` field in the JSON payload.

Now, go to your [Organization Settings](https://app.ab.bot/settings/organization) in Abbot, and select "Custom Runners" on the far right.
If you don't see "Custom Runners", contact `support@ab.bot` to ask about enabling this feature for your account.

Click "Edit" next to "Python" in "Runner Endpoints":

![The "Edit" button next to "Python" in "Runner Endpoints"](https://user-images.githubusercontent.com/7574/226437223-d1310902-b8f6-4d69-b89c-2c8bd68cbb04.png)

Insert the public URL to the `/api/v1/execute` endpoint of your custom runner as the "Url".
Paste the token you generated in `ABBOT_SKILL_TOKEN` above as the "Api Token".

![Editing the Python Endpoint](https://user-images.githubusercontent.com/7574/226443164-fe2f6918-1f0a-4c24-86ac-0454f28ef42e.png)

Save, and that's it!
You're now running your runner on Fly!

## Other deployments

You can launch Abbot Runners anywhere a Docker image can be hosted.
For example, all the below environments should be usable, though not all have been tested:

* [Fly.io](https://fly.io)
* [Azure Container Instances](https://azure.microsoft.com/en-us/products/container-instances)
* [Amazon Elastic Container Service (ECS)](https://aws.amazon.com/ecs/)
* [Google Cloud Run](https://cloud.google.com/run/)

You can also host the runner on your own infrastructure.

## Deployment prerequisites

To launch a self-hosted runner, you must set the following environment variables:

* `HOST` must be set to the public-facing IP address you want to access the runner on, or `0.0.0.0` to bind to all IP addresses
* `PORT` can be set to any value, but whatever port you use must be mapped to a public-facing address
* `ABBOT_SANDBOX_POLICY` must be set to the sandboxing policy you want to use in your runner (see "Sandboxing" below)
* `AbbotApiBaseUrl` must be set to `https://app.ab.bot/api`
* `ABBOT_SKILL_RUNNER_TOKEN` must be set to a 64 character (or longer) random string that will be used as the "Api Token" value when configuring your custom runner.

## Sandboxing

Since the Skill Runner is designed to run skill code your users create, we provide _some minor_ sandboxing options within the runner itself to ensure skill code can't do things it shouldn't be able to do.

> [!NOTE]
> The sandboxing within the runner itself is **NOT SUFFICIENT** to ensure safety when running untrusted code from third-parties.
> You are still responsible for ensuring the skills you write don't abuse your own runner infrastructure.

There are three sandboxing policies available.
Select the policy you want to apply by setting the `ABBOT_SANDBOX_POLICY` environment variable:

* `none` - Absolutely no sandboxing is performed. The skill code runs unrestricted.
* `permissive` - Permissive sandboxing, generally good for preventing unintentional disruption to the runner:
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
ENV HOST=0.0.0.0
ENV PORT=8080
ENV ABBOT_SANDBOX_POLICY=none # See "Sandboxing" below
ENV AbbotApiBaseUrl="https://app.ab.bot/api"

RUN pip install cuddle
```
