# Non-Root Containers

There are two types of container images: root and non-root. Non-root images add an extra layer of security and are generally recommended for production environments. However, because they run as a non-root user, privileged tasks such as installing system packages, editing configuration files, creating system users and groups, and modifying network information, are typically off-limits.

This guide gives you a quick introduction to non-root container images, explains possible issues you might face using them, and also shows you how to modify them to work as root images.

## Advantages Of Non-Root Containers

Non-root containers are recommended for the following reasons:

* Security: Non-root containers are automatically more secure. If there is a container engine security issue, running the container as an unprivileged user will prevent any malicious code from gaining elevated permissions on the container host. To learn more about Docker’s security features, see this guide.

* Platform restrictions: Some Kubernetes distributions (such as OpenShift) run containers using random UUIDs. This approach is not compatible with root containers, which must always run with the root user’s UUID. In such cases, root-only container images will simply not run and a non-root image is a must.

## Disadvantages Of Non-Root Containers

Non-root containers also have some disadvantages when used for local development:

* Failed writes on mounted volumes: Docker mounts host volumes preserving the host UUID and GUID. This can lead to permission conflicts with non-root containers, as the user running the container may not have the appropriate privileges to write to the host volume.

* Failed writes on persistent volumes in Kubernetes: Data persistence in Kubernetes is configured using persistent volumes. Kubernetes mounts these volumes with the root user as the owner; therefore, non-root containers don’t have permissions to write to the persistent directory.

* Issues with specific utilities or services: Some utilities (eg. Git) or servers (eg. PostgreSQL) run additional checks to find the user in the /etc/passwd file. These checks will fail for non-root container images.

## Using Non-Root Containers As Root Containers

If you wish to run a non-root container image as a root container image, you can do so by adding the line `user: root` right after the `image:` directive in the container’s docker-compose.yml. After making this change, simply restart the container and it will run as the root user with all privileges instead of an unprivileged user.

## This repository

This repository contains of some Dockerfile that are non-root containers from the original Docker images.
