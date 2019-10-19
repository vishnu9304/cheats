#### Docker Edition

    Community Edition   - Free & Opensource
    Enterprise Edition  - Paid version

#### Installing Docker CE

    1 - sudo yum install -y device-mapper-persistent-data lvm2
    2 - sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    3 - sudo yum install -y docker-ce-18.09.5 docker-ce-cli-18.09.5 containerd.io
    4 - sudo systemctl start docker
    5 - sudo systemctl enable docker 
    6 - sudo useradd -a -G docker <username> // For allowing normal users to execute docker commands, add them under docker group.

#### Selcting Storage Driver

    Command to find the storage driver: docker info

    Changing the default storage driver:

        1 - Change it under "/usr/lib/systemd/system/docker.service" and add this flag:  --storage-driver devicemapper

        ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
        ExecStart=/usr/bin/dockerd -H fd:// --storage-driver devicemapper --containerd=/run/container containerd.sock

        2 - Daemon config file. (Recommended Way)

        /etc/docker/daemon.json

        {
            "storage-driver": "devicemapper"
        }

    When you start a container, a thin writable container layer is added on top of the other layers.
    Any changes the container makes to the filesystem are stored here.
    The major difference between a container and an image is the top writable layer.
    When the container is deleted, the writable layer is also deleted. 
    The underlying image remains unchanged.
    
    Container Size On Disk: docker ps -s    // https://docs.docker.com/storage/storagedriver/

    Copy On Write: Copy the files to the writable layer only if the files are modifed.

#### Running the docker Container: (docker run command)

    Some of the common falgs used with "docker run" command.
    Syntax: docker run [options] image[:tag] [command] [args]
    -d      Run container in detached mode.
    --name  For providing a descriptive name to the container.
    
    --restart <options>
        no - Never restart the container.
        on-failure - If container exits with non-zero exit code.  
        always - Always restart the container whether it is successful or not. Also start the container on daemon restart.
        unless-stopped - It is similar to "always", Container will not be restarted, if you stop it explicitly.
                
    -p <host_port>:<container_port>     Publish, expose a port inside the container by mapping it with host port.
    --rm                                Remove the container automatically when it exits. Not compatible with "--restart" option.
    --memory                            Memory Hard Limit.
    --memory-reservation                A soft limit on memory usage. Container will be restricted within this memory if docker detects memory contention on the host.

#### Logging Drivers

    https://docs.docker.com/config/containers/logging/configure/

    Logging Drivers are a pluggable framework for accessing log data from services and container in docker.

    Configure the default logging driver under /etc/docker/daemon.json using options "log-driver" and "log-opts" (System Wide)

    {
        "log-driver": "json-file",
        "log-opts": {
                "max-size": "15m"
            }
    }

    How to override the settings at the container level.

    docker run --log-driver json-file --log-opt max-size=50m nginx

#### Image Creation, Management, and Registry

    Docker Images:

        The image consists of one or more read-only layers, while the container adds one additional layer.

    The layered filesystem allows multiple images and containers to share the same layers. 
    
    This results in
    
        1 - Small overall footprint
        2 - Faster image transfer
        3 - Faster image build

        docker image pull IMAGE[:TAG]   To pull the docker image.
        docker image history IMAGE      To list the layers used by the image.
    
#### Components Of Dockerfile
    
        https://docs.docker.com/engine/reference/builder/

        A docker file is set of instructions which are used to construct docker image.
        These instructions are called directives.

        FROM            Starts with a new build stage and sets the base image.
        ENV             To set environment variables.
        RUN             Create new filesystem layer by running a command.
        CMD             Default command to run when the container is executed.
        EXPOSE          Technically it will not expose any ports. Documents which ports are intended to be published when running a container.
        WORKDIR         Sets the current working directory for subsequent directives such as ADD,COPY,CMD,ENTRYPOINT
                        We can have multiple WORKDIR directives inside the docker file.
                        WORKDIR /var
                        WORKDIR www         //Relative path
                        WORKDIR html
                        The above three WORKDIR directives are equivalent to /var/www/html      //Absolute path
        COPY            Copy files from local machine to the image
        ADD             Similar to copy, but little advanced than COPY, like pulling files using URL and extract an archive into loose files in the image.
        STOPSIGNAL      Specify the signal that will be used to stop the container. When you run docker container stop, this signal will be passed.
        HEALTHCHECK     Used to specify a custom health check, to verify the container is running fine.
                        HEALTHCHECK CMD curl localhost:80

        docker build -t custom-nginx .

        docker run -d custom-nginx -p 8080:80
        
        Example:

        FROM Centos7:latest
        ENV NGINX_VERSION=1.0.8
        RUN yum update -y && yum install -y curl
        RUN yum update -y && yum install -y nginx=$NGINX_VERSION

        /* 
        why "yum update -y" is added twice?

        When you rebuild the image by changing nginx version, it will first look for the line "RUN yum update -y && yum install -y curl"
        since there is not change in the RUN directive it will use the same old layer.

        Note: Inorder to trigger a change to any layer, we should modify the RUN directive.
        */

        CMD ["nginx", "-g", "daemon off;"]

