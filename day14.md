# Day 14

* To understand what a container is, we first need to understand the problem it fixes:
    - Installation
    - Troubleshooting
    - Conflicts

* Containers share the host OS kernel, isolating only applications and their dependencies.

* Virtual machines are ideal for running multiple different operating systems or legacy applications, while containers excel at deploying scalable, portable micro-services.

* Microservices are a switch in the style of application architecture, where before it was a lot more common to deploy apps in a monolithic fashion (built as a single unit, a single code base, usually as a single executable ).

* A container engine is software that builds, runs, and manages containers by leveraging the host system’s OS kernel features like namespaces and cgroups.

* Docker is an open-source platform for developers to build, deploy, and manage containers.

* Containers are executable units of software which package and manage the software and components to run a service.

* A __container escape__ is a technique that enables code running inside a container to obtain rights or execute on the host kernel (or other containers) beyond its isolated environment (Example: creating a privileged container with access to the public internet from a test container with no internet access).

* Containers use a client-server setup on the host. The CLI tools act as the client, sending requests to the container daemon, which handles the actual container management and execution. The runtime exposes an API server via Unix sockets (runtime sockets) to handle CLI and daemon traffic. If an attacker can communicate with that socket from inside the container, they can exploit the runtime (this is how we would create the privileged container with internet access).

## Practical

* "tail -f /dev/null" as CMD for container to keep it running

* After logging in, check the socket access by running (inside container):
```sh
ls -la /var/run/docker.sock
```

* The Docker documentation mentions that by default, there is a setting called “Enhanced Container Isolation” which blocks containers from mounting the Docker socket to prevent malicious access to the Docker Engine. In some cases, like when running test containers, they need Docker socket access. The socket provides a means to access containers via the API directly.