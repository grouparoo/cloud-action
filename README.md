# grouparoo/cloud-action

A GitHub Action for connecting your GitHub repository to Grouparoo Cloud.

To use this Action you'll need to have an existing [Grouparoo Cloud](https://cloud.grouparoo.com/) project. If you don't have one yet, you can sign up for a free trial at [grouparoo.com/trial](https://grouparoo.com/trial).

## Usage

Make sure to first checkout the repository by using [actions/checkout](https://github.com/marketplace/actions/checkout). Then, this action will do the rest.

```yml
      - uses: actions/checkout@v2
        with:
          ref: ${{ github.head_ref }}
      - uses: grouparoo/cloud-action@v1
        with:
          project_id: ${{ secrets.GROUPAROO_CLOUD_PROJECT_ID }}
          token: ${{ secrets.GROUPAROO_CLOUD_TOKEN }}
```

For a full usage example with validation on Pull Requests and automatic deployments when merging to `main`, check out your project's "Integrations" page on Grouparoo Cloud, or take a look at our [example project](https://github.com/grouparoo/app-example-cloud/blob/main/.github/workflows/grouparoo-cloud.yml).

### Action inputs

The only required inputs are `project_id` and `token`. You can find these values on your project's "Integrations" page on Grouparoo Cloud. A good practice is to set these up as secrets for your GitHub repository.

| Name | Description | Default |
| --- | --- | --- |
| `project_id` | Grouparoo Cloud Project ID (**required**) | |
| `token` | Grouparoo Cloud API Token (**required**) | |
| `type` | Should the action run `ci` or `deploy`? | `ci` |
| `api_url` | Override the base URL for the Grouparoo Cloud API | `https://cloud.grouparoo.com` |

### Action behavior

This action works by internally running the `grouparoo ci` and `grouparoo deploy` commands from within your Grouparoo project with the appropriate inputs. 

It will also send some additional metadata along with the Configuration to allow you to cross-reference GitHub PRs and commits from Grouparoo Cloud:

- Pull Requests:
  - Pull Request URL
  - Pull Request Title
- Pushes:
  - Commit message
  - Commit URL

#### Repository structure

This Action assumes that your Grouparoo project is the root of the GitHub repository, and your configuration directory is `/config`.

Example:

```
.
├── config
│   ├── apps
│   │   ├── hubspot.json
│   │   └── snowflake.json
│   ├── destinations
│   │   └── hubspot.json
│   ├── models
│   │   └── users.json
│   ├── properties
│   │   ├── email.json
│   │   ├── first_name.json
│   │   ├── last_name.json
│   │   └── user_id.json
│   └── sources
│       └── users.json
└── package.json
```

