### Docker images

A Docker image is a complete application package. It contains one application and all of its
dependencies, the language runtime, the application host, and the underlying operating
system

### Docker containers

A container is an instance of an application created from an image. The image contains the whole application stack, and it also specifies the process to start the application, so Docker knows what to do when you run a container.

You start your application with docker container run, specifying the name of the image and your configuration options.

Docker provides the container runtime as well as image packaging and distribution. In a small environment and in development, you will manage individual containers on a single Docker host, which would be your laptop or a test server. When you move to production, you'll need high availability and the option to scale, and that comes with Docker swarm.

### Docker swarm

Docker has the ability to run on a single machine or as one node in a cluster of machines all running Docker. This cluster is called a swarm, and you don't need to install anything extra to run in swarm mode. You install Docker on a set of machines, and on the first you run `docker swarm init` to initialize the swarm, and on the others you run `docker swarm join` to join the swarm.
