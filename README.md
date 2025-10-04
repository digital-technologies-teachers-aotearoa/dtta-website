# DTTA website

The DTTA website is centered on using [Association Management Software](https://github.com/digital-technologies-teachers-aotearoa/ams) (AMS) configured for the association.
This repository stores any custom configuration required, and is also used for deploying code with version control (GitOps).

## Environments and deployments

Environments are configured with a YAML file within the `environments/` directory.

### Environments

The following environments are supported:

- UAT (User acceptance testing) - `uat.yml`

Production will be added a later stage.

### Environment file configuration

The following keys are supported:

- `ams` - Value corresponds to a tag for an available [AMS Docker image](https://github.com/digital-technologies-teachers-aotearoa/ams/pkgs/container/ams-django).

##### Example environment file

```yaml
ams: 1.7.3
```

Additional keys may be added at a later stage for additional services, such as Discourse.

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

AMS instances require configuration such as expected environment variables, see [AMS Deployment Documentation](https://digital-technologies-teachers-aotearoa.github.io/ams/developer/deployment/#production-environment).

These are currently setup manually, though could be moved to an infrastructure as code setup if required.
