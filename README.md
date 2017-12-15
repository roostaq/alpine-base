[![Build Status](https://travis-ci.org/woahbase/alpine-base.svg?branch=master)](https://travis-ci.org/woahbase/alpine-base)

[![](https://images.microbadger.com/badges/image/woahbase/alpine-base.svg)](https://microbadger.com/images/woahbase/alpine-base)

[![](https://images.microbadger.com/badges/commit/woahbase/alpine-base.svg)](https://microbadger.com/images/woahbase/alpine-base)

[![](https://images.microbadger.com/badges/version/woahbase/alpine-base.svg)](https://microbadger.com/images/woahbase/alpine-base)

## Alpine-Base
#### Container for Alpine Linux Base Builds

---

This [image][5] serves as the base rootfs container for [Alpine Linux][8].
Built from scratch using the minirootfs image from [here][9].

The image is tagged respectively for 2 architectures,
* **armhf**
* **x86_64**

**armhf** builds have embedded binfmt_misc support and contain the
[qemu-user-static][7] binary that allows for running it also inside
an x64 environment that has it.

---
#### Get the Image
---

Pull the image for your architecture it's already available from
Docker Hub.

```
# make pull
docker pull woahbase/alpine-base:x86_64

```

---
#### Run
---

If you want to run images for other architectures, you will need
to have binfmt support configured for your machine. [**multiarch**][6],
has made it easy for us containing that into a docker container.

```
# make regbinfmt
docker run --rm --privileged multiarch/qemu-user-static:register --reset

```
Without the above, you can still run the image that is made for your
architecture, e.g for an x86_64 machine..

```
# make
docker run --rm -it \
  --name docker_base --hostname base \
  woahbase/alpine-base:x86_64

# make stop
docker stop -t 2 docker_base

# make rm
# stop first
docker rm -f docker_base

# make restart
docker restart docker_base

```

---
#### Shell access
---

```
# make rshell
docker exec -u root -it docker_base /bin/bash

# make shell
docker exec -it docker_base /bin/bash

# make logs
docker logs -f docker_base

```

---
## Development
---

If you have the repository access, you can clone and
build the image yourself for your own system, and can push after.

---
#### Setup
---

Before you clone the repo, you must have [Git][1], [GNU make][2],
and [Docker][3] setup on the machine.

```
git clone https://github.com/woahbase/alpine-base
cd alpine-base

```
You can always skip installing **make** but you will have to
type the whole docker commands then instead of using the sweet
make targets.

---
#### Build
---

You need to have binfmt_misc configured in your system to be able
to build images for other architectures.

Otherwise to locally build the image for your system.

```
# make ARCH=x86_64 build
# fetches minirootfs inside ./data/
# sets up binfmt if not x86_64
docker build --rm --force-rm \
  --no-cache=true --pull \
  -f ./Dockerfile_x86_64 \
  -t woahbase/alpine-base:x86_64 \
  --build-arg BUILD_DATE=2017-12-15T19:42:28Z \
  --build-arg VCS_REF=$(shell git rev-parse --short HEAD)

# make ARCH=x86_64 test
docker run --rm -it \
  --name docker_base --hostname base \
  woahbase/alpine-base:x86_64 \
  bash --version

# make ARCH=x86_64 push
docker push woahbase/alpine-base:x86_64

```

---
## Maintenance
---

Built daily at Travis.CI (armhf / x64 builds). Docker hub builds maintained by [woahbase][4].

[1]: https://git-scm.com
[2]: https://www.gnu.org/software/make/
[3]: https://www.docker.com
[4]: https://hub.docker.com/u/woahbase

[5]: https://hub.docker.com/r/woahbase/alpine-base
[6]: https://hub.docker.com/r/multiarch/qemu-user-static/
[7]: https://github.com/multiarch/qemu-user-static/releases/
[8]: https://alpinelinux.org/
[9]: http://dl-4.alpinelinux.org/alpine/latest-stable/releases/
