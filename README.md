# Docker GitlabCI Runner PHP 5.5

Gitlab CI runner docker base images with ssh-key sharing.

## Base Image

This docker image is based on gitlabhq/gitlab-ci-runner image and provide a way to pass it an ssh-key or automatically
generate a new one.
This is a base image you can extend with your own stack.

### Build

In order to build it, you need to execute the following commands:

```
docker build -t gitlabhq/gitlab-ci-runner github.com/gitlabhq/gitlab-ci-runner
docker build -t bobey/docker-gitlab-ci-runner github.com/bobey/docker-gitlab-ci-runner
```

### Usage


```
docker pull bobey/docker-gitlab-ci-runner
```

Then, you can run as many runners as you want by executing:

```
docker run -e CI_SERVER_URL=https://ci.example.com -e REGISTRATION_TOKEN=replaceme -e HOME=/root -e GITLAB_SERVER_FQDN=gitlab.example.com bobey/docker-gitlab-ci-runner
```

If you need to pass an ssh key to the runner (a deploy key for example), use the following command:

```
docker run \
  -e CI_SERVER_URL=https://ci.example.com \
  -e REGISTRATION_TOKEN=replaceme -e HOME=/root \
  -e GITLAB_SERVER_FQDN=gitlab.example.com \
  -v /absolute/path/to/your/home/.ssh/id_rsa:/root/.ssh/id_rsa:ro \
  bobey/docker-gitlab-ci-runner
```

If you don't mount this optional volume, an ssh-key will be automatically generated and the public key will be displayed
on standard output.

If you need to start a bash inside your container, use the following command:

```
docker run -e CI_SERVER_URL=https://ci.example.com -e REGISTRATION_TOKEN=replaceme -e HOME=/root --rm -it bobey/docker-gitlab-ci-runner:latest /bin/bash
```

## PHP Image

A docker gitlab-ci runner containing the following stack:

- PHP 5.5
- Git
- Composer
- Xdebug

### Usage

You can run as many runners as you want by executing:

```
docker run \
  -e CI_SERVER_URL=https://ci.example.com \
  -e REGISTRATION_TOKEN=replaceme -e HOME=/root \
  -e GITLAB_SERVER_FQDN=gitlab.example.com \
  bobey/docker-gitlab-ci-runner-php
```

In your GitlabCI, a basic phpunit job could looks like this:

```
composer install
php vendor/phpunit/phpunit/phpunit --coverage-text
```

By displaying code coverage as text, you can easily extract code coverage metrics. In your project settings, under
"Test coverage parsing", just input the following regex: `  Lines:\s+(\d+.\d+\%)`.


### Development

This docker image is based on bobey/docker-gitlab-ci-runner image. In order to build it, you need to execute the following
command:

```
docker build -t bobey/docker-gitlab-ci-runner-php github.com/bobey/docker-gitlab-ci-runner/php
```

## NodeJS Image

A docker gitlab-ci runner containing the following stack:

- NodeJS
- NPM

### Usage

You can run as many runners as you want by executing:

```
docker run \
  -e CI_SERVER_URL=https://ci.example.com \
  -e REGISTRATION_TOKEN=replaceme -e HOME=/root \
  -e GITLAB_SERVER_FQDN=gitlab.example.com \
  bobey/docker-gitlab-ci-runner-nodejs
```

### Development

This docker image is based on bobey/docker-gitlab-ci-runner image. In order to build it, you need to execute the following
commands:

```
docker build -t bobey/docker-gitlab-ci-runner-php github.com/bobey/docker-gitlab-ci-runner/nodejs
```

## Gitlab CI setup

By starting Gitlab CI runners through Docker, assuming you passed a valid registration token, they will automatically
add themselves to your CI instance. You should see them in the "Runners" tab.
