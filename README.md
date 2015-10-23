# cf-mysql-ci
Contains Concourse CI scripts and configuration we use to test [cf-mysql-release](https://github.com/cloudfoundry/cf-mysql-release)

#### Configure a pipeline
```
fly configure -c pipelines/cf-mysql.yml --vars-from YOUR_CREDS_FILE.yml cf-mysql
```

#### Credentials

The pipeline config files are parametrized to allow private credentials to be stored outside this repo.
If your credentials file (`YOUR_CREDS_FILE.yml`) is missing a required field, `fly` will report it as an error, e.g.:
```
* unbound variable in template: 'cf-docker-username'
```

#### Environment Config Files

The cf-mysql and cf-mysql-acceptance pipelines are also parametrized to allow CI to deploy to different environments.
The cf-mysql pipeline deploys to `initial_env` at the start of the pipeline, and `integration_env` at the end.
The cf-mysql-acceptance pipeline performs a single deploy to `acceptance_env`.
There are a collection of variables in the pipeline configs (e.g. `{{initial_env_bosh_url}}`) to allow these environments to be specified by the user.
These config variables can be defined in the above credentials file, or by adding `--vars-from YOUR_ENVS.yml` to the above command.

In addition to the Concourse parameters, our scripts expect the following files to exist for each environment:
```
${DEPLOYMENTS_CONFIG_DIR}/${ENV_NAME}/cf-aws-stub.yml
${DEPLOYMENTS_CONFIG_DIR}/${ENV_NAME}/cf-networks-stub.yml
${DEPLOYMENTS_CONFIG_DIR}/${ENV_NAME}/cf-shared-secrets.yml
${DEPLOYMENTS_CONFIG_DIR}/${ENV_NAME}/cf-mysql-plans-stub.yml
${DEPLOYMENTS_CONFIG_DIR}/${ENV_NAME}/cf-properties.yml
${DEPLOYMENTS_CONFIG_DIR}/${ENV_NAME}/cf-mysql-secrets.yml
```
