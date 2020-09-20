# postgis/postgis

Adds ARM support for deploying to RPi clusters

As of late September 2020, postgis debian (stable) apt package does not have support for ARM architectures.

However, the next debian release Bullseye does. This project stitches a solution using the following projects:
- [docker-library/postgres](https://github.com/docker-library/postgres)
- [postgis/docker-postgis](https://github.com/postgis/docker-postgis)

## Compiling for MacOS

In a terminal, check you can cross-compile by running:

`docker buildx inspect --bootstrap`

You should see the following output:

```
Name:   qemu
Driver: docker-container

Nodes:
Name:      qemu0
Endpoint:  unix:///var/run/docker.sock
Status:    running
Platforms: linux/amd64, linux/arm64, linux/ppc64le, linux/s390x, linux/386, linux/arm/v7, linux/arm/v6
```

If you do not, add these env variables to you `~/.bash_profile` (or `~/.zshrc` if you use that):

```
export DOCKER_BUILD_KIT=1
export DOCKER_CLI_EXPERIMENTAL=enabled
```

then run `source ~/.bash_profile` (or `~/.zshrc` if you use that).

Add the qemu buildx configuration with:

`docker buildx create --use --name=qemu`

Try the first command again to finish the setup:

`docker buildx inspect --bootstrap`

Now, in the `config.env` file in the root of the project, change `REGISTRY_DOMAIN_OR_DOCKERHUB_USERNAME` value to be
your Docker Hub username (e.g. mine is `awill88`).

Finally, run `make buildx` to build and push the multi-architecture layers to Docker Hub.

You can also easily change the `REGISTRY_DOMAIN_OR_DOCKERHUB_USERNAME` to be another registry address.

### Still Doesn't Work?

Read this article: [Preparation toward running Docker on ARM Mac: Building multi-arch images with Docker BuildX](https://medium.com/nttlabs/buildx-multiarch-2c6c2df00ca2)
