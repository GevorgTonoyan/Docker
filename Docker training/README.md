# Running Nginx container with a static website using COPY

COPY has two forms:

COPY <src>... <dest>
COPY ["<src>",... "<dest>"] (this form is required for paths containing whitespace)

The COPY instruction copies new files or directories from <src> and adds them to the filesystem of the container at the path <dest>.

Docker’s official documentation notes that COPY should always be the go-to instruction as it is more transparent than ADD.

If you need to copy from the local build context into a container, stick to using COPY.

The Docker team also strongly discourages using ADD to download and copy a package from a URL. Instead, it’s safer and more efficient to use wget or curl within a RUN command. By doing so, you avoid creating an additional image layer and save space.


# EXPOSE    
Why did we not use EXPOSE in the Dockerfile?

```The EXPOSE instruction does not actually publish the port. It functions as a type of documentation between the person who builds the image and the person who runs the container, about which ports are intended to be published. To actually publish the port when running the container, use the -p flag on docker run to publish and map one or more ports, or the -P flag to publish all exposed ports and map them to high-order ports.```

# Labs

 - Install Docker
 - create a static HTML file and put it in `html` directory
 - create a Dockerfile 
 - Build image and add a tag
 `docker build -t nginx_image .`
 - Run the container and expose the port  
 `docker run -d -p 8080:80  nginx_image`
 - navigate to `localhost:8080` 

`-d` detached
`-p` hostPort:containerPort
`-P` option tells Docker to map those ports to ports on the Docker host that are randomly selected from the range between 49153 and 65535. We could do this for multiple nginx containers so there would be no conflict. 

# Lab 2
COPY works fine if you need a static content. But now to make any changes in the website or the container config we would need to stop the container, rebuild the image with the updated file etc. This can be done better by attaching a volume to a container.

-ti for a terminal interactive (-t Allocate a pseudo-TTY
                                -i Keep STDIN open even if not attached)

We build the image just like in the example 1 and then run it this time we want to see the output of the terminal. We should see "Hello Java Academy" and --rm results in deleting the container and it's tracks. Then we can update the run.php message. After rerunning the container we will notice that the message did not change even though we did change the `run.php` file. This is because docker copied run.php during the build and is keeping it as it was at the time of the build.  

Let's check the images: 
`docker images`
Then delete the php images:
`docker rmi php`
This time we will not build the container but just run it with an image:
`docker run -it --rm --name JAP  -v *path_to_run.php*:*workdir_inside_container* -w /usr/src/myapp php:7.2-cli php run.php`

-w and WORKDIR
Workdir behaves as `mkdir -p` i.e. all elements of the path are created if they don't exist already.

# Hello World for Java
Let's run our container with some sample Java Code inside. 

# Deploy Spring Boot & MySQL in Docker

- Docker MySQL official image is avalivable (here)[https://hub.docker.com/_/mysql/]
`docker run --name mysql-standalone -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=test -e MYSQL_USER=sa -e MYSQL_PASSWORD=password -d mysql`

generate a Spring project  (I will use some generic one downloaded from https://start.spring.io)