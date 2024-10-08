# Description

Docker Debian based Images with dependencies installed, ready to be used
to build Kamailio from sources

# Upgrade

You need the kamailio sources at _src_ so get them:

```
git clone https://github.com/kamailio/kamailio.git src
```

or refresh them:

```
make pull
```

and just
```
make
```

# build locally the image
for instance:
```
export DIST=bookworm VERSION=5.7
```
```
cd ${DIST}; docker build --tag=pkg-kamailio-docker:${VERSION}-${DIST} .
```

or pull the image from Github Packages

```
docker pull ghcr.io/kamailio/pkg-kamailio-docker:${VERSION}-${DIST}
```
# run container

If you built the docker image locally:
```
IMAGE=pkg-kamailio-docker
```
or if you've pulled it from Github Packages:
```
IMAGE=ghcr.io/kamailio/pkg-kamailio-docker
```


Then, in all cases, go to the root folder of your kamailio source code and launch the container:
```
docker run -i -t --rm -v `pwd`:/code:rw ${IMAGE}:${VERSION}-${DIST} /bin/bash
```

Softlink to the .a folders
```
ln -s /usr/lib/x86_64-linux-gnu/ /usr/lib64
```

To build kamailio against Debian stable using that container, run:
```
cd /code
make deb-stable
```
