# Development Evironment Container

[![version-image]][release-url]
[![release-date-image]][release-url]
[![license-image]][license-url]

A development enviroment container that allows you to use a container as a full-featured development environment.

- GitHub: [https://github.com/entelecheia/devenv-container][repo-url]

The Development Enviroment Container seeks to find ways to develop, build, test, and deploy software in a containerized environment without the need to install any software on your local machine.

## Prerequisites

- Docker
- Docker Compose
- NVIDIA Docker (for GPU support, Optional)

## Build and Run

1. Build the base Docker image:

   ```bash
   make docker-build-base
   ```

   The base Docker image are used to build other Docker images. It installs all the dependencies required by the project.

   The `docker.base.env` file includes various configuration options and environment variables. The `docker-compose.base.yaml` file uses these variables to customize the behavior of the services. This is a common practice when you want to set different configurations for development, testing, and production environments. The `Dockerfile.base` file uses these variables to customize the Docker build. These files are automatically generated by Copier when you run the `copier copy` command.

2. Build the application Docker image:

   ```bash
   make docker-build-app
   ```

   The application Docker image is used to run the application. It installs the application and its dependencies.

   The `docker.app.env` file includes various configuration options and environment variables. The `docker-compose.app.yaml` file uses these variables to customize the behavior of the services. This is a common practice when you want to set different configurations for development, testing, and production environments. The `Dockerfile.app` file uses these variables to customize the Docker build. These files are automatically generated by Copier when you run the `copier copy` command.

3. Start the Docker container:

   ```bash
   make docker-up-app
   ```

   This will start a Docker container with the image built in the previous step. The container will run a bash lauch script that starts the application.

## Usage

After starting the container, you can access the application at `localhost:<APP_HOST_WEB_SVC_PORT>`. By default, the port is set to `18888`.

You can also SSH into the container using the SSH port specified in `APP_HOST_SSH_PORT`. By default, the port is set to `2222`.

## Volumes

The `docker-compose.app.yaml` file specifies several volumes that bind mount directories on the host to directories in the container. These include the cache, the workspace directory, and the scripts directory. Changes made in these directories will persist across container restarts.

## Troubleshooting

If you encounter any issues with this setup, please check the following:

- Make sure Docker and Docker Compose are installed correctly.
- Make sure NVIDIA Docker is installed if you're planning to use GPU acceleration.
- Ensure the environment variables in the `docker.app.env` file are correctly set.
- Check the Docker and Docker Compose logs for any error messages.

## Environment Variables

In Docker, environment variables can be used in the `docker-compose.app.yaml` file to customize the behavior of the services.

The `docker-compose` command has an `--env-file` argument, but it's used to set the environment variables for the services defined in the `docker-compose.yaml` file, not for the `docker-compose` command itself. The variables defined in the `--env-file` will not be substituted into the `docker-compose.yaml` file.

However, the environment variables we set in the `.docker/docker.app.env` file are used in the `docker-compose.app.yaml` file. For example, the `$BUILD_FROM` variable is used to set the base image for the Docker build. Therefore, we need to export these variables to the shell environment before running the `docker-compose` command.

This method also allows us to use shell commands in the variable definitions, like `"$(whoami)"` for the `USERNAME` variable, which wouldn't be possible if we used the `--env-file` argument. Shell commands in the `.env` file are not evaluated.

## Changelog

See the [CHANGELOG] for more information.

## Contributing

Contributions are welcome! Please see the [contributing guidelines] for more information.

## License

This project is released under the [MIT License][license-url].
<!-- Links: -->
[license-image]: https://img.shields.io/github/license/entelecheia/devenv-container
[license-url]: https://github.com/entelecheia/devenv-container/blob//LICENSE
[version-image]: https://img.shields.io/github/v/release/entelecheia/devenv-container?sort=semver
[release-date-image]: https://img.shields.io/github/release-date/entelecheia/devenv-container
[release-url]: https://github.com/entelecheia/devenv-container/releases
[repo-url]: https://github.com/entelecheia/devenv-container
[docs-url]: 
[changelog]: https://github.com/entelecheia/devenv-container/blob//CHANGELOG.md
[contributing guidelines]: https://github.com/entelecheia/devenv-container/blob//CONTRIBUTING.md
<!-- Links: -->
