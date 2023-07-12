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
ENV PORT=8080
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

Now, go to the ["Runners" section of your Organization settings page](https://app.ab.bot/settings/organization/runners) in Abbot, and select "Custom Runners" on the far right.

Click "Edit" next to "Python" in "Runner Endpoints":

<img width="1539" alt="The &quot;Edit&quot; button next to &quot;Python&quot; in &quot;Runner Endpoints&quot;" src="https://user-images.githubusercontent.com/7574/227323557-a36ac954-e699-4dc0-a9bb-397125fdd6d3.png">


Insert the public URL to the `/api/v1/execute` endpoint of your custom runner as the "Url", e.g. 
`https://your-app-name.fly.dev/api/v1/execute`.
Paste the token you generated in `ABBOT_SKILL_TOKEN` above as the "Api Token".

<img width="663" alt="Editing the Python Endpoint" src="https://user-images.githubusercontent.com/7574/227323507-05b87e9d-7cad-4509-929a-a06d2d9d8c89.png">

Save, and that's it!
You're now running your runner on Fly!
