
**What is a Container?**
![[Pasted image 20230408222229.png]]

**Where do containers live?**
Containers can be hosted/uploaded to online container repository. 
Companies maintain their own private repositories.
There is also a public repository of Docker called Docker Hub.

What was the story of software **development** before Containers?
Developer A is on a mac. Developer B is on linux. 
To install certain dependencies of their project like postgresSQL, Redis both the developers follow the process as designed for their OS. 
This has a downside as there are many steps involved and are different for both developers, leading to more ways things could go wrong in the installation. 
Now imagine a project having 10 dependencies. How tedious is that!

A Container on the other hand packages your dependency, for example, postgreSQL, and all you need to do as a developer is run the single docker command to fetch this container from the online container repository. The container image will install and run itself immediately after getting installed.
A Container has its own isolated linux environment and runs your service on that. 
This is why, containers can be fetched on any OS, and they are able to install and setup your required dependency service on this isoloted environment. 

![[Pasted image 20230408230631.png]]
**NOTE:** docker commands remain the same on any OS you use to fetch the container. 
**NOTE:** with docker containers, another benefit is that you can run 2 different versions of a software on your local environment. 

What was the story of **deployment** before containers? 
- required configurations had to be set on the server - this could lead to dependency version conflicts
- textual guide of deployment was maintained by development team and provided to deployment team. - this could lead to misunderstandings between the two teams. 
![[Pasted image 20230409002029.png]]
With **containers** however, this process is simplified as the developers and operations work together to package the application in a container. 
This means **no environmental configuration needed on the server**. - except Docker Runtime.
![[Pasted image 20230409002253.png]]

**What is a Container, technically?** 
Its a **layer of images** where the **base image** is mostly a linux distribution, example alpine or any debian based distro. But it is important that the base image be small (so that docker container as a result is small and lighweight) so it is mostly a linux distribution rather a windows/mac. 
The base image is followed by other dependency images and finally the application image at the top. 
```
application image
configurations 2
dependency2
configurations 1 
dependency1
base image: alpine
```
**NOTE:** alpine is not debian based. Its a GNU free linux distro.

`$docker pull postgres:9.6`

![[Pasted image 20230409005122.png]]
**NOTICE** how each of the images within the postgres container are being downloaded, where the first image is the base image which is a linux distro. 

**NOTE:** when you locally have some version of a container and you decide to upgrade to a new version, ONLY the images that are different will be downloaded and other same images will be kept untouched. This allows for very fast upgrades. 

```
$docker pull postgres:10.10
$docker pull postgres:9.6
$docker ps  #shows all the running containers
```


CONTAINER ID | IMAGE | COMMAND       | CREATED | STATUS | PORTS
-------------|-------|---------|--------|---------|------
fad0f8456ca7 | postgres:9.6 | `"docker-entrypoint..." `| 45 seconds ago | Up 47 seconds | 5432/tcp
8633db431a29 | postgres:10.10 | `"docker-entrypoint..." `| 5 minutes ago | Up 5 minutes| 5432/tcp


**Docker image Vs Docker Container**
You fetch a docker image. once you start that docker image/application, its called a container. Therefore all the packages uploaded to DockerHub are all images until it is actually executed where it then is now called a container. 
![[Pasted image 20230409011416.png]]

**Docker Vs Virtual machine:**
![[Pasted image 20230409025304.png]]
Docker | VM
-- | --
Docker virtualises the applications layer of the host and interact/use the kernel layer of the host to run itself| VM has the applications layer and its own kernel layer both. So it virtualizes the complete OS, which means that when you run a VM image on the host it doesn't use the kernel of the host.
Docker image is much smaller in size. (MBs) | VM images can be as large as couple of GBs.
Docker containers start and run much faster. | VM machine has to start the whole OS and then the applications.
Docker image of different OS is usually not compatible with a different OS. (fix is Docker toolbox)| VM of any OS can run on any OS host