#### Building Efficient Images

    General tips:

    - Put things that are less likely to change on lower level layers.
    - Don't create unnecessary layers.
    - Avoid including any unnecessary files, packages, etc..

    Docker Multistage builds:

        Docker supports ablity to perform multistage builds.It will have more than one FROM directive in the docker file with each FROM directive starting a new stage.

        Each stage begins with a completely new set of layers, allowing you to selectively copy only the files needed from previous layer.

    Example:
    
        Below steps will create an image size of 774MB

        FROM golang:1.12.4
        WORKDIR /helloworld
        COPY helloworld.go .
        RUN GOOS=linux go build -a -installsuffix cgo -o helloworld .
        CMD ["./helloworld"]

    Example:

        Multistate build.This will produce only image size of 7MB.
        Idea is to keep only the required files not all, in our case we don't need the entire to go image to run our program. 
        All we need is a binary. Create the binay in STAGE1 and move it to STAGE2 image with smaller size.

        FROM golang:1.12.4 AS compiler                                  //STAGE1
        WORKDIR /helloworld
        COPY helloworld.go .
        RUN GOOS=linux go build -a -installsuffix cgo -o helloworld .

        FROM alpine:3.9.3                                               //STAGE2
        WORKDIR /root
        COPY --from=compiler /helloworld/helloworld .
        CMD ["./helloworld"]

        REPOSITORY                                                     TAG                 IMAGE ID            CREATED             SIZE
        gostage                                                        latest              3b3816104992        7 seconds ago       7.53MB
        go-custom                                                      latest              9802cc0d3ab8        10 minutes ago      776MB

#### Managing Images

    docker pull                                                         To pull the images from registry, if not found locally.
    docker image ls                                                     To list images.
    docker image ls -a                                                  To list images including intermediate images.
    docker inspect <image name>                                         To get more info about the images. Provides json output.
    docker inspect --format "{{.Arch}} {{.Os}}"                         --format (go template) to extract specific fields.
    docker image rm <image name>    / docker rmi <image name>           To remove the image.
    docker container ls -a / docker ps -a                               To list the containers.

#### Dangling Images:

    Dangling Images are something which doesn't have tags and no containers referencing them.

    When we delete a container, it doesn't necessarily delete the uderlying image. 
    
    It will delete only the tags not the image. So that image is called danglig image.

#### Cleaning up the Dangling Images:

    docker image prune

    If you have any image which is not referenced by anything or any containers.
    This command will do a clean up.

#### Flattening an Image: Docker doesn't provide an official way to do this.

    Run a container -> docker export (export the container to an archive) -> docker import (Import the archive as new image)
        docker export <container> > flat.tar
        cat flat.tar | docker import - flat:latest


#### Docker Storage: https://docs.docker.com/storage/

    Storage drivers are also known as Graph Drivers. 
    The proper storage driver to use often depends on your operating system.

    overlay2        Centos8 and RHEL versions   
    aufs            Ubuntu 
    device mapper   Centos7 and earlier
    
    Storage Models: Persistent data can be managed using several storage models.

#### Filesystem storage:

    Data stored in the form of a file system.
    Used by overlay and aufs.
    Efficient use of memory.
    Inefficient with write-heavy workloads.

#### Block storage:

    Stores data in block.
    Used by device mapper.
    Efficient with write-heavy workloads.

#### Object storage:

    Stores data in an external object based store.
    Application must be designed to use object based storage.
    Flexible and scalable.