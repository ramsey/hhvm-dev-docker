This repository contains the sources for the following Docker Hub images:

 - [benramsey/hhvm-dev](https://registry.hub.docker.com/u/benramsey/hhvm-dev/)

The benramsey/hhvm-dev image differs from the official [hhvm/hhvm](https://hub.docker.com/r/hhvm/hhvm/) images in that it contains the `hhvm-dev` package, which includes tools like `hphpize` for building HHVM extensions.

Building A New Version
======================

When a new version of HHVM is released:

Update The Base Image
---------------------

This is built on top of `ubuntu:14.04`, so make sure you're building against
the latest version of that:

```
$ docker pull ubuntu:14.04
```

Change The benramsey/hhvm-dev Version Number
--------------------------------------------

For example, for 3.11.0 => 3.12.0:

```diff
diff --git a/hhvm-dev-latest/Dockerfile b/hhvm-dev-latest/Dockerfile
index 185d896..17644e0 100644
--- a/hhvm-dev-latest/Dockerfile
+++ b/hhvm-dev-latest/Dockerfile
@@ -4,4 +4,4 @@ RUN apt-get update -y
 RUN apt-get install -y software-properties-common
 RUN apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0x5a16e7281be7
 RUN add-apt-repository "deb http://dl.hhvm.com/ubuntu trusty main"
-RUN apt-get update -y && apt-get install -y hhvm-dev=3.11.0~trusty
+RUN apt-get update -y && apt-get install -y hhvm-dev=3.12.0~trusty
```

Build And Tag benramsey/hhvm-dev
--------------------------------

The ID will not match; be sure to tag your new image ID instead of copying
the one in this example.

```
$ docker build hhvm-dev-latest/
...
Successfully built ba93944aeef2
$ docker tag ba93944aeef2 benramsey/hhvm-dev:latest
$ docker tag ba93944aeef2 benramsey/hhvm-dev:3.12.0
```

Push To DockerHub
-----------------

```
$ docker push benramsey/hhvm-dev
The push refers to a repository [benramsey/hhvm-dev] (len: 1)
a263d6b57a5c: Image already exists
340a5b1399f2: Image successfully pushed
0c66775c4b88: Image successfully pushed
7dc759deb8e7: Image successfully pushed
8693db7e8a00: Image successfully pushed
a4c5be5b6e59: Image successfully pushed
c4fae638e7ce: Image successfully pushed
f15ce52fc004: Image successfully pushed
Digest: sha256:83de7b6a572550f565f9599952a0a3856536cb5e13f5e85646dc979ec5c61440
```

Push Updated Sources
--------------------

```
$ git commit -m 'v3.12.0'
$ git push
```