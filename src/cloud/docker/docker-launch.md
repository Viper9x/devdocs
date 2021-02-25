---
group: cloud-guide
title: Use and manage the Docker environment
functional_areas:
  - Cloud
  - Setup
  - Configuration
redirect_from:
  - /cloud/reference/docker-launch.html
---

`{{site.data.var.mcd-prod}}` deploys Magento to a read-only file system by default in the Docker environment, which mirrors the read-only file system deployed in the Production environment. You have the option to deploy a Docker environment in developer mode, which provides an active development environment with full, writable filesystem permissions.

You use the `ece-docker build:compose` command to generate the Docker Compose configuration from a Docker Compose configuration file and to deploy {{site.data.var.ece}} to a local Docker environment. The configuration file can be generated from multiple sources depending on your requirements. See [Configure sources]

{:.bs-callout-warning }
When you run the `ece-docker build:compose` command, it regenerates the `docker-compose,yml` configuration file and overwrites the existing `docker-compose.yml` configuration file. You can save custom configuration across builds by adding the settings to a `docker-compose.override.yml` file. See a detailed example in the [Docker quick reference][docker-reference].

## Set the launch mode

You can launch a Docker environment in production or developer mode by setting the `mode` option on the `ece-docker build:compose` command:

-  **Production mode**—The `--mode="production"` setting supports an active production environment with read-only filesystem permissions. This is the default configuration setting for launching a Docker environment. Selecting this option builds the Docker environment in production mode and verifies configured service versions. See [Production mode launch instructions][prod-mode].
-  **Developer mode**—The `--mode="developer"` setting supports an active development environment with full, writable filesystem permissions. Selecting this option builds the Docker environment in developer mode and verifies configured service versions. System performance is slower in developer mode because of additional file synchronization operations. See [Developer mode launch instructions][dev-mode].

For example, the following command starts the Docker configuration generator for the developer mode:

```bash
./vendor/bin/ece-docker build:compose --mode="developer"
```

To skip the interactive mode, use the `-n, --no-interaction` option.

{:.bs-callout-info}
The mode option for the `ece-docker build:compose` command does not affect the Magento mode. It determines the {{site.data.var.ece}} file system installation and read-only or read-write behavior.

## Stop and start containers

You can stop containers and restore them afterwards using the following methods.

Action | Command
------ | -------
Suspend containers to continue your work later | `docker-compose stop`
Stop and remove all containers, images, and volumes | `docker-compose down`
Start containers from a suspended state | `docker-compose start`
Stop the synchronization daemon | `docker-sync stop`
Start the synchronization daemon | `docker-sync start`

Use the following command to stop and remove the Docker configuration:

   ```bash
   docker-compose down -v
   ```

{: .bs-callout-warning}
This command removes all components of your local Docker instance including containers, networks, volumes, and images except for the persistent database and the `magento-sync` volume. See [Rebuild a clean environment][refresh].

<!--Link definitions-->
[docker-reference]: {{site.baseurl}}/cloud/docker/docker-quick-reference.html
[prod-mode]: {{site.baseurl}}/cloud/docker/docker-mode-production.html
[dev-mode]: {{site.baseurl}}/cloud/docker/docker-mode-developer.html
[refresh]: {{site.baseurl}}/cloud/docker/docker-containers.html#rebuild-a-clean-environment
