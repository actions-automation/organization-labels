# manage-your-labels

A Github Action to manage labels across organization or user repositories.
Specify which labels you want on which repositories in one place, and let a bot
take care of the rest.

## Usage

To get started, make sure you've created the `.github` repository within your
organization or user account. `.github` is a special repository for
Github-specific community health files, as per the
[documentation](https://docs.github.com/en/free-pro-team@latest/github/building-a-strong-community/creating-a-default-community-health-file).
`manage-your-labels` must run on the `.github` repository to ensure only one
instance of it is active -- otherwise, conflicts between competing
configurations may happen.

### Authenticating with Github

To make sure everything works, you'll need to ensure a personal access token
with the `public_repository` scope is available via the `BOT_TOKEN` environment
variable. The account used to create the PAT must also have permission to edit labels on
the repository you want to manage.

Following the guide over on the [`pull-request-responsibility`](https://github.com/actions-automation/pull-request-responsibility#github-authentication-with-a-personal-access-token) repo should suffice.

### Configuration

`manage-your-labels` uses a config file stored at `manage-your-labels.yml`
within the top-level directory of the `.github` repo for label management. You
can read more about how to set up that configuration and see an example in the
[`input`](input/) directory.

With a valid configuration set up, create a new Actions workflow within the
`.github` repo and set it up like so:

```yml
name: manage-your-labels
on:
  schedule:
    - cron: "*/15 * * * *"
  workflow_dispatch:
jobs:
  mirror-labels:
    runs-on: ubuntu-latest
    env:
      BOT_TOKEN: ${{ secrets.BOT_TOKEN }}
    name: manage-your-labels
    steps:
      - uses: actions/checkout@v2
      - uses: actions-automation/manage-your-labels@main
```

From there, the action should work its magic!
