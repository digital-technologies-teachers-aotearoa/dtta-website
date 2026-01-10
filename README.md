# DTTA website

The DTTA website is centered on using [Association Management Software](https://github.com/digital-technologies-teachers-aotearoa/ams) (AMS) configured for the association.
This repository stores any custom configuration required, and is also used for deploying code with version control (GitOps).

## Environments and deployments

Environments are configured with configuration files within an `environments/` directory.

### Environments

The following environments are supported:

- UAT (User acceptance testing) - `environments/uat/`
- Production - `environments/prod/`

### Environment file configuration

The following keys are supported within the `config.yml` file:

- `ams` - Value corresponds to a tag for an available [AMS Docker image](https://github.com/digital-technologies-teachers-aotearoa/ams/pkgs/container/ams-django).

##### Example environment file

```yaml
ams:
  version: 1.0.0
  envs:
    AMS_NOTIFY_STAFF_MEMBERSHIP_EVENTS: True
    AMS_NOTIFY_STAFF_ORGANISATION_EVENTS: True
    AMS_REQUIRE_FREE_MEMBERSHIP_APPROVAL: False
```

### GitHub configuration

For each defined environment, the following must exist:

- GitHub Actions workflow for each environment.
   - Workflow should be configured to run whenever the associated environment file is modified.
- GitHub repository environment, configured with required secrets.

### DigitalOcean setup

Each environment requires the following:

- App Platform
   - Web Service - Django AMS
   - Job - Django AMS running `python /app/manage.py deploy_steps` before each deployment
- Database - Postgres
- Discourse instance (managed or self hosting)

AMS instances require configuration such as expected environment variables, see [AMS Deployment Documentation](https://digital-technologies-teachers-aotearoa.github.io/ams/developer/deployment/#production-environment).

The deployment workflow sets up the infrastructure required using `app.yml` files during the deployment.
These files define the required infrastructure, configuration values, environment variables, etc.
Required secrets are loaded from a GitHub environment to populate the required environment variables (encrypted values are entered directly in Digital Ocean, then the corresponding encrypted value is copied from the online `app.yaml` and entered into the repository file).

These can be deployed or adjusted manually on the Digital Ocean website if required, however a new code deployment would override changes made directly through the Digital Ocean website.
