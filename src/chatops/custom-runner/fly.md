---
uid: runner-on-fly
Title: "Tutorial: Deploying a runner to Fly.io"
---

## Tutorial: Deploying a runner to Fly.io

[Fly.io](https://fly.io) is a platform for quickly launching applications all over the world.
It's a great platform for hosting Abbot skill runners, since you can quickly launch any Docker image you want.

To begin, make sure you've signed up for an account at [Fly.io](https://fly.io).
Download the [Fly CLI](https://fly.io/docs/hands-on/install-flyctl/) and log in to it.

Create a new Fly application by moving to an empty folder on your machine and running the following command:

```shell
> flyctl launch
```

Enter any app name you'd like, select the organization and region (we recommend `sea` or `sjc` because of their proximity to Abbot's infrastructure).
You **do not** need to create a Postgresql or Redis database to deploy the runner.

Create a `Dockerfile` with the following text:

```dockerfile
FROM abbotpublic.azurecr.io/runners/python:latest
ENV ABBOT_SANDBOX_POLICY=none
```

Generate a random token to use to authenticate Abbot and the runner.
This token ensures that only your organization can connect to and run skills on the runner.
To generate the token, just generate a random string, at least 64 characters long, and save it as a secret in Fly:

```shell
> ABBOT_SKILL_RUNNER_TOKEN=$(openssl rand -hex 64)
> flyctl secrets set ABBOT_SKILL_RUNNER_TOKEN="$ABBOT_SKILL_RUNNER_TOKEN"
```

> [!NOTE]
> Don't lose ABBOT_SKILL_RUNNER_TOKEN! If you do, you'll need to regenerate it and redeploy.

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

Insert the public URL to the `/api/v1/execute` endpoint of your custom runner as the "Url", e.g. 
`https://your-app-name.fly.dev/api/v1/execute`.
Paste the token you generated in `ABBOT_SKILL_TOKEN` above as the "Api Token".

![Editing the Python Endpoint](https://user-images.githubusercontent.com/7574/226443164-fe2f6918-1f0a-4c24-86ac-0454f28ef42e.png)

Save, and that's it!
You're now running your runner on Fly!
