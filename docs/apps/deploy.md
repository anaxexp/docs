# Code deployment 

## Direct git deployments

!!! info "Only for Drupal and WP"
    Direct git deployment is available only for [Drupal](../stacks/drupal/index.md) and [WordPress](../stacks/wordpress/index.md) stacks and their forks

You can connect your git repository to AnaxExp and use it as a codebase source for your applications. On code deployment we will perform pull from the target branch and run [post-deployment scripts](post-deployment-scripts.md) (if enabled). 

Additionally, you can run deployment automatically every time you push code to a git branch (all instances using this branch will be deployed):

1. Configure git hooks for your git repository 
2. Navigate to `Instance > Deployment > Settings` and check `Automatic deploy` option

For details instructions how to connect a repository and configure hooks see the following articles:

* [GitHub](../integrations/github.md) 
* [BitBucket](../integrations/bitbucket.md) 
* [GitLab](../integrations/gitlab.md) 
* [Custom git provider](../integrations/custom.md) 

## CI/CD

Sometimes direct git integration may not be enough for a few reasons:

* Build stage is a must have for repositories with dependencies (e.g. npm, composer)
* You need to run tests
* Direct git deployment cannot be used for custom stacks and cluster deployments
* With CI/CD you have build artifacts like docker images that you can download locally
* With CI/CD you can rollback build (feature TBA)

### AnaxExp CI

!!! tip "Coming soon"
    We plan to replace direct git integration with our built-in CI system

### Via third-party CI

You can set up CI/CD workflow for your application by integrating AnaxExp with third-party CI tools. Build can be performed on any CI tools with AnaxExp CLI. 

Big picture:

1. Deploy an app based on a managed stack that supports CI deployments or custom stack with at least one service having [`deployment.type=ci`](../stacks/template.md#deployment)
2. [Get](#anaxexp-cli) AnaxExp CLI tool or its docker image
3. [Initialize](#init) the build by providing your AnaxExp API key and UUID of your app instance
4. [Build](#build) images with your codebase. Images will be based on the images from your stack
5. Push ([release](#release)) images to a private docker registry we provide you (or any other registry)
6. [Deploy](#deploy) the build (a set of images) to your app instance

#### AnaxExp CLI

!!! tldr "VM-based builds over docker-in-docker"
    If your CI tool can run builds both in Docker and Virtual Machine (docker daemon must be available) we recommend using the latter because it's faster

You can install the latest stable AnaxExp CLI (Linux amd64) tool during the build like this:

```shell
wget -qO- https://api.anaxexp.com/api/v1/get/cli | sh
```

If you want to install it locally for other systems such as macOS or Windows, or install a specific version follow the instructions at https://github.com/anaxexp/anaxexp-cli

Or you can use [`anaxexp/anaxexp-cli`](https://hub.docker.com/r/anaxexp/anaxexp-cli/) docker image if your CI supports only docker-based builds

#### Init

```shell
anaxexp ci init [INSTANCE UUID]
```

This command will gather build information about your instance such as services (images) that can be built and private docker registry credentials. All builds must start with the init. To perform this step you must have your [AnaxExp API key](../dev.md#api-keys) exported as `$ANAXEXP_API_KEY` or provided via `--api-key`. Make sure the key is secured and not exposed to public. 

#### Build

!!! tldr "Services available during the build"
    If you're building a managed stack, the list of services eligible for the build is hardcoded and you can find it in [a stack documentation](../stacks). If you're building a custom stack, all services that have [`deployment.type=ci`](../stacks/template.md#deployment) will be available
    
During the build stage you can prepare your codebase for the build by running `anaxexp ci run` which is basically a wrapper of `docker run`. 

You can either specify a docker image that runs a command:

```shell
anaxexp ci run -i anaxexp/node:8 -- yarn install
anaxexp ci run -i anaxexp/node:9 -p path/to/frontend -- yarn install
```

Or specify a service name from your stack, the image of your current stack version will be used:

```shell
anaxexp ci run -s backend -- composer install --prefer-dist -n -o --no-dev
```

You can use bind mounts on libraries cache directories to utilize caching capabilities of your CI tool (`.travis.yml` example):

```yml
cache:
  directories:
    - /home/travis/.composer/cache

script:
  - anaxexp ci run -v $HOME/.composer:$HOME/.composer -s php -- composer install
```

Once the codebase is ready you can run the build via `anaxexp ci build` which is a wrapper of `docker build`. By default the build command builds a new image based on the image of a service you specified, and copies codebase (contents of the current directory, same as `--from \.`) to service's image default working directory:

```shell
# Build all ci services' images
anaxexp ci build 
# Same thing
anaxexp ci build --from \.
# Build php service image 
anaxexp ci build php
# Build all images of services starting with node-
anaxexp ci build node-*
# Build php service image with the contents from ./build directory
anaxexp ci build php --from ./build
# Build node service image with the contents from ./build directory to /usr/src/app directory inside node image 
anaxexp ci build node --from ./build --to /usr/src/app
```

Or you can build from your own `Dockerfile`:

```shell
anaxexp ci build --dockerfile /path/to/my/Dockerfile
```

If you're using custom `Dockerfile` make sure it starts with the following lines to make sure it will be based on the image from your stack: 

```
ARG ANAXEXP_BASE_IMAGE
FROM ${ANAXEXP_BASE_IMAGE}
```

By default we build images with the name (tag) of a private docker registry we provide. But you can use your own registry:

```shell
anaxexp ci build -t my-private-docker-hub/repository
``` 

#### Release

!!! tldr "Docker registry"
    AnaxExp provides a private docker registry `registry.anaxexp.com` which used by default. You can use custom docker registry during the build but if it's a private one make sure to add the appropriate [docker registry integration](../integrations/docker-registry.md) so servers where you deploy instances can access your images.

!!! question "How to download images?"
    Once you deployed your first build you can find images' URLs on `Instance > Stack` page. You can get those images locally by running `docker login registry.anaxexp.com` and entering your AnaxExp user's email/password.  

Once images are built, you can push them to a docker registry:

```shell
# Push all images to the default docker registry 
anaxexp ci release
# Push images of specific services 
anaxexp ci release php node
# Push to a custom docker registry
anaxexp ci release -t my-private-docker-hub/repository
# Additionally push with the tag of the current git branch name 
anaxexp ci release -t my-private-docker-hub/repository -b
``` 

#### Deploy

```shell
# Deploy all services from the default docker registry
anaxexp ci deploy
# Deploy specific services
anaxexp ci deploy php crond
# Deploy all services from a custom docker registry
anaxexp ci deploy -t my-private-docker-hub/repository
```

#### Examples

You can find build examples for different CI services such as CircleCI, TravisCI, BitBucket pipelines and custom shell scripts at https://github.com/anaxexp/anaxexp-ci 
