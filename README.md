# Guide for Making Your Own Docker Images Work on Resin

## Introduction

Resin.io offers you the flexibility to deploy your application from a custom Dockerfile which allows you to define your own development environment. 

To make an image works on resin, it needs to embed QEMU because we do cross-compiling on our servers to build your images.

Follow each step in this guide and you will find how easy to make an image work on resin

## Steps

In this example, we use [sdhibit/rpi-raspbian](https://registry.hub.docker.com/u/sdhibit/rpi-raspbian/dockerfile/) as the target image.

* Rebuild your image with qemu embedded.
```sh
FROM sdhibit/rpi-raspbian

COPY qemu-arm-static /usr/bin/
```
And

`docker build -t nghiant2710/sdhibit-rpi-raspbian-qemu .`

Replace `sdhibit/rpi-raspbian` with your image name and `nghiant2710/sdhibit-rpi-raspbian-qemu` with you new image name then build this Dockerfile, the new image will have QEMU embedded.
We highly recommend using the `qemu-arm-static` in this repo for your image. This is the resin's patched QEMU for your stability your application.

* Push you new image
`docker push nghiant2710/sdhibit-rpi-raspbian-qemu`

Replace `nghiant2710/sdhibit-rpi-raspbian-qemu` with your new image name and push it to Docker Hub

* Push your application using the newly built image
```sh
FROM nghiant2710/sdhibit-rpi-raspbian-qemu

RUN apt-get update && apt-get install -y python

COPY . /

CMD ["python", "/hello.py"]
```

We've made a simple appication using the newly created image. What will you create?
