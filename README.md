# Tags and respective `Dockerfile` links

- [`1.15.32`, `1.15`, `latest` *(1.15.32/Dockerfile)*](https://github.com/nbrownuk/docker-aws-cli/blob/master/Dockerfile)

[![](https://images.microbadger.com/badges/image/nbrown/aws-cli.svg)](https://microbadger.com/images/nbrown/aws-cli "View on microbadger.com")
[![](https://images.microbadger.com/badges/version/nbrown/aws-cli.svg)](https://microbadger.com/images/nbrown/aws-cli "View on microbadger.com")

# What is aws-cli?

[aws-cli](https://github.com/aws/aws-cli) is the Universal Command Line Interface for [Amazon Web Services](https://aws.amazon.com/).

# How to use this image

## Executing aws-cli commands

In order to execute aws-cli commands, simply supply the command and any subcommands as arguments when invoking a container, e.g.

```
$ docker container run -it --rm nbrown/aws-cli ec2 describe-instances
```

It makes a lot of sense to create an alias for the command, instead:

```
$ alias aws='docker container run -it --rm nbrown/aws-cli "$@"'
```

The aws-cli requires credentials, which can be provided using environment variables, passed to the container at runtime, using the `--env` configuration option, but it is better to mount `$HOME/.aws` inside the container instead, so that `$HOME/.aws/config` or `$HOME/.aws/credentials` are available to the aws-cli:

```
$ docker container run -it --rm -v $HOME/.aws:/home/aws/.aws nbrown/aws-cli ec2 describe-instances
```

**Please Note:** the user is set to `aws`, with a default UID of `1000`. If the user on the host has a different UID, the `aws` user in the container will not be able to read the mounted credentials. In this scenario, build a custom version of the image using a build argument to specify the UID;

```
$ docker image build --build-arg UID=2000 --build-arg VERSION="1.15.329" -t aws-cli
```

## Building the image

If building the image from the Dockerfile, be sure to use a build argument to specify the version of the aws-cli to install; e.g.

```
$ docker image build --build-arg VERSION="1.15.329" -t aws-cli
```

To find the latest version available, check the [Package Index for awscli](https://pypi.python.org/pypi/awscli)
