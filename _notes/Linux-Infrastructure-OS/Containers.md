[[Index]] 

## Container Management Tools

Red Hat Enterprise Linux provides a set of container tools that you can use to run containers in a
single server.

• podman manages containers and container images.

• skopeo inspects, copies, deletes, and signs images.

• buildah creates container images.

These tools are compatible with the Open Container Initiative (OCI). With these tools, you
can manage any Linux containers that are created by OCI-compatible container engines, such
as Podman or Docker. These tools are specifically designed to run containers under Red Hat
Enterprise Linux on a single-node container host.
In this chapter, you use the podman and skopeo utilities to run and manage containers and
existing container images. 

-   [Finding Images]()
-   [Building Images]()
-   [Running Containers on Images]()
-   [Working with Container Processes]()
-   [Working with the Container Filesystem]()
-   [Removing Images]()
-   [Miscellaneous]()

For more information about podman, visit the [Red Hat Developer website](https://developers.redhat.com/). This cheat sheet was written by Doug Tidwell, with huge thanks to Dan Walsh and Scott McCarty.

In the following `container` is either a container name or a container ID. If `tag` is omitted in image:tag , the default value is latest.

## Finding Images[]( "Direct link to Finding Images")

| Command | Description |
| --- | --- |
| podman images | List all local images |
| podman history image:tag | Display information about how an image was built |
| podman login registryURL -u username \[-p password\] | Log in to a remote registry |
| podman pull registry/username/image:tag | Pull an image from a remote registry |
| podman search searchString | Search local cache and remote registries for images |
| podman logout | Log out of the current remote registry |

_The list of registries is defined in `/etc/containers/registries.conf`_

## Building Images[]( "Direct link to Building Images")

| Command | Description |
| --- | --- |
| podman build -t image:tag . | Build and tag an image using the instructions in Docker?le in the |
| current directory |  |
| podman build -t image:tag -f Dockerfile2 | Same as above, but with a di?erent Docker?le |
| podman tag image:tag image:tag2 | Add an additional name to a local image |
| podman tag image:tag registry/username/image:tag | Same as above, but the additional name includes a remote registry |
| podman push registry/username/image:tag | Push an image to a remote registry |

## Running Containers on Images[]( "Direct link to Running Containers on Images")

| Command | Description |
| --- | --- |
| podman run --rm -it \[--name name\] image:tag command | Run a container based on a given image. |

-   `--rm` Remove the container after it exits
-   `-it` Connect the container to the terminal
-   `--name` name Give the container a name
-   `image:tag` The image used to create the container
-   `command` A command to run (/bin/bash for example)
-   `-d` Run the container in the background
-   `-p 8080:32000` Expose container port 8080 as localhost:32000
-   `-v /var/lib/mydb:/var/lib/db` Map the /var/lib/mydb directory on localhost to a volume named /var/lib/db inside the container

| Command | Description |
| --- | --- |
| podman commit container newImage:tag | Create a new image based on the current state of a running container |
| podman create \[--name name\] image:tag | Create (but don’t start) a container from an image |
| podman start container | Start an existing container from an image |
| podman restart container | Restart an existing container |
| podman wait container1 \[container2… \] | Wait on one or more containers to stop |
| podman stop container | Stop a running container gracefully |
| podman kill container | Send a signal to a running container |
| podman rm \[-f\] container | Remove a container (use -f if the container is running) |
| podman stats container | Display a live stream of a container’s resource usage |
| podman inspect container | Return metadata (in JSON) about a running container |

## Working with Container Processes[]( "Direct link to Working with Container Processes")

| Command | Description |
| --- | --- |
| podman ps \[--all\] | List the running containers on the system (use --all to include non- |
| running containers) |  |
| podman attach container | Attach to a running container and view its output or control it |
| \+ + detaches from the container but leaves it running. |  |
| podman exec container command | Execute a command in a running container |
| podman top container | Display the running processes of a container |
| podman logs \[-tail\] container | Display the logs of a container |
| podman pause container / podman unpause container | Pause/unpause all the processes in a container |
| podman port container | List the port mappings from a container to localhost |

## Working with the Container Filesystem[]( "Direct link to Working with the Container Filesystem")

| Command | Description |
| --- | --- |
| podman diff container | Display all the changes to a container’s ?lesystem |
| podman cp source target | Copy ?les and folders between a container and localhost |
| podman mount container / podman umount container | Mount or unmount a container’s root ?lesystem |
| podman import tarball | Import a tarball and save it as a ?lesystem image |
| podman export \[-o outputFile\] container | Export the container’s ?lesystem to a tar ?le |
| podman save \[-o archiveFile\]\--format docker-archive | oci-archive |
| podman load -i archiveFile | Load a saved image from docker-archive or another format |

## Removing Images[]( "Direct link to Removing Images")

| Command | Description |
| --- | --- |
| podman rmi \[-f\] image:tag | Remove a local image from local cache (use -f to force removal) |
| podman rmi \[-f\] registry/username/image:tag | Remove a remote image from local cache (use -f to force removal) |

## Miscellaneous[]("Direct link to Miscellaneous")

| Command | Description |
| --- | --- |
| podman version | Display podman version information |
| podman info | Display information about the podman environment |

## Container Images and Registries

To run containers, you must use a container image. A container image is a static file that contains
codified steps, and it serves as a blueprint to create containers. The container images package an
application with all its dependencies, such as its system libraries, programming language runtimes
and libraries, and other configuration settings.

Container images are built according to specifications, such as the Open Container Initiative (OCI)
image format specification. These specifications define the format for container images, as well
as the metadata about the container host operating systems and hardware architectures that the
image supports 

A container registry is a repository for storing and retrieving container images. A developer pushes
or uploads container images to a container registry. You can pull or download container images
from a registry to a local system to run containers.

You might use a public registry that contains third-party images, or you might use a private
registry that your organization controls. The source of your container images matters. As with any
other software package, you must know whether you can trust the code in the container image.
Policies vary between registries about whether and how they provide, evaluate, and test container
images that are submitted to them.

Red Hat distributes certified container images through two main container registries that you can
access with your Red Hat login credentials.

• registry.redhat.io for containers that are based on official Red Hat products.

• registry.connect.redhat.com for containers that are based on third-party products

You need a Red Hat Developer account to download an image from the Red Hat registries. You
can use the podman login command to authenticate to the registries. If you do not provide a
registry URL to the podman login command, then it authenticates to the default configured
registry.

```bash
[user@host ~]$ podman login registry.lab.example.com
Username: RH134
Password: EXAMPLEPASSWORD
Login Succeeded!
```



You can also use the podman login command --username and --password-stdin options,
to specify the user and password to log in to the registry. The --password-stdin option reads
the password from stdin. Red Hat does not recommend to use the --password option to provide
the password directly, as this option stores the password in the log files.

```bash
[user@host ~]# echo $PASSWORDVAR | podman login --username RH134 \
--password-stdin registry.access.redhat.com
```


To verify that you are logged in to a registry, use the podman login command --get-login
option.

```bash
[user01@rhel-vm ~]$ podman login registry.access.redhat.com --get-login 

[user01@rhel-vm ~]$ podman login quay.io --get-login
Error: not logged into quay.io
```

In the preceding output, the podman utility is authenticated to the
registry.access.redhat.com registry with the RH134 user credentials, but the podman
utility is not authenticated to the quay.io registry.
Configure Container Registries
The default configuration file for container registries is the /etc/containers/
registries.conf file.
[user@host ~]$ cat /etc/containers/registries.conf
...output omitted...
[registries.search]
registries = ['registry.redhat.io', 'quay.io', 'docker.io']
# If you need to access insecure registries, add the registry's fully-qualified
 name.
# An insecure registry is one that does not have a valid SSL certificate or only
 does HTTP.
[registries.insecure]
registries = []
...output omitted...
Because Red Hat recommends to use a non-privileged user to manage containers, you can
create a registries.conf file for container registries in the $HOME/.config/containers
directory. The configuration file in this directory overrides the settings in the /etc/containers/
registries.conf file.
The list of registries to look for containers is configured in the [registries.search] section of
this file. If you specify the fully qualified name of a container image from the command line, then
the container utility does not search in this section.
Insecure registries are listed in the [registries.insecure] section of the registries.conf
file. If a registry is listed as insecure, then connections to that registry are not protected with
TLS encryption. If a registry is both searchable and insecure, then it can be listed in both
[registries.search] and [registries.insecure].


## Container Files to Build Container Images

A container file is a text file with the instructions to build a container image. A container file usually
has a context that defines the path or URL where its files and directories are located. The resulting
container image consists of read-only layers, where each layer represents an instruction from the
container file.

The following output is an example of a container file that uses the UBI image from the
registry.access.redhat.com registry, installs the python3 package, and prints the hello
string to the console. 

[user@host ~]$ cat Containerfile
FROM registry.access.redhat.com/ubi8/ubi:latest
RUN dnf install -y python3
CMD ["/bin/bash", "-c", "echo hello"]


## Container Management at Scale

New applications increasingly use containers to implement functional components. Those
containers provide services that other parts of the application consume. In an organization,
managing a growing number of containers might quickly become an overwhelming task.
Deploying containers at scale in production requires an environment that can adapt to the
following challenges: 

• The platform must ensure the availability of containers that provide essential services.

• The environment must respond to application usage spikes by increasing or decreasing the
number of running containers and load balancing the traffic.

• The platform must detect the failure of a container or a host and react accordingly.

• Developers might need an automated workflow to deliver new application versions transparently
and securely.

Kubernetes is an orchestration service that deploys, manages, and scales container-based
applications across a cluster of container hosts. Kubernetes redirects traffic to your containers
with a load balancer, so that you can scale the number of containers that provide a service.
Kubernetes also supports user-defined health checks to monitor your containers and to restart
them if they fail.

Red Hat provides a distribution of Kubernetes called Red Hat OpenShift. Red Hat OpenShift is a
set of modular components and services that are built on top of the Kubernetes infrastructure. It
provides additional features, such as remote web-based management, multitenancy, monitoring
and auditing, advanced security features, application lifecycle management, and self-service
instances for developers. 

## Deploy Containers

Objectives
Discuss container management tools for using registries to store and retrieve images, and for
deploying, querying, and accessing containers. 

## The Podman Utility 

Podman is a fully featured container engine from the container-tools meta-package to
manage Open Container Initiative (OCI) containers and images. The podman utility does not use
a daemon to function, and so developers do not need a privileged user account on the system to
start or stop containers. Podman provides multiple subcommands to interact with containers and
images. The following list shows subcommands that are used in this section: 

## Podman Commands  

## Command Description 

```bash 
podman-build : Build a container image with a container file.
podman-run   : Run a command in a new container.
podman-images: List images in local storage.
podman-ps    : Print information about containers.
podman-inspect : Display configuration of a container, image, volume, network, or pod.
podman-pull    : Download an image from a registry.
podman-cp      : Copy files or folders between a container and the local file system.
podman-exec    : Execute a command in a running container.
podman-rm      : Remove one or more containers.
podman-rmi     : Remove one or more locally stored images.
podman-search  : Search a registry for an image.


```




To cover the topics in this lecture, imagine the following scenario.
As a system administrator, you are tasked to run a container that is based on the RHEL 8 UBI
container image called python38 with the python-38 package. You are also tasked to create a
container image from a container file and run a container called python36 from that container
image. The container image that is created with the container file must have the python36:1.0
tag. Identify the differences between the two containers. Also ensure that the installed python
packages in the containers do not conflict with the installed Python version in your local machine 

## Install Container Utilities 

The container-tools meta-package contains required utilities to interact with containers
and container images. To download, run, and compare containers on your system, you install
the container-tools meta-package with the dnf install command. Use the dnf info
command to view the version and contents of the container-tools package. 

```bash 
[root@host ~]# dnf install container-tools
...output omitted...
[user@host ~]$ dnf info container-tools
...output omitted...

Summary : A meta-package witch container tools such as podman, buildah,
 : skopeo, etc.
License : MIT
Description : Latest versions of podman, buildah, skopeo, runc, conmon, CRIU,
 : Udica, etc as well as dependencies such as container-selinux
 : built and tested together, and updated.
...output omitted... 
```



The container-tools meta-package provides the needed podman and skopeo utilities to
achieve the assigned tasks. 

## Download a Container Image from a Registry 

First, you ensure that the podman utility is configured to search and download containers from the
registry.redhat.io registry. The podman info command displays configuration information
of the podman utility, including the configured registries. 

```bash 
[user@host ~]$ podman info
...output omitted...
insecure registries:
 registries: []
registries:
 registries:
 - registry.redhat.io
 - quay.io
 - docker.io
...output omitted... 


```

The podman search command searches for a matching image in the list of configured registries.
By default, Podman searches in all unqualified-search registries. Depending on the Docker
distribution API that is implemented with the registry, some registries might not support the
search feature. 

Use the podman search command to display a list of images on the configured registries that
contain the python-38 package 

```bash 
[user@host ~]$ podman search python-38
NAME DESCRIPTION
registry.access.redhat.com/ubi7/python-38 Python 3.8 platform for building and
 running applications
registry.access.redhat.com/ubi8/python-38 Platform for building and running
 Python 3.8 applications
...output omitted...

```


The registry.access.redhat.com/ubi8/python-38 image seems to match the criteria for
the required container. 

You can use the skopeo inspect command to examine different container image formats
from a local directory or a remote registry without downloading the image. This command output
displays a list of the available version tags, exposed ports of the containerized application, and
metadata of the container image. You use the skopeo inspect command to verify that the
image contains the required python-38 package.

```
[user@host ~]$ skopeo inspect docker://registry.access.redhat.com/ubi8/python-38
{
"Name": "registry.access.redhat.com/ubi8/python-38",
"Digest":
"sha256:c6e522cba2cf2b3ae4a875d5210fb94aa1e7ba71b6cebd902a4f4df73cb090b8",
"RepoTags": [
...output omitted...
"1-68",
"1-77-source",
"latest"
...output omitted...
"name": "ubi8/python-38",
"release": "86.1648121386",
"summary": "Platform for building and running Python 3.8 applications",
...output omitted...
```
The registry.access.redhat.com/ubi8/python-38 image contains the required package
and it is based on the required image. You use the podman pull command to download the
selected image to the local machine. You can use the fully qualified name of the image from the
preceding output to avoid ambiguity on container versions or registries.

```
[user@host ~]$ podman pull registry.access.redhat.com/ubi8/python-38
Trying to pull registry.access.redhat.com/ubi8/python-38:latest...
Getting image source signatures
Checking if image destination supports signatures
Copying blob c530010fb61c done
...output omitted...
```
Then, you use the podman images command to display the local images.

```
[user@host ~]$ podman images
REPOSITORY TAG IMAGE ID CREATED SIZE
registry.access.redhat.com/ubi8/python-38 latest a33d92f90990 1 hour ago 901 MB
```



**Create a Container Image from a Container File**

You are provided with the following container file to create the container image in the python36-
app directory:

```
[user@host python36-app]$ cat Containerfile
FROM registry.access.redhat.com/ubi8/ubi:latest
RUN dnf install -y python36
CMD ["/bin/bash", "-c", "sleep infinity"]
```
The previous container file uses the registry.access.redhat.com/ubi8/ubi:latest
image as the base image. The container file then installs the python36 package and runs the
sleep infinity bash command to prevent the container from exiting.

Normally, a container runs a process, and then exits after that process is complete. The sleep
infinity command prevents the container from exiting, because the process never completes.
You can then test, develop, and debug inside the container.

After examining the container file, you use the podman build command to build the image.
The podman build command builds a container image by using instructions from one or more
container files. You must be in the directory with the container file to build the image with the
podman build command. You can use the podman build command -t option to provide the
name and python36:1.0 tag for the new image.

```
[user@host python36-app]$ podman build -t python36:1.0.
STEP 1/3: FROM registry.access.redhat.com/ubi8/ubi:latest
STEP 2/3: RUN dnf install -y python36
...output omitted...
STEP 3/3: CMD ["/bin/bash", "-c", "sleep infinity"]
COMMIT python36:1.0
--> 35ab820880f
Successfully tagged localhost/python36:1.0
35ab820880f1708fa310f835407ffc94cb4b4fe2506b882c162a421827b156fc
```
The last line of the preceding output shows the container image ID. Most Podman commands
use the first 12 characters of the container image ID to refer to the container image. You can use
this short ID or the name of a container or a container image as arguments for most Podman
commands.

```
Note
If a version number is not specified in the tag, then the image is created with the
:latest tag. If an image name is not specified, then the image and tag fields show
the <none> string.
```
You use the podman images command to verify that the image is created with the defined name
and tag.

```
[user@host ~]$ podman images
REPOSITORY TAG IMAGE ID CREATED SIZE
localhost/python36 1.0 35ab820880f1 3 minute ago 266 MB
registry.access.redhat.com/ubi8/python-38 latest a33d92f90990 1 hour ago 901 MB
```



You then use the podman inspect command to view the low-level information of the container
image and verify that its content matches the requirements for the container.

```
[user@host ~]$ podman inspect localhost/python36:1.0
...output omitted...
"Cmd": [
"/bin/bash",
"-c",
"sleep infinity"
],
...output omitted...
{
"created": "2022-04-18T19:47:52.708227513Z",
"created_by": "/bin/sh -c dnf install -y python36",
"comment": "FROM registry.access.redhat.com/ubi8/ubi:latest"
},
...output omitted...
```
The output of the podman inspect command shows the registry.access.redhat.com/
ubi8/ubi:latest base image, the dnf command to install the python36 package, and the
sleep infinity bash command that is executed at runtime to prevent the container from
exiting.

```
Note
The podman inspect command output varies from the python-38 image to
the python36 image, because you created the /python36 image by adding a
layer with changes to the existing registry.access.redhat.com/ubi8/
ubi:latest base image, whereas the python-38 image is itself a base image.
```
**Run Containers**

Now that you have the required container images, you can use them to run containers. A container
can be in one of the following states:

**Created**
A container that is created but is not started.

**Running**
A container that is running with its processes.

**Stopped**
A container with its processes stopped.

**Paused**
A container with its processes paused. Not supported for rootless containers.

**Deleted**
A container with its processes in a dead state.

The podman ps command lists the running containers on the system. Use the podman ps -a
command to view all containers (created, stopped, paused, or running) in the machine.

You use the podman create command to create the container to run later. To create the
container, you use the ID of the localhost/python36 container image. You also use the --




name option to set a name to identify the container. The output of the command is the long ID of
the container.

```
[user@host ~]$ podman create --name python36 dd6ca291f097
c54c7ee281581c198cb96b07d78a0f94be083ae94dacbae69c05bd8cd354bbec
```
```
Note
If you do not set a name for the container with the podman create or podman
run command with the --name option, then the podman utility assigns a random
name to the container.
```
You then use the podman ps and podman ps -a commands to verify that the container is
created but is not started. You can see information about the python36 container, such as
the short ID, name, and the status of the container, the command that the container runs when
started, and the image to create the container.

```
[user@host ~]$ podman create --name python36 dd6ca291f097
c54c7ee281581c198cb96b07d78a0f94be083ae94dacbae69c05bd8cd354bbec
[user@host ~]$ podman ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
[user@host ~]$ podman ps -a
CONTAINER ID IMAGE COMMAND CREATED STATUS
PORTS NAMES
c54c7ee28158 localhost/python36:1.0 /bin/bash -c slee... 5 seconds ago Created
python36
```
Now that you verified that the container is created correctly, you decide to start the container,
so you run the podman start command. You can use the name or the container ID to start the
container. The output of this command is the name of the container.

```
[user@host ~]$ podman start python36
python36
[user@host ~]$ podman ps
CONTAINER ID IMAGE COMMAND CREATED STATUS
PORTS NAMES
c54c7ee28158 localhost/python36:1.0 /bin/bash -c slee... 6 minutes ago Up 3
seconds ago python36
```
**Run a Container from a Remote Repository**

You can use the podman run command to create and run the container in one step. The podman
run command runs a process inside a container, and this process starts the new container.

You use the podman run command -d option to run a container in _detached mode_ , which runs
the container in the background instead of in the foreground of the session. In the example of the
python36 container, you do not need to provide a command for the container to run, because the
sleep infinity command was already provided in the container file that created the image for
that container.




To create the python38 container, you decide to use the podman run command and to refer
to the registry.access.redhat.com/ubi8/python-38 image. You also decide to use the
sleep infinity command to prevent the container from exiting.

```
[user@host ~]$ podman run -d --name python38 \
registry.access.redhat.com/ubi8/python-38 \
sleep infinity
a60f71a1dc1b997f5ef244aaed232e5de71dd1e8a2565428ccfebde73a2f9462
[user@host ~]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
c54c7ee28158 localhost/python36:1.0 /bin/bash -c
slee... 37 minutes ago Up 30 minutes ago python36
a60f71a1dc1b registry.access.redhat.com/ubi8/python-38:latest sleep infinity
32 seconds ago Up 33 seconds ago python38
```
```
Important
If you run a container by using the fully qualified image name, but the image is not
yet stored locally, then the podman run command first pulls the image from the
registry and then runs.
```
**Environment Isolation in Containers**

Containers isolate the environment of an application. Each container has its own file system,
networking, and processes. You can notice the isolation feature when you look at the output of the
ps command and compare it between the host machine and a running container.

You first run the ps -ax command on the local machine and the command returns an expected
result with many processes.

```
[root@host ~]# ps -ax
PID TTY STAT TIME COMMAND
1? Ss 0:01 /usr/lib/systemd/systemd --switched-root --system --
deseriali
2? S 0:00 [kthreadd]
3? I< 0:00 [rcu_gp]
4? I< 0:00 [rcu_par_gp]
...output omitted...
```
The podman exec command executes a command inside a running container. The command
takes the name or ID of the container as the first argument and the following arguments as
commands to run inside the container. You use the podman exec command to view the running
processes in the python36 container. The output from the ps aux command looks different,
because it is running different processes from the local machine.

```
[student@host ~]$ podman exec python38 ps -ax
PID TTY STAT TIME COMMAND
1? Ss 0:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /
usr/bin/sleep infinity
7? R 0:00 ps -ax
```



You can use the sh -c command to encapsulate the command to execute in the container. In the
following example, the ps -ax > /tmp/process-data.log command is interpreted as the
command to be executed in the container. If you do not encapsulate the command, then Podman
might interpret the greater-than character (>) as part of the podman command instead of as an
argument to the podman exec option.

```
[student@host ~]$ podman exec python38 sh -c 'ps -ax > /tmp/process-data.log'
PID TTY STAT TIME COMMAND
1? Ss 0:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /
usr/bin/sleep infinity
7? R 0:00 ps -ax
```
You decide to compare the installed python version on the host system with the installed python
version on the containers.

```
[user@host ~]$ python3 --version
Python 3.9.10
[user@host ~]$ podman exec python36 python3 --version
Python 3.6.8
[user@host ~]$ podman exec python38 python3 --version
Python 3.8.8
```
**File-system Isolation in Containers**

Developers can use the file-system isolation feature to write and test applications for different
versions of programming languages without the need to use multiple physical or virtual machines.

You create a simple bash script that displays hello world on the terminal in the /tmp directory.

```
[user@host ~]$ echo "echo 'hello world'" > /tmp/hello.sh
```
The /tmp/hello.sh file exists on the host machine, and does not exist on the file system inside
the containers. If you try to use the podman exec to execute the script, then it gives an error,
because the /tmp/hello.sh script does not exist in the container.

```
[user@host ~]$ stat /tmp/hello.sh
File: /tmp/hello.sh
Size: 19 Blocks: 8 IO Block: 4096 regular file
Device: fc04h/64516d Inode: 17655599 Links: 1
Access: (0644/-rw-r--r--) Uid: ( 1000/ user) Gid: ( 1000/ user)
Context: unconfined_u:object_r:user_tmp_t:s0
Access: 2022-04-19 21:47:40.101601412 -0400
Modify: 2022-04-19 21:47:36.497558132 -0400
Change: 2022-04-19 21:47:36.497558132 -0400
Birth: 2022-04-19 21:45:24.785976758 -0400
```
```
[user@host ~]$ podman exec python38 stat /tmp/hello.sh
stat: cannot statx '/tmp/hello.sh': No such file or directory
```
The podman cp command copies files and folders between host and container file systems. You
can copy the /tmp/hello.sh file to the python38 container with the podman cp command.




```
[user@host ~]$ podman cp /tmp/hello.sh python38:/tmp/hello.sh
```
```
[user@host ~]$ podman exec python38 stat /tmp/hello.sh
File: /tmp/hello.sh
Size: 19 Blocks: 8 IO Block: 4096 regular file
Device: 3bh/59d Inode: 12280058 Links: 1
Access: (0644/-rw-r--r--) Uid: ( 1001/ default) Gid: ( 0/ root)
Access: 2022-04-20 01:47:36.000000000 +0000
Modify: 2022-04-20 01:47:36.000000000 +0000
Change: 2022-04-20 02:02:04.732982187 +0000
Birth: 2022-04-20 02:02:04.732982187 +0000
```
After the script is copied to the container file system, it can be executed from within the container.

```
[user@host ~]$ podman exec python38 bash /tmp/hello.sh
hello world
```
**Remove Containers and Images**

You can remove containers and images by using the podman rm and podman rmi commands,
respectively. Before you remove a container image, any existing running containers from that
image must be removed.

You decide to remove the python38 container and its related image. If you attempt to remove
the registry.access.redhat.com/ubi8/python-38 image while the python38 container
exists, then it gives an error.

```
[user@host ~]$ podman rmi registry.access.redhat.com/ubi8/python-38
Error: Image used by
a60f71a1dc1b997f5ef244aaed232e5de71dd1e8a2565428ccfebde73a2f9462: image is in use
by a container
```
You must stop the container before you can remove it. To stop a container, use the podman stop
command.

```
[user@host ~]$ podman stop python38
```
After you stop the container, use the podman rm command to remove the container.

```
[user@host ~]$ podman rm python38
a60f71a1dc1b997f5ef244aaed232e5de71dd1e8a2565428ccfebde73a2f9462
```
When the container no longer exists, the registry.access.redhat.com/ubi8/python-38
can be removed with the podman rmi command.

```
[user@host ~]$ podman rmi registry.access.redhat.com/ubi8/python-38
Untagged: registry.access.redhat.com/ubi8/python-38:latest
Deleted: a33d92f90990c9b1bad9aa98fe017e48f30c711b49527dcc797135352ea57d12
```



```
References
podman(1), podman-build(1), podman-cp(1), podman-exec(1), podman-
images(1), podman-inspect(1), podman-ps(1), podman-pull(1), podman-rm(1),
podman-rmi(1), podman-run(1), podman-search(1), and podman-stop(1) man
pages.
```
```
For more information, refer to the Starting with containers chapter in the Building,
running, and managing Linux containers on Red Hat Enterprise Linux 9 Guide at
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/
html-single/building_running_and_managing_containers/index#starting-with-
containers_building-running-and-managing-containers
```



**Guided Exercise**

### Deploy Containers

```
In this exercise, you use container management tools to build an image, run a container, and
query the running container environment.
```
**Outcomes**

- Configure a container image registry and create a container from an existing image.
- Create a container by using a container file.
- Copy a script from a host machine into containers and run the script.
- Delete containers and images.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start containers-deploy
```
**Instructions**

**1.** Log in to the servera machine as the student user.

```
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$
```
**2.** Install the container-tools meta-package.

```
[student@servera ~]$ sudo dnf install container-tools
[sudo] password for student: student
...output omitted...
Is this ok [y/N]: y
...output omitted...
Complete!
```
**3.** Configure the registry.lab.example.com classroom registry in your home directory.
    Log in to the container registry with admin as the user and redhat321 as the password.

```
3.1. Create the /home/student/.config/containers directory.
```
```
[student@servera ~]$ mkdir -p /home/student/.config/containers
```



```
3.2. Create the /home/student/.config/containers/registries.conf file with
the following contents:
```
```
unqualified-search-registries = ['registry.lab.example.com']
```
```
[[registry]]
location = "registry.lab.example.com"
insecure = true
blocked = false
```
```
3.3. Verify that the classroom registry is added.
```
```
[student@servera ~]$ podman info
...output omitted...
registries:
registry.lab.example.com:
Blocked: false
Insecure: true
Location: registry.lab.example.com
MirrorByDigestOnly: false
Mirrors: null
Prefix: registry.lab.example.com
search:
```
- registry.lab.example.com
_...output omitted..._

```
3.4. Log in to the classroom registry.
```
```
[student@servera ~]$ podman login registry.lab.example.com
Username: admin
Password: redhat321
Login Succeeded!
```
**4.** Run the python38 container in detached mode from an image with the python 3.8
    package and based on the ubi8 image. The image is hosted on a remote registry.

```
4.1. Search for a python-38 container in the registry.lab.example.com registry.
```
```
[student@servera ~]$ podman search registry.lab.example.com/
NAME DESCRIPTION
...output omitted...
registry.lab.example.com/ubi8/python-38
registry.lab.example.com/ubi8/httpd-24
registry.lab.example.com/rhel8/php-74
```
```
4.2. Inspect the image.
```



```
[student@servera ~]$ skopeo inspect \
docker://registry.lab.example.com/ubi8/python-38
...output omitted...
"description": "Python 3.8 available as container is a base platform for
building and running various Python 3.8 applications and frameworks.
...output omitted...
```
```
4.3. Pull the python-38 container image.
```
```
[student@servera ~]$ podman pull registry.lab.example.com/ubi8/python-38
Trying to pull registry.lab.example.com/ubi8/python-38:latest...
...output omitted...
671cc3cb42984e338733ebb5a9a68e69e267cb7f9cb802283d3bc066f6321617
```
```
4.4. Verify that the container is downloaded to the local image repository.
```
```
[student@servera ~]$ podman images
REPOSITORY TAG IMAGE ID CREATED SIZE
registry.lab.example.com/ubi8/python-38 latest 671cc3cb4298 5 days ago 901 MB
```
```
4.5. Start the python38 container.
```
```
[student@servera ~]$ podman run -d --name python38 \
registry.lab.example.com/ubi8/python-38 sleep infinity
004756b52d3d3326545f5075594cffa858afd474b903288723a3aa299e72b1af
```
```
4.6. Verify that the container was created.
```
```
[student@servera ~]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
004756b52d3d registry.lab.example.com/ubi8/python-38:latest sleep infinity
About a minute ago Up About a minute ago python38
```
**5.** Build a container image called python39:1.0 from a container file, and use the image to
    create a container called python39.

```
5.1. Examine the container file in the /home/student/python39 directory.
```
```
[student@servera ~]$ cat /home/student/python39/Containerfile
FROM registry.lab.example.com/ubi9-beta/ubi:latest
RUN echo -e '[rhel-9.0-for-x86_64-baseos-rpms]\nbaseurl = http://
content.example.com/rhel9.0/x86_64/dvd/BaseOS\nenabled = true\ngpgcheck =
false\nname = Red Hat Enterprise Linux 9.0 BaseOS (dvd)\n[rhel-9.0-for-x86_64-
appstream-rpms]\nbaseurl = http://content.example.com/rhel9.0/x86_64/dvd/AppStream
\nenabled = true\ngpgcheck = false\nname = Red Hat Enterprise Linux 9.0 Appstream
(dvd)'>/etc/yum.repos.d/rhel_dvd.repo
RUN yum install --disablerepo=* --enablerepo=rhel-9.0-for-x86_64-baseos-rpms --
enablerepo=rhel-9.0-for-x86_64-appstream-rpms -y python3
```



```
5.2. Create the container image from the container file.
```
```
[student@servera ~]$ podman build -t python39:1.0 /home/student/python39/.
STEP 1/4: FROM registry.lab.example.com/ubi9-beta/ubi:latest
...output omitted...
STEP 2/4: RUN echo -e '[rhel-9.0-for-x86_64-baseos-rpms] ...
...output omitted...
STEP 3/4: RUN yum install --disablerepo=* --enablerepo=rhel-9.0-for-x86_64-baseos-
rpms --enablerepo=rhel-9.0-for-x86_64-appstream-rpms -y python3
...output omitted...
STEP 4/4: CMD ["/bin/bash", "-c", "sleep infinity"]
...output omitted...
Successfully tagged localhost/python39:1.0
80e68c195925beafe3b2ad7a54fe1e5673993db847276bc62d5f9d109e9eb499
```
```
5.3. Verify that the container image exists in the local image repository.
```
```
[student@servera ~]$ podman images
REPOSITORY TAG IMAGE ID CREATED SIZE
localhost/python39 1.0 80e68c195925 3 minutes ago 266 MB
registry.lab.example.com/ubi8/python-38 latest 671cc3cb4298 5 days ago 901 MB
registry.lab.example.com/ubi9-beta/ubi latest fca12da1dc30 4 months ago 235 MB
```
```
5.4. Inspect the python39 container.
```
```
[student@servera ~]$ podman inspect localhost/python39:1.0
...output omitted...
"comment": "FROM registry.lab.example.com/ubi9-beta/ubi:latest"
...output omitted...
"created_by": "/bin/sh -c yum install --disablerepo=*
--enablerepo=rhel-9.0-for-x86_64-baseos-rpms --enablerepo=rhel-9.0-for-x86_64-
appstream-rpms -y python3"
...output omitted...
"created_by": "/bin/sh -c #(nop) CMD [\"/bin/bash\", \"-c\", \"sleep
infinity\"]",
...output omitted...
```
```
5.5. Create the python39 container.
```
```
[student@servera ~]$ podman create --name python39 localhost/python39:1.0
3db4eabe9043224a7bdf195ab5fd810bf95db98dc29193392cef7b94489e1aae
```
```
5.6. Start the python39 container.
```
```
[student@servera ~]$ podman start python39
python39
```
```
5.7. Verify that the container is running.
```



```
[student@servera ~]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
004756b52d3d registry.lab.example.com/ubi8/python-38:latest sleep infinity
33 minutes ago Up 33 minutes ago python38
3db4eabe9043 localhost/python39:1.0 /bin/bash -c
slee... About a minute ago Up 42 seconds ago python39
```
**6.** Copy the /home/student/script.py script into the /tmp directory of the running
    containers and run the script on each container.

```
6.1. Copy the /home/student/script.py python script into the /tmp directory in
both containers.
```
```
[student@servera ~]$ podman cp /home/student/script.py python39:/tmp/script.py
[student@servera ~]$ podman cp /home/student/script.py python38:/tmp/script.py
```
```
6.2. Run the Python script in both containers, and then run the Python script on the host.
```
```
[student@servera ~]$ podman exec -it python39 python3 /tmp/script.py
This script was not run on the correct version of Python
Expected version of Python is 3.8
Current version of python is 3.9
[student@servera ~]$ podman exec -it python38 python3 /tmp/script.py
This script was correctly run on Python 3.8
[student@servera ~]$ python3 /home/student/script.py
This script was not run on the correct version of Python
Expected version of Python is 3.8
Current version of python is 3.9
```
**7.** Delete containers and images. Return to workstation.

```
7.1. Stop both containers.
```
```
[student@servera ~]$ podman stop python39 python38
...output omitted...
python38
python39
```
```
7.2. Remove both containers.
```
```
[student@servera ~]$ podman rm python39 python38
3db4eabe9043224a7bdf195ab5fd810bf95db98dc29193392cef7b94489e1aae
004756b52d3d3326545f5075594cffa858afd474b903288723a3aa299e72b1af
```
```
7.3. Remove both container images.
```



```
[student@servera ~]$ podman rmi localhost/python39:1.0 \
registry.lab.example.com/ubi8/python-38:latest \
registry.lab.example.com/ubi9-beta/ubi
Untagged: localhost/python39:1.0
Untagged: registry.lab.example.com/ubi8/python-38:latest
Deleted: 80e68c195925beafe3b2ad7a54fe1e5673993db847276bc62d5f9d109e9eb499
Deleted: 219e43f6ff96fd11ea64f67cd6411c354dacbc5cbe296ff1fdbf5b717f01d89a
Deleted: 671cc3cb42984e338733ebb5a9a68e69e267cb7f9cb802283d3bc066f6321617
```
```
7.4. Return to workstation.
```
```
[student@servera ~]$ exit
logout
Connection to servera closed.
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.

```
[student@workstation ~]$ lab finish containers-deploy
```
This concludes the section.




#### Manage Container Storage and Network Resources

**Objectives**

Provide persistent storage for container data by sharing storage from the container host, and
configure a container network.

**Manage Container Resources**

You can use containers to run a simple process and exit. You can also configure a container to run
a service continuously, such as a database server. In this scenario, you might consider adding more
resources to the container, such as persistent storage or DNS resolution for other containers.

You can use different strategies to configure persistent storage for containers. In an enterprise
container platform, such as Red Hat OpenShift, you can use sophisticated storage solutions to
provide storage to your containers without knowing the underlying infrastructure.

For small deployments where you use only a single container host, and without a need to scale,
you can create persistent storage from the container host by creating a directory to mount on the
running container.

When a container, such as a web server or database server, serves content for clients outside the
container host, you must set up a communication channel for those clients to access the content
of the container. You can configure _port mapping_ to enable communication to a container. With
port mapping, the requests that are destined for a port on the container host are forwarded to a
port inside the container.

To cover the topics in this lecture, imagine the following scenario.

As a system administrator, you are tasked to create the db01 containerized database, based
on MariaDB, to use the local machine to allow port 3306 traffic with the appropriate firewall
configuration. The db01 container must use persistent storage with the appropriate SELinux
context. Add the appropriate network configuration so that the client01 container can
communicate with the db01 container with DNS.

**Environment Variables for Containers**

Some container images allow passing environment variables to customize the container at
creation time. You can use environment variables to set parameters to the container to tailor for
your environment without the need to create your own custom image. Usually, you would not
modify the container image, because it would add layers to the image, which might be harder to
maintain.

You use the podman run -d registry.lab.example.com/rhel8/mariadb-105
command to run a containerized database, but you notice that the container fails to start.




```
[user@host ~]$ podman run -d registry.lab.example.com/rhel8/mariadb-105 \
--name db01
20751a03897f14764fb0e7c58c74564258595026124179de4456d26c49c435ad
[user@host ~]$ podman ps -a
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
20751a03897f registry.lab.example.com/rhel8/mariadb-105:latest run-mysqld
29 seconds ago Exited (1) 29 seconds ago db01
```
You use the podman container logs command to investigate the reason of the container
status.

```
[user@host ~]$ podman container logs db01
...output omitted...
You must either specify the following environment variables:
MYSQL_USER (regex: '^[a-zA-Z0-9_]+$')
MYSQL_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
MYSQL_DATABASE (regex: '^[a-zA-Z0-9_]+$')
Or the following environment variable:
MYSQL_ROOT_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
Or both.
...output omitted...
```
From the preceding output, you determine that the container did not continue to run because
the required environment variables were not passed to the container. So you inspect the
mariadb-105 container image to find more information about the environment variables to
customize the container.

```
[user@host ~]$ skopeo inspect docker://registry.lab.example.com/rhel8/mariadb-105
...output omitted...
"name": "rhel8/mariadb-105",
"release": "40.1647451927",
"summary": "MariaDB 10.5 SQL database server",
"url": "https://access.redhat.com/containers/#/registry.access.redhat.com/
rhel8/mariadb-105/
images/1-40.1647451927",
"usage": "podman run -d -e MYSQL_USER=user -e MYSQL_PASSWORD=pass -e
MYSQL_DATABASE=db -p 3306:3306 rhel8/mariadb-105",
"vcs-ref": "c04193b96a119e176ada62d779bd44a0e0edf7a6",
"vcs-type": "git",
"vendor": "Red Hat, Inc.",
...output omitted...
```
The usage label from the output provides an example of how to run the image. The url label
points to a web page in the Red Hat Container Catalog that documents environment variables and
other information about how to use the container image.

The documentation for this image shows that the container uses the 3306 port for the database
service. The documentation also shows that the following environment variables are available to
configure the database service:




**Environment Variables for the mariadb Image**

```
Variable Description
```
```
MYSQL_USER Username for the MySQL account to be created
```
```
MYSQL_PASSWORD Password for the user account
```
```
MYSQL_DATABASE Database name
```
```
MYSQL_ROOT_PASSWORD Password for the root user (optional)
```
After examining the available environment variables for the image, you use the podman run
command -e option to pass environment variables to the container, and use the podman ps
command to verify that it is running.

```
[user@host ~]$ podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
registry.lab.example.com/rhel8/mariadb-105
[user@host ~]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
4b8f01be7fd6 registry.lab.example.com/rhel8/mariadb-105:latest run-mysqld 6
seconds ago Up 6 seconds ago db01
```
**Container Persistent Storage**

By default, the storage that a container uses is ephemeral. The ephemeral nature of container
storage means that its contents are lost after you remove the container. You must consider file-
system level permissions when you mount a persistent volume in a container.

In the MariaDB image, the mysql user must own the /var/lib/mysql directory, the same as
if MariaDB was running on the host machine. The directory that you intend to mount into the
container must have mysql as the user and group owner (or the UID/GID of the mysql user, if
MariaDB is not installed on the host machine). If you run a container as the root user, then the
UIDs and GIDs on your host machine match the UIDs and GIDs inside the container.

The UID and GID matching configuration does not occur the same way in a rootless container. In a
rootless container, the user has root access from within the container, because Podman launches a
container inside the user namespace.

You can use the podman unshare command to run a command inside the user namespace. To
obtain the UID mapping for your user namespace, use the podman unshare cat command.

```
[user@host ~]$ podman unshare cat /proc/self/uid_map
0 1000 1
1 100000 65536
[user@host ~]$ podman unshare cat /proc/self/gid_map
0 1000 1
1 100000 65536
```



The preceding output shows that in the container, the root user (UID and GID of 0) maps to your
user (UID and GID of 1000 ) on the host machine. In the container, the UID and GID of 1 maps to
the UID and GID of 100000 on the host machine. Every UID and GID after 1 increments by 1. For
example, the UID and GID of 30 inside a container maps to the UID and GID of 100029 on the
host machine.

You use the podman exec command to view the mysql user UID and GID inside the container
that is running with ephemeral storage.

```
[user@host ~]$ podman exec -it db01 grep mysql /etc/passwd
mysql:x:27:27:MySQL Server:/var/lib/mysql:/sbin/nologin
```
You decide to mount the /home/user/db_data directory into the db01 container to provide
persistent storage on the /var/lib/mysql directory of the container. You then create the
/home/user/db_data directory and use the podman unshare command to set the user
namespace UID and GID of 27 as the owner of the directory.

```
[user@host ~]$ mkdir /home/user/db_data
[user@host ~]$ podman unshare chown 27:27 /home/user/db_data
```
The UID and GUI of 27 in the container maps to the UID and GID of 100026 on the host machine.
You can verify the mapping by viewing the ownership of the /home/user/db_data directory
with the ls command.

```
[student@workstation ~]$ ls -l /home/user/
total 0
drwxrwxr-x. 3 100026 100026 18 May 5 14:37 db_data
...output omitted...
```
Now that the correct file-system level permissions are set, you use the podman run command -v
option to mount the directory.

```
[user@host ~]$ podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql \
registry.lab.example.com/rhel8/mariadb-105
```
You notice that the db01 container is not running.

```
[user@host ~]$ podman ps -a
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
dfdc20cf9a7e registry.lab.example.com/rhel8/mariadb-105:latest run-mysqld
29 seconds ago Exited (1) 29 seconds ago db01
```
The podman container logs command shows a permission error for the /var/lib/mysql/
data directory.




```
[user@host ~]$ podman container logs db01
...output omitted...
---> 16:41:25 Initializing database ...
---> 16:41:25 Running mysql_install_db ...
mkdir: cannot create directory '/var/lib/mysql/data': Permission denied
Fatal error Can't create database directory '/var/lib/mysql/data'
```
This error happens because of the incorrect SELinux context that is set on the /home/user/
db_data directory on the host machine.

**SELinux Contexts for Container Storage**

You must set the container_file_t SELinux context type before you can mount the directory
as persistent storage to a container. If the directory does not have the container_file_t
SELinux context, then the container cannot access the directory. You can append the Z option to
the argument of the podman run command -v option to automatically set the SELinux context
on the directory.

So you use the podman run -v /home/user/dbfiles:/var/lib/mysql:Z command to
set the SELinux context for the /home/user/dbfiles directory when you mount it as persistent
storage for the /var/lib/mysql directory.

```
[user@host ~]$ podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql:Z \
registry.lab.example.com/rhel8/mariadb-105
```
You then verify that the correct SELinux context is set on the /home/user/dbfiles directory
with the ls command -Z option.

```
[user@host ~]$ ls -Z /home/user/
system_u:object_r:container_file_t:s0:c81,c1009 dbfiles
...output omitted...
```
**Assign a Port Mapping to Containers**

To provide network access to containers, clients must connect to ports on the container host that
pass the network traffic through to ports in the container. When you map a network port on the
container host to a port in the container, network traffic that is sent to the host network port is
received by the container.

For example, you can map the 13306 port on the container host to the 3306 port on the container
for communication with the MariaDB container. Therefore, traffic that is sent to the container host
port 13306 would be received by MariaDB that is running in the container.

You use the podman run command -p option to set a port mapping from the 13306 port from
the container host to the 3306 port on the db01 container.




```
[user@host ~]$ podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql:Z \
-p 13306:3306 \
registry.lab.example.com/rhel8/mariadb-105
```
Use the podman port command -a option to show all container port mappings in use. You can
also use the podman port db01 command to show the mapped ports for the db01 container.

```
[user@host ~]$ podman port -a
1c22fd905120 3306/tcp -> 0.0.0.0:13306
[user@host ~]$ podman port db01
3306/tcp -> 0.0.0.0:13306
```
You use the firewall-cmd command to allow port 13306 traffic into the container host machine
so that it can be redirected to the container.

```
[root@host ~]# firewall-cmd --add-port=13306/tcp --permanent
[root@host ~]# firewall-cmd --reload
```
```
Important
A rootless container cannot open a privileged port (ports below 1024 ) on the
container. That is, the podman run -p 80:8080 command does not normally
work for a running rootless container. To map a port on the container host below
1024 to a container port, you must run Podman as root or make other adjustments
to the system.
```
```
You can map a port above 1024 on the container host to a privileged port on the
container, even if you are running a rootless container. The 8080:80 mapping works
if the container provides service listening on port 80.
```
**DNS Configuration in a Container**

Podman v4.0 supports two network back ends for containers, Netavark and CNI. Starting with
RHEL 9, systems use Netavark by default. To verify which network back end is used, run the
following podman info command.

```
[user@host ~]$ podman info --format {{.Host.NetworkBackend}}
netavark
```



```
Note
The container-tools meta-package includes the netavark and aardvark-
dns packages. If Podman was installed as a stand-alone package, or if the
container-tools meta-package was installed later, then the result of the
previous command might be cni. To change the network back end, set the
following configuration in the /usr/share/containers/containers.conf file:
```
```
[network]
...output omitted...
network_backend = "netavark"
```
Existing containers on the host that use the default Podman network cannot resolve each other’s
hostnames, because DNS is not enabled on the default network.

Use the podman network create command to create a DNS-enabled network. You use the
podman network create command to create the network called db_net, and specify the
subnet as 10.87.0.0/16 and the gateway as 10.87.0.1.

```
[user@host ~]$ podman network create --gateway 10.87.0.1 \
--subnet 10.87.0.0/16 db_net
db_net
```
If you do not specify the --gateway or --subnet options, then they are created with the default
values.

The podman network inspect command displays information about a specific network. You
use the podman network inspect command to verify that the gateway and subnet were
correctly set and that the new db_net network is DNS-enabled.

```
[user@host ~]$ podman network inspect db_net
[
{
"name": "db_net",
...output omitted...
"subnets": [
{
"subnet": "10.87.0.0/16",
"gateway": "10.87.0.1"
}
],
...output omitted...
"dns_enabled": true,
...output omitted...
```
You can add the DNS-enabled db_net network to a new container with the podman run
command --network option. You use the podman run command --network option to create
the db01 and client01 containers that are connected to the db_net network.

```
[user@host ~]$ podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
```



```
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql:Z \
-p 13306:3306 \
--network db_net \
registry.lab.example.com/rhel8/mariadb-105
[user@host ~]$ podman run -d --name client01 \
--network db_net \
registry.lab.example.com/ubi8/ubi:latest \
sleep infinity
```
Because containers are designed to have only the minimum required packages, the containers
might not have the required utilities to test communication such as the ping and ip commands.
You can install these utilities in the container by using the podman exec command.

```
[user@host ~]$ podman exec -it db01 dnf install -y iputils iproute
...output omitted...
[user@host ~]$ podman exec -it client01 dnf install -y iputils iproute
...output omitted...
```
The containers can now ping each other by container name. You test the DNS resolution with the
podman exec command. The names resolve to IPs within the subnet that was manually set for
the db_net network.

```
[user@host ~]$ podman exec -it db01 ping -c3 client01
PING client01.dns.podman (10.87.0.4) 56(84) bytes of data.
64 bytes from 10.87.0.4 (10.87.0.4): icmp_seq=1 ttl=64 time=0.049 ms
...output omitted...
--- client01.dns.podman ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2007ms
rtt min/avg/max/mdev = 0.049/0.060/0.072/0.013 ms
```
```
[user@host ~]$ podman exec -it client01 ping -c3 db01
PING db01.dns.podman (10.87.0.3) 56(84) bytes of data.
64 bytes from 10.87.0.3 (10.87.0.3): icmp_seq=1 ttl=64 time=0.021 ms
...output omitted...
--- db01.dns.podman ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2047ms
rtt min/avg/max/mdev = 0.021/0.040/0.050/0.013 ms
```
You verify that the IP addresses in each container match the DNS resolution with the podman
exec command.

```
[user@host ~]$ podman exec -it db01 ip a | grep 10.8
inet 10.87.0.3/16 brd 10.87.255.255 scope global eth0
inet 10.87.0.4/16 brd 10.87.255.255 scope global eth0
[user@host ~]$ podman exec -it client01 ip a | grep 10.8
inet 10.87.0.3/16 brd 10.87.255.255 scope global eth0
inet 10.87.0.4/16 brd 10.87.255.255 scope global eth0
```



**Multiple Networks to a Single Container**

Multiple networks can be connected to a container at the same time to help to separate different
types of traffic.

You use the podman network create command to create the backend network.

```
[user@host ~]$ podman network create backend
```
You then use the podman network ls command to view all the Podman networks.

```
[user@host ~]$ podman network ls
NETWORK ID NAME DRIVER
a7fea510a6d1 backend bridge
fe680efc5276 db01 bridge
2f259bab93aa podman bridge
```
The subnet and gateway were not specified with the podman network create command
--gateway and --subnet options.

You use the podman network inspect command to obtain the IP information of the backend
network.

```
[user@host ~]$ podman network inspect backend
[
{
"name": "backend",
...output omitted...
"subnets": [
{
"subnet": "10.89.1.0/24",
"gateway": "10.89.1.1"
...output omitted...
```
You can use the podman network connect command to connect additional networks to a
container when it is running. You use the podman network connect command to connect the
backend network to the db01 and client01 containers.

```
[user@host ~]$ podman network connect backend db01
[user@host ~]$ podman network connect backend client01
```
```
Important
If a network is not specified with the podman run command, then the container
connects to the default network. The default network uses the slirp4netns
network mode, and the networks that you create with the podman network
create command use the bridge network mode. If you try to connect a bridge
network to a container by using the slirp4netns network mode, then the
command fails:
```
```
Error: "slirp4netns" is not supported: invalid network mode
```



You use the podman inspect command to verify that both networks are connected to each
container and to display the IP information.

```
[user@host ~]$ podman inspect db01
...output omitted...
"backend": {
"EndpointID": "",
"Gateway": "10.89.1.1",
"IPAddress": "10.89.1.4",
...output omitted...
},
"db_net": {
"EndpointID": "",
"Gateway": "10.87.0.1",
"IPAddress": "10.87.0.3",
...output omitted...
[user@host ~]$ podman inspect client01
...output omitted...
"backend": {
"EndpointID": "",
"Gateway": "10.89.1.1",
"IPAddress": "10.89.1.5",
...output omitted...
},
"db_net": {
"EndpointID": "",
"Gateway": "10.87.0.1",
"IPAddress": "10.87.0.4",
...output omitted...
```
The client01 container can now communicate with the db01 container on both networks.
You use the podman exec command to ping both networks on the db01 container from the
client01 container.

```
[user@host ~]$ podman exec -it client01 ping -c3 10.89.1.4 | grep 'packet loss'
3 packets transmitted, 3 received, 0% packet loss, time 2052ms
[user@host ~]$ podman exec -it client01 ping -c3 10.87.0.3 | grep 'packet loss'
3 packets transmitted, 3 received, 0% packet loss, time 2054ms
```
```
References
podman(1), podman-exec(1), podman-info(1), podman-network(1), podman-
network-create(1), podman-network-inspect(1), podman-network-ls(1),
podman-port(1), podman-run(1), and podman-unshare(1) man pages
```
```
For more information, refer to the Working with containers chapter in the Building,
running, and managing Linux containers on Red Hat Enterprise Linux 9 Guide at
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/
html-single/building_running_and_managing_containers/assembly_working-with-
containers_building-running-and-managing-containers
```



**Guided Exercise**

### Manage Container Storage and Network

### Resources

```
In this exercise, you pass environment variables to a container during creation, mount
persistent storage to a container, create and connect multiple container networks, and
expose container ports from the host machine.
```
**Outcomes**

- Create container networks and connect them to containers.
- Troubleshoot failed containers.
- Pass environment variables to containers during creation.
- Create and mount persistent storage to containers.
- Map host ports to ports inside containers.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start containers-resources
```
**Instructions**

**1.** Log in to the servera machine as the student user.

```
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$
```
**2.** Create the frontend container network. Create the db_client and db_01 containers
    and connect them to the frontend network.

```
2.1. Use the podman network create command --subnet and --gateway
options to create the frontend network with the 10.89.1.0/24 subnet and the
10.89.1.1 gateway.
```
```
[student@servera ~]$ podman network create --subnet 10.89.1.0/24 \
--gateway 10.89.1.1 frontend
frontend
```
```
2.2. Log in to the registry.lab.example.com registry.
```



```
[student@servera ~]$ podman login registry.lab.example.com
Username: admin
Password: redhat321
Login Succeeded!
```
```
2.3. Create the db_client container that is connected to the frontend network. Mount
the /etc/yum.repos.d DNF repositories directory inside the container at /etc/
yum.repos.d to allow package installation.
```
```
[student@servera ~]$ podman run -d --name db_client \
--network frontend \
-v /etc/yum.repos.d:/etc/yum.repos.d \
registry.lab.example.com/ubi9-beta/ubi \
sleep infinity
e20dfed7e392abe4b7bea3c25e9cb17ef95d16af9cedd50d68f997a663ba6c15
```
```
2.4. Create the db_01 container that is connected to the frontend network.
```
```
[student@servera ~]$ podman run -d --name db_01 --network frontend \
registry.lab.example.com/rhel8/mariadb-105
3e767ae6eea4578152a216beb5ae98c8ef03a2d66098debe2736b8b458bab405
```
```
2.5. View all containers.
```
```
[student@servera ~]$ podman ps -a
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
e20dfed7e392 registry.lab.example.com/ubi8/ubi:latest sleep infinity
56 seconds ago Up 56 seconds ago db_client
3e767ae6eea4 registry.lab.example.com/rhel8/mariadb-105:latest run-mysqld 1
second ago Exited (1) 1 second ago db_01
```
**3.** Troubleshoot the db_01 container and determine why it is not running. Re-create the
    db_01 container by using the required environment variables.

```
3.1. View the container logs and determine why the container exited.
```
```
[student@servera ~]$ podman container logs db_01
...output omitted...
You must either specify the following environment variables:
MYSQL_USER (regex: '^[a-zA-Z0-9_]+$')
MYSQL_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
MYSQL_DATABASE (regex: '^[a-zA-Z0-9_]+$')
Or the following environment variable:
MYSQL_ROOT_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
Or both.
...output omitted...
```
```
3.2. Remove the db_01 container and create it again with environment variables. Provide
the required environment variables.
```



```
[student@servera ~]$ podman rm db_01
3e767ae6eea4578152a216beb5ae98c8ef03a2d66098debe2736b8b458bab405
[student@servera ~]$ podman run -d --name db_01 \
--network frontend \
-e MYSQL_USER=dev1 \
-e MYSQL_PASSWORD=devpass \
-e MYSQL_DATABASE=devdb \
-e MYSQL_ROOT_PASSWORD=redhat \
registry.lab.example.com/rhel8/mariadb-105
948c4cd767b561432056e77adb261ab4024c1b66a22af17861aba0f16c66273b
```
```
3.3. View the current running containers.
```
```
[student@servera ~]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
e20dfed7e392 registry.lab.example.com/ubi8/ubi:latest sleep infinity
56 seconds ago Up 56 seconds ago db_client
948c4cd767b5 registry.lab.example.com/rhel8/mariadb-105:latest run-mysqld
11 seconds ago Up 12 seconds ago db_01
```
**4.** Create persistent storage for the containerized MariaDB service, and map the local
    machine 13306 port to the 3306 port in the container. Allow traffic to the 13306 port on the
    servera machine.

```
4.1. Create the /home/student/database directory on the servera machine.
```
```
[student@servera ~]$ mkdir /home/student/databases
```
```
4.2. Obtain the mysql UID and GID from the db_01 container, and then remove the db01
container.
```
```
[student@servera ~]$ podman exec -it db_01 grep mysql /etc/passwd
mysql:x:27:27:MySQL Server:/var/lib/mysql:/sbin/nologin
[student@servera ~]$ podman stop db_01
db_01
[student@servera ~]$ podman rm db_01
948c4cd767b561432056e77adb261ab4024c1b66a22af17861aba0f16c66273b
```
```
4.3. Run the chown command inside the container namespace, and set the user and
group owner to 27 on the /home/student/database directory.
```
```
[student@servera ~]$ podman unshare chown 27:27 /home/student/databases/
[student@servera ~]$ ls -l /home/student/
total 0
drwxr-xr-x. 2 100026 100026 6 May 9 17:40 databases
```
```
4.4. Create the db_01 container, and mount the /home/student/databases directory
from the servera machine to the /var/lib/mysql directory inside the db_01
container. Use the Z option to apply the required SELinux context.
```



```
[student@servera ~]$ podman run -d --name db_01 \
--network frontend \
-e MYSQL_USER=dev1 \
-e MYSQL_PASSWORD=devpass \
-e MYSQL_DATABASE=devdb \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/student/databases:/var/lib/mysql:Z \
-p 13306:3306 \
registry.lab.example.com/rhel8/mariadb-105
```
```
4.5. Install the mariadb package in the db_client container.
```
```
[student@servera ~]$ podman exec -it db_client dnf install -y mariadb
...output omitted...
Complete!
```
```
4.6. Create the crucial_data table in the dev_db database in the db_01 container
from the db_client container.
```
```
[student@servera ~]$ podman exec -it db_client mysql -u dev1 -p -h db_01
Enter password: devpass
...output omitted...
MariaDB [(none)]> USE devdb;
Database changed
MariaDB [devdb]> CREATE TABLE crucial_data(column1 int);
Query OK, 0 rows affected (0.036 sec)
```
```
MariaDB [devdb]> SHOW TABLES;
+-----------------+
| Tables_in_devdb |
+-----------------+
| crucial_data |
+-----------------+
1 row in set (0.001 sec)
```
```
MariaDB [devdb]> quit
Bye
```
```
4.7. Allow port 13306 traffic in the firewall on the servera machine.
```
```
[student@servera ~]$ sudo firewall-cmd --add-port=13306/tcp --permanent
[sudo] password for student: student
success
[student@servera ~]$ sudo firewall-cmd --reload
success
```
```
4.8. Open a second terminal on the Workstation machine and use the MariaDB client
to connect to the servera machine on port 13306 to show tables inside the db_01
container that are stored in the persistent storage.
```



```
[student@workstation ~]$ mysql -u dev1 -p -h servera --port 13306 \
devdb -e 'SHOW TABLES';
Enter password: devpass
+-----------------+
| Tables_in_devdb |
+-----------------+
| crucial_data |
+-----------------+
```
**5.** Create a second container network called backend, and connect the backend network
    to the db_client and db_01 containers. Test network connectivity and DNS resolution
    between the containers.

```
5.1. Create the backend network with the 10.90.0.0/24 subnet and the 10.90.0.1
gateway.
```
```
[student@servera ~]$ podman network create --subnet 10.90.0.0/24 \
--gateway 10.90.0.1 backend
backend
```
```
5.2. Connect the backend container network to the db_client and db_01 containers.
```
```
[student@servera ~]$ podman network connect backend db_client
[student@servera ~]$ podman network connect backend db_01
```
```
5.3. Obtain the IP addresses of the db_01 container.
```
```
[student@servera ~]$ podman inspect db_01
...output omitted...
"Networks": {
"backend": {
"EndpointID": "",
"Gateway": "10.90.0.1",
"IPAddress": "10.90.0.3",
...output omitted...
"frontend": {
"EndpointID": "",
"Gateway": "10.89.1.1",
"IPAddress": "10.89.1.6",
...output omitted...
```
```
5.4. Install the iputils package in the db_client container.
```
```
[student@servera ~]$ podman exec -it db_client dnf install -y iputils
...output omitted...
Complete!
```
```
5.5. Ping the db_01 container name from the db_client container.
```



```
[student@servera ~]$ podman exec -it db_client ping -c4 db_01
PING db_01.dns.podman (10.90.0.3) 56(84) bytes of data.
...output omitted...
--- db_01.dns.podman ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3048ms
rtt min/avg/max/mdev = 0.043/0.049/0.054/0.004 ms
```
```
5.6. Exit the servera machine.
```
```
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.

```
[student@workstation ~]$ lab finish containers-resources
```
This concludes the section.




#### Manage Containers as System Services

**Objectives**

Configure a container as a systemd service, and configure a container service to start at boot
time.

**Manage Small Container Environments with systemd**

**Units**

You can run a container to complete a system task or to obtain the output of a series of
commands. You also might want to run containers that run a service indefinitely, such as web
servers or databases. In a traditional environment, a privileged user typically configures these
services to run at system boot, and manages them with the systemctl command.

As a regular user, you can create a systemd unit to configure your rootless containers. You can
use this configuration to manage your container as a regular system service with the systemctl
command.

Managing containers based on systemd units is mainly useful for basic and small deployments
that do not need to scale. For more sophisticated scaling and orchestration of many container-
based applications and services, you can use an enterprise orchestration platform that is based on
Kubernetes, such as Red Hat OpenShift Container Platform.

To discuss the topics in this lecture, imagine the following scenario.

As a system administrator, you are tasked to configure the webserver1 container that is based on
the http24 container image to start at system boot. You must also mount the /app-artifacts
directory for the web server content and map the 8080 port from the local machine to the
container. Configure the container to start and stop with systemctl commands.

**Requirements for systemd User Services**

As a regular user, you can enable a service with the systemctl command. The service starts
when you open a session (graphical interface, text console, or SSH) and it stops when you close
the last session. This behavior differs from a system service, which starts when the system boots
and stops when the system shuts down.

By default, when you create a user account with the useradd command, the system uses the
next available ID from the regular user ID range. The system also reserves a range of IDs for
the user’s containers in the /etc/subuid file. If you create a user account with the useradd
command --system option, then the system does not reserve a range for the user containers. As
a consequence, you cannot start rootless containers with system accounts.

You decide to create a dedicated user account to manage containers. You use the useradd
command to create the appdev-adm user, and use redhat as the password.




```
[user@host ~]$ sudo useradd appdev-adm
myapp.service
[user@host ~]$ sudo passwd appdev-adm
Changing password for user appdev-adm.
New password: redhat
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: redhat
passwd: all authentication tokens updated successfully.
```
You then use the su command to switch to the appdev-adm user, and you start using the podman
command.

```
[user@host ~]$ su appdev-adm
Password: redhat
[appdev-adm@host ~]$ podman info
ERRO[0000] XDG_RUNTIME_DIR directory "/run/user/1000" is not owned by the current
user
[appdev-adm@host ~]$
```
Podman is a stateless utility and requires a full login session. Podman must be used within an SSH
session and cannot be used in a sudo or an su shell. So you exit the su shell and log in to the
machine via SSH.

```
[appdev-adm@host ~]$ exit
[user@host ~]$ exit
[user@example ~]$ ssh appdev-adm@host
[appdev-adm@host ~]$
```
You then configure the container registry and authenticate with your credentials. You run the http
container with the following command.

```
[appdev-adm@host ~]$ podman run -d --name webserver1 -p 8080:8080 -v \
~/app-artifacts:/var/www:Z registry.access.redhat.com/ubi8/httpd-24
af84e1ec33ea2f0d9787c56fbe7a62a4b9ce8ac03911be9e97f95575b306c297
[appdev-adm@host ~]$ podman ps -a
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
af84e1ec33ea registry.access.redhat.com/ubi8/httpd-24:latest /usr/bin/run-
http... 16 seconds ago Exited (1) 15 seconds ago 0.0.0.0:8080->8080/tcp
webserver1
```
You notice that the webserver1 container did not start, so you run the podman container
logs command to view the logs of the container.




```
[appdev-adm@host ~]$ podman container logs webserver1
=> sourcing 10-set-mpm.sh ...
=> sourcing 20-copy-config.sh ...
=> sourcing 40-ssl-certs.sh ...
---> Generating SSL key pair for httpd...
AH00526: Syntax error on line 122 of /etc/httpd/conf/httpd.conf:
DocumentRoot '/var/www/html' is not a directory, or is not readable
[appdev-adm@servera ~]$
```
The logs of the webserver1 container show that the /var/www/html file is not readable. This
error occurs because the container mount directory cannot find the html subdirectory. You delete
the existing container, update the command, and run it as follows:

```
[appdev-adm@host ~]$ podman run -d --name webserver1 -p 8080:8080 -v \
~/app-artifacts:/var/www/html:Z registry.access.redhat.com/ubi8/httpd-24
cde4a3d8c9563fd50cc39de8a4873dcf15a7e881ba4548d5646760eae7a35d81
[appdev-adm@host ~]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
cde4a3d8c956 registry.access.redhat.com/ubi8/httpd-24:latest /usr/bin/run-
http... 4 seconds ago Up 5 seconds ago 0.0.0.0:8080->8080/tcp webserver1
```
**Create systemd User Files for Containers**

You can manually define systemd services in the ~/.config/systemd/user/ directory. The
file syntax for user services is the same as for the system services files. For more details, review
the systemd.unit(5) and systemd.service(5) man pages.

Use the podman generate systemd command to generate systemd service files for an
existing container. The podman generate systemd command uses a container as a model to
create the configuration file.

The podman generate systemd command --new option instructs the podman utility to
configure the systemd service to create the container when the service starts, and to delete the
container when the service stops.

```
Important
Without the --new option, the podman utility configures the service unit file to start
and stop the existing container without deleting it.
```
You use the podman generate systemd command with the --name option to display the
systemd service file that is modeled for the webserver1 container.

```
[appdev-adm@host ~]$ podman generate systemd --name webserver1
...output omitted...
ExecStart=/usr/bin/podman start webserver1
ExecStop=/usr/bin/podman stop -t 10 webserver1
ExecStopPost=/usr/bin/podman stop -t 10 webserver1
...output omitted...
```



```
On start, the systemd daemon executes the podman start command to start the existing
container.
```
```
On stop, the systemd daemon executes the podman stop command to stop the container.
Notice that the systemd daemon does not delete the container on this action.
```
You then use the previous command with the addition of the --new option to compare the
systemd configuration.

```
[appdev-adm@host ~]$ podman generate systemd --name webserver1 --new
...output omitted...
ExecStartPre=/bin/rm -f %t/%n.ctr-id
ExecStart=/usr/bin/podman run --cidfile=%t/%n.ctr-id --cgroups=no-conmon --rm --
sdnotify=conmon --replace -d --name webserver1 -p 8080:8080 -v /home/appdev-adm/
app-artifacts:/var/www/html:Z registry.access.redhat.com/ubi8/httpd-24
ExecStop=/usr/bin/podman stop --ignore --cidfile=%t/%n.ctr-id
ExecStopPost=/usr/bin/podman rm -f --ignore --cidfile=%t/%n.ctr-id
...output omitted...
```
```
On start, the systemd daemon executes the podman run command to create and then
start a new container. This action uses the podman run command --rm option, which
removes the container on stop.
```
```
On stop, systemd executes the podman stop command to stop the container.
```
```
After systemd has stopped the container, systemd removes it using the podman rm -f
command.
```
You verify the output of the podman generate systemd command and run the previous
command with the --files option to create the systemd user file in the current directory.
Because the webserver1 container uses persistent storage, you choose to use the podman
generate systemd command with the --new option. You then create the ~/.config/
systemd/user/ directory and move the file to this location.

```
[appdev-adm@host ~]$ podman generate systemd --name webserver1 --new --files
/home/appdev-adm/container-webserver1.service
[appdev-adm@host ~]$ mkdir -p ~/.config/systemd/user/
[appdev-adm@host ~]$ mv container-webserver1.service ~/.config/systemd/user/
```
**Manage systemd User Files for Containers**

Now that you created the systemd user file, you can use the systemctl command --user
option to manage the webserver1 container.

First, you reload the systemd daemon to make the systemctl command aware of the new user
file. You use the systemctl --user start command to start the webserver1 container. Use
the name of the generated systemd user file for the container.

```
[appdev-adm@host ~]$ systemctl --user start container-webserver1.service
[appdev-adm@host ~]$ systemctl --user status container-webserver1.service
● container-webserver1.service - Podman container-webserver1.service
Loaded: loaded (/home/appdev-adm/.config/systemd/user/container-
webserver1.service; disabled; vendor preset: disabled)
Active: active (running) since Thu 2022-04-28 21:22:26 EDT; 18s ago
```



```
Docs: man:podman-generate-systemd(1)
Process: 31560 ExecStartPre=/bin/rm -f /run/user/1003/container-
webserver1.service.ctr-id (code=exited, status=0/SUCCESS)
Main PID: 31600 (conmon)
...output omitted...
[appdev-adm@host ~]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
18eb00f42324 registry.access.redhat.com/ubi8/httpd-24:latest /usr/bin/run-
http... 28 seconds ago Up 29 seconds ago 0.0.0.0:8080->8080/tcp webserver1
Created symlink /home/appdev-adm/.config/systemd/user/default.target.wants/
container-webserver1.service → /home/appdev-adm/.config/systemd/user/container-
webserver1.service.
```
```
Important
When you configure a container with the systemd daemon, the daemon monitors
the container status and restarts the container if it fails. Do not use the podman
command to start or stop these containers. Doing so might interfere with the
systemd daemon monitoring.
```
The following table summarizes the different directories and commands that are used between
systemd system and user services.

**Comparing System and User Services**

```
Storing custom System services /etc/systemd/system/ unit .service
unit files
User services ~/.config/systemd/user/ unit .service
```
```
System services
# systemctl daemon-reload
Reloading unit
files
```
```
User services
$ systemctl --user daemon-reload
```
```
System services
# systemctl start UNIT
# systemctl stop UNIT
```
```
Starting and
stopping a
service
```
```
User services
$ systemctl --user start UNIT
$ systemctl --user stop UNIT
```
```
System services
# systemctl enable UNIT
Starting a
service when
the machine
starts User services
$ loginctl enable-linger
$ systemctl --user enable UNIT
```



**Configure Containers to Start at System Boot**

Now that the systemd configuration for the container is complete, you exit the SSH session.
Some time after, you are notified that the container stops after you exited the session.

You can change this default behavior and force your enabled services to start with the server and
stop during the shutdown by running the loginctl enable-linger command.

You use the loginctl command to configure the systemd user service to persist after the last
user session of the configured service closes. You then verify the successful configuration with the
loginctl show-user command.

```
[user@host ~]$ loginctl show-user appdev-adm
...output omitted...
Linger=no
[user@host ~]$ loginctl enable-linger
[user@host ~]$ loginctl show-user appdev-adm
...output omitted...
Linger=yes
```
To revert the operation, use the loginctl disable-linger command.

**Manage Containers as Root with systemd**

You can also configure containers to run as root and manage them with systemd service files.
One advantage of this approach is that you can configure the service files to work exactly like
common systemd unit files, rather than as a particular user.

The procedure to set the service file as root is similar to the previously outlined procedure for
rootless containers, with the following exceptions:

- Do not create a dedicated user for container management.
- The service file must be in the /etc/systemd/system directory instead of in the
    ~/.config/systemd/user directory.
- You manage the containers with the systemctl command without the --user option.
- Do not run the loginctl enable-linger command as the root user.

For a demonstration, see the YouTube video from the Red Hat Videos channel that is listed in the
References at the end of this section.

```
References
loginctl(1), systemd.unit(5), systemd.service(5), subuid(5), and
podman-generate-systemd(1) man pages
```
```
Managing Containers in Podman with Systemd Unit Files
https://www.youtube.com/watch?v=AGkM2jGT61Y
```
```
For more information, refer to the Running Containers as Systemd Services with
Podman chapter in the Red Hat Enterprise Linux 9 Building, Running, and Managing
Containers Guide at
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html-
single/building_running_and_managing_containers/index
```



**Guided Exercise**

### Manage Containers as System Services

```
In this exercise, you configure a container to manage it as a systemd service, and use
systemctl commands to manage that container so that it automatically starts when the
host machine starts.
```
**Outcomes**

- Create systemd service files to manage a container.
- Configure a container so you can manage it with systemctl commands.
- Configure a user account for systemd user services to start a container when the host
    machine starts.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start containers-services
```
**Instructions**

**1.** Log in to the servera machine as the student user.

```
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$
```
**2.** Create a user account called contsvc and use redhat as the password. Use this user
    account to run containers as systemd services.

```
2.1. Create the contsvc user. Set redhat as the password for the contsvc user.
```
```
[student@servera ~]$ sudo useradd contsvc
[sudo] password for student: student
[student@servera ~]$ sudo passwd contsvc
Changing password for user contsvc.
New password: redhat
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: redhat
passwd: all authentication tokens updated successfully.
```
```
2.2. To manage the systemd user services with the contsvc account, you must log in
directly as the contsvc user. You cannot use the su and sudo commands to create a
session with the contsvc user.
```



```
Return to the workstation machine as the student user, and then log in as the
contsvc user.
```
```
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$ ssh contsvc@servera
...output omitted...
[contsvc@servera ~]$
```
**3.** Configure access to the registry.lab.example.com classroom registry in your home
    directory. Use the /tmp/containers-services/registries.conf file as a template.

```
3.1. Create the ~/.config/containers/ directory.
```
```
[contsvc@servera ~]$ mkdir -p ~/.config/containers/
```
```
3.2. The lab script prepares the registries.conf file in the /tmp/containers-
services/ directory. Copy that file to the ~/.config/containers/ directory.
```
```
[contsvc@servera ~]$ cp /tmp/containers-services/registries.conf \
~/.config/containers/
```
```
3.3. Verify that you can access the registry.lab.example.com registry. If everything
works as expected, then the command should list some images.
```
```
[contsvc@servera ~]$ podman search ubi
NAME DESCRIPTION
registry.lab.example.com/ubi7/ubi
registry.lab.example.com/ubi8/ubi
registry.lab.example.com/ubi9-beta/ubi
```
**4.** Use the /home/contsvc/webcontent/html/ directory as persistent storage for the
    web server container. Create the index.html test page with the Hello World line inside
    the directory.

```
4.1. Create the ~/webcontent/html/ directory.
```
```
[contsvc@servera ~]$ mkdir -p ~/webcontent/html/
```
```
4.2. Create the index.html file and add the Hello World line.
```
```
[contsvc@servera ~]$ echo "Hello World" > ~/webcontent/html/index.html
```
```
4.3. Confirm that the permission for others is set to r-- in the index.html file. The
container uses a non-privileged user that must be able to read the index.html file.
```



```
[contsvc@servera ~]$ ls -ld webcontent/html/
drwxr-x r-x. 2 contsvc contsvc 24 Aug 28 04:56 webcontent/html/
[contsvc@servera ~]$ ls -l webcontent/html/index.html
-rw-r-- r--. 1 contsvc contsvc 12 Aug 28 04:56 webcontent/html/index.html
```
**5.** Use the registry.lab.example.com/rhel8/httpd-24:1-105 image to run a
    container called webapp in detached mode. Redirect the 8080 port on the local host to the
    container 8080 port. Mount the ~/webcontent directory from the host to the /var/www
    directory in the container.

```
5.1. Log in to the registry.lab.example.com registry as the admin user with
redhat321 as the password.
```
```
[contsvc@servera ~]$ podman login registry.lab.example.com
Username: admin
Password: redhat321
Login Succeeded!
```
```
5.2. Use the registry.lab.example.com/rhel8/httpd-24:1-163 image to run
a container called webapp in detached mode. Use the -p option to map the 8080
port on servera to the 8080 port in the container. Use the -v option to mount the
~/webcontent directory on servera to the /var/www directory in the container.
```
```
[contsvc@servera ~]$ podman run -d --name webapp -p 8080:8080 -v \
~/webcontent:/var/www:Z registry.lab.example.com/rhel8/httpd-24:1-163
750a681bd37cb6825907e9be4347eec2c4cd79550439110fc6d41092194d0e06
...output omitted...
```
```
5.3. Verify that the web service is working on port 8080.
```
```
[contsvc@servera ~]$ curl http://localhost:8080
Hello World
```
**6.** Create a systemd service file to manage the webapp container with systemctl
    commands. Configure the systemd service so that when you start the service, the
    systemd daemon creates a container. After you finish the configuration, stop and then
    delete the webapp container. Remember that the systemd daemon expects that the
    container does not exist initially.

```
6.1. Create and change to the ~/.config/systemd/user/ directory.
```
```
[contsvc@servera ~]$ mkdir -p ~/.config/systemd/user/
[contsvc@servera ~]$ cd ~/.config/systemd/user
```
```
6.2. Create the unit file for the webapp container. Use the --new option so that systemd
creates a container when starting the service and deletes the container when
stopping the service.
```
```
[contsvc@servera user]$ podman generate systemd --name webapp --files --new
/home/contsvc/.config/systemd/user/container-webapp.service
```



```
6.3. Stop and then delete the webapp container.
```
```
[contsvc@servera user]$ podman stop webapp
webapp
[contsvc@servera user]$ podman rm webapp
750a681bd37cb6825907e9be4347eec2c4cd79550439110fc6d41092194d0e06
[contsvc@servera user]$ podman ps -a
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
```
**7.** Reload the systemd daemon configuration, and then enable and start your new
    container-webapp user service. Verify the systemd service configuration, stop and
    start the service, and display the web server response and the container status.

```
7.1. Reload the configuration to recognize the new unit file.
```
```
[contsvc@servera user]$ systemctl --user daemon-reload
```
```
7.2. Enable and start the container-webapp service.
```
```
[contsvc@servera user]$ systemctl --user enable --now container-webapp
Created symlink /home/contsvc/.config/systemd/user/multi-user.target.wants/
container-webapp.service → /home/contsvc/.config/systemd/user/container-
webapp.service.
Created symlink /home/contsvc/.config/systemd/user/default.target.wants/container-
webapp.service → /home/contsvc/.config/systemd/user/container-webapp.service.
```
```
7.3. Verify that the web server responds to requests.
```
```
[contsvc@servera user]$ curl http://localhost:8080
Hello World
```
```
7.4. Verify that the container is running.
```
```
[contsvc@servera user]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
3e996db98071 registry.access.redhat.com/ubi8/httpd-24:1-163 /usr/bin/run-http...
3 minutes ago Up 3 minutes ago 0.0.0.0:8080->8080/tcp webapp
```
```
Notice the container ID. Use this information to confirm that systemd creates a
container when you restart the service.
```
```
7.5. Stop the container-webapp service, and confirm that the container no longer
exists. When you stop the service, systemd stops and then deletes the container.
```
```
[contsvc@servera user]$ systemctl --user stop container-webapp
[contsvc@servera user]$ podman ps --all
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
```
```
7.6. Start the container-webapp service, and then confirm that the container is
running.
```



```
The container ID is different, because the systemd daemon creates a container with
the start instruction and deletes the container with the stop instruction.
```
```
[contsvc@servera user]$ systemctl --user start container-webapp
[contsvc@servera user]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
4584b4df514c registry.access.redhat.com/ubi8/httpd-24:1-163 /usr/bin/run-http...
6 seconds ago Up 7 seconds ago 0.0.0.0:8080->8080/tcp webapp
```
**8.** Ensure that the services for the contsvc user start at system boot. When done, restart the
    servera machine.

```
8.1. Run the loginctl enable-linger command.
```
```
[contsvc@servera user]$ loginctl enable-linger
```
```
8.2. Confirm that the Linger option is set for the contsvc user.
```
```
[contsvc@servera user]$ loginctl show-user contsvc
...output omitted...
Linger=yes
```
```
8.3. Switch to the root user, and then use the systemctl reboot command to restart
servera.
```
```
[contsvc@servera user]$ su -
Password: redhat
Last login: Fri Aug 28 07:43:40 EDT 2020 on pts/0
[root@servera ~]# systemctl reboot
Connection to servera closed by remote host.
Connection to servera closed.
[student@workstation ~]$
```
**9.** When the servera machine is up again, log in to servera as the contsvc user. Verify
    that systemd started the webapp container and that the web content is available.

```
9.1. Log in to servera as the contsvc user.
```
```
[student@workstation ~]$ ssh contsvc@servera
...output omitted...
```
```
9.2. Verify that the container is running.
```
```
[contsvc@servera ~]$ podman ps
CONTAINER ID IMAGE COMMAND
CREATED STATUS PORTS NAMES
6c325bf49f84 registry.access.redhat.com/ubi8/httpd-24:1-163 /usr/bin/run-http...
2 minutes ago Up 2 minutes ago 0.0.0.0:8080->8080/tcp webapp
```
```
9.3. Access the web content.
```



```
[contsvc@servera ~]$ curl http://localhost:8080
Hello World
```
```
9.4. Return to the workstation machine as the student user.
```
```
[contsvc@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$
```
**Finish**

On the workstation machine, run the lab finish containers-services script to
complete this exercise.

```
[student@workstation ~]$ lab finish containers-services
```
This concludes the section.




**Lab**

### Run Containers

```
In this lab, you configure a container on your server that provides a MariaDB database
service, stores its database on persistent storage, and starts automatically with the server.
```
**Outcomes**

- Create detached containers.
- Configure port redirection and persistent storage.
- Configure systemd for containers to start when the host machine starts.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start containers-review
```
**Instructions**

**1.** On serverb, install the container tools package.
**2.** The container image registry at registry.lab.example.com stores the rhel8/
    mariadb-103 image with several tags. Use the podsvc user to list the available tags and
    note the tag with the _lowest_ version number. Use the admin user and redhat321 password
    to authenticate to the registry. Use the /tmp/registries.conf file as a template for the
    registry configuration.
**3.** As the podsvc user, create the /home/podsvc/db_data directory. Configure the directory
    so that containers have read/write access.
**4.** Create the inventorydb detached container. Use the rhel8/mariadb-103 image from
    the registry.lab.example.com registry, and specify the tag with the lowest version
    number on that image, which you found in a preceding step. Map port 3306 in the container
    to port 13306 on the host. Mount the /home/podsvc/db_data directory on the host as
    /var/lib/mysql/data in the container. Declare the following variable values for the
    container:

```
Variable Value
```
```
MYSQL_USER operator1
```
```
MYSQL_PASSWORD redhat
```
```
MYSQL_DATABASE inventory
```
```
MYSQL_ROOT_PASSWORD redhat
```



```
You can copy and paste these parameters from the /home/podsvc/containers-
review/variables file on serverb. Execute the /home/podsvc/containers-
review/testdb.sh script to confirm that the MariaDB database is running.
```
**5.** Configure the systemd daemon so that the inventorydb container starts automatically
    when the system boots.

**Evaluation**

As the student user on the workstation machine, use the lab command to grade your work.
Correct any reported failures and rerun the command until successful.

```
[student@workstation ~]$ lab grade containers-review
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.

```
[student@workstation ~]$ lab finish containers-review
```
This concludes the section.




**Solution**

### Run Containers

```
In this lab, you configure a container on your server that provides a MariaDB database
service, stores its database on persistent storage, and starts automatically with the server.
```
**Outcomes**

- Create detached containers.
- Configure port redirection and persistent storage.
- Configure systemd for containers to start when the host machine starts.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start containers-review
```
**Instructions**

**1.** On serverb, install the container tools package.

```
1.1. Log in to serverb as the student user.
```
```
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$
```
```
1.2. Install the container-tools package.
```
```
[student@serverb ~]$ sudo dnf install container-tools
[sudo] password for student: student
...output omitted...
Is this ok [y/N]: y
...output omitted...
Complete!
```
**2.** The container image registry at registry.lab.example.com stores the rhel8/
    mariadb-103 image with several tags. Use the podsvc user to list the available tags and
    note the tag with the _lowest_ version number. Use the admin user and redhat321 password
    to authenticate to the registry. Use the /tmp/registries.conf file as a template for the
    registry configuration.

```
2.1. Return to the workstation machine as the student user.
```



```
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```
```
2.2. Log in to serverb as the podsvc user.
```
```
[student@workstation ~]$ ssh podsvc@serverb
...output omitted...
[podsvc@serverb ~]$
```
```
2.3. Configure access to the registry.lab.example.com classroom registry in your
home directory. Use the /tmp/registries.conf file as a template.
```
```
[podsvc@serverb ~]$ mkdir -p ~/.config/containers/
[podsvc@serverb ~]$ cp /tmp/registries.conf \
~/.config/containers/
```
```
2.4. Log in to the container registry with the podman login command.
```
```
[podsvc@serverb ~]$ podman login registry.lab.example.com
Username: admin
Password: redhat321
Login Succeeded!
```
```
2.5. View information about the registry.lab.example.com/rhel8/mariadb-103
image.
```
```
[podsvc@serverb ~]$ skopeo inspect \
docker://registry.lab.example.com/rhel8/mariadb-103
{
"Name": "registry.lab.example.com/rhel8/mariadb-103",
"Digest": "sha256:a95b...4816",
"RepoTags": [
"1-86" ,
"1-102",
"latest"
],
...output omitted...
```
```
The lowest version tag is the 1-86 version.
```
**3.** As the podsvc user, create the /home/podsvc/db_data directory. Configure the directory
    so that containers have read/write access.

```
3.1. Create the /home/podsvc/db_data directory.
```
```
[podsvc@serverb ~]$ mkdir /home/podsvc/db_data
```
```
3.2. Set the access mode of the directory to 777 so that everyone has read/write access.
```



```
[podsvc@serverb ~]$ chmod 777 /home/podsvc/db_data
```
**4.** Create the inventorydb detached container. Use the rhel8/mariadb-103 image from
    the registry.lab.example.com registry, and specify the tag with the lowest version
    number on that image, which you found in a preceding step. Map port 3306 in the container
    to port 13306 on the host. Mount the /home/podsvc/db_data directory on the host as
    /var/lib/mysql/data in the container. Declare the following variable values for the
    container:

```
Variable Value
```
```
MYSQL_USER operator1
```
```
MYSQL_PASSWORD redhat
```
```
MYSQL_DATABASE inventory
```
```
MYSQL_ROOT_PASSWORD redhat
```
```
You can copy and paste these parameters from the /home/podsvc/containers-
review/variables file on serverb. Execute the /home/podsvc/containers-
review/testdb.sh script to confirm that the MariaDB database is running.
```
```
4.1. Create the container.
```
```
[podsvc@serverb ~]$ podman run -d --name inventorydb -p 13306:3306 \
-e MYSQL_USER=operator1 \
-e MYSQL_PASSWORD=redhat \
-e MYSQL_DATABASE=inventory \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/podsvc/db_data:/var/lib/mysql/data:Z \
registry.lab.example.com/rhel8/mariadb-103:1-86
...output omitted...
```
```
4.2. Confirm that the database is running.
```
```
[podsvc@serverb ~]$ ~/containers-review/testdb.sh
Testing the access to the database...
SUCCESS
```
**5.** Configure the systemd daemon so that the inventorydb container starts automatically
    when the system boots.

```
5.1. If you used sudo or su to log in as the podsvc user, then exit serverb and use the
ssh command to directly log in to serverb as the podsvc user. Remember, the
systemd daemon requires the user to open a direct session from the console or
through SSH. Omit this step if you already logged in to the serverb machine as the
podsvc user by using SSH.
```
```
[student@workstation ~]$ ssh podsvc@serverb
...output omitted...
[podsvc@serverb ~]$
```



```
5.2. Create the ~/.config/systemd/user/ directory.
```
```
[podsvc@serverb ~]$ mkdir -p ~/.config/systemd/user/
```
```
5.3. Create the systemd unit file from the running container.
```
```
[podsvc@serverb ~]$ cd ~/.config/systemd/user/
[podsvc@serverb user]$ podman generate systemd --name inventorydb --files --new
/home/podsvc/.config/systemd/user/container-inventorydb.service
```
```
5.4. Stop and then delete the inventorydb container.
```
```
[podsvc@serverb user]$ podman stop inventorydb
inventorydb
[podsvc@serverb user]$ podman rm inventorydb
0d28f0e0a4118ff019691e34afe09b4d28ee526079b58d19f03b324bd04fd545
```
```
5.5. Instruct the systemd daemon to reload its configuration, and then enable and start the
container-inventorydb service.
```
```
[podsvc@serverb user]$ systemctl --user daemon-reload
[podsvc@serverb user]$ systemctl --user enable --now container-inventorydb.service
Created symlink /home/podsvc/.config/systemd/user/multi-user.target.wants/
container-inventorydb.service → /home/podsvc/.config/systemd/user/container-
inventorydb.service.
Created symlink /home/podsvc/.config/systemd/user/default.target.wants/
container-inventorydb.service → /home/podsvc/.config/systemd/user/container-
inventorydb.service.
```
```
5.6. Confirm that the container is running.
```
```
[podsvc@serverb user]$ ~/containers-review/testdb.sh
Testing the access to the database...
SUCCESS
[podsvc@serverb user]$ podman ps
CONTAINER ID IMAGE COMMAND CREATED
STATUS PORTS NAMES
3ab24e7f000d registry.lab.example.com/rhel8/mariadb-103:1-86 run-mysqld 47
seconds ago Up 46 seconds ago 0.0.0.0:13306->3306/tcp inventorydb
```
```
5.7. Run the loginctl enable-linger command for the user services to start
automatically when the server starts.
```
```
[podsvc@serverb ~]$ loginctl enable-linger
```
```
5.8. Return to the workstation machine as the student user.
```



```
[podsvc@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```
**Evaluation**

As the student user on the workstation machine, use the lab command to grade your work.
Correct any reported failures and rerun the command until successful.

```
[student@workstation ~]$ lab grade containers-review
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.

```
[student@workstation ~]$ lab finish containers-review
```
This concludes the section.
