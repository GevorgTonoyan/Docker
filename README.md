# Docker Training

# Part 2 - Docker training for junior developers

*This is part 2 of Introduction to Docker.*
**Table of contents**

---

## Useful flags

- `d` detached
- `p` hostPort:containerPort
- `P` option tells Docker to map those ports to ports on the Docker host that are randomly selected from the range between 49153 and 65535. We could do this for multiple nginx containers so there would be no conflict.
- `ti` for a terminal interactive (-t Allocate a pseudo-TTY
- `i` Keep STDIN open even if not attached)
- `-w`  WORKDIR behaves as `mkdir -p` i.e. all elements of the path are created if they don't exist already.

---

# Labs

### Example 1 - **Running an nginx container with a static website using COPY**

COPY <src>... <dest>

COPY ["<src>",... "<dest>"] (this form is required for paths containing whitespace)


The COPY instruction copies new files or directories from `src` and adds them to the filesystem of the container at the path `dest`.

*Docker’s official documentation notes that COPY should always be the go-to instruction as it is more transparent than ADD.*

If you need to copy from the local build context into a container, stick to using COPY.

The Docker team also strongly discourages using ADD to download and copy a package from an URL. Instead, it’s safer and more efficient to use `wget` or `curl` within a RUN command. By doing so, you avoid creating an additional image layer and save space

**Why did we not use EXPOSE in the Dockerfile?**

> The EXPOSE instruction does not actually publish the port. It
functions as a type of documentation between the person who builds the
image and the person who runs the container, about which ports are
intended to be published. To actually publish the port when running
the container, use the -p flag on docker run to publish and map one or
more ports, or the -P flag to publish all exposed ports and map them
to high-order ports.

### **Steps**

Install Docker

Create a new directory

Create a static HTML file and put it in `html` directory within the newly created folder.

For html code you can use the provided example:

    <!DOCTYPE html>
    <html>
    <body>
    
    <h1>Hello Java Academy</h1>
    
    </body>
    </html>

Create a `Dockerfile`:

- Use `nginx` as a base image
- Copy the file from `html` directory to a container

Build image and tag it with an appropriate name
`docker build -t nginx_image .`

Run the container and expose the port
`docker run -d -p 8080:80 nginx_image`

Navigate to `[localhost:8080](http://localhost:8080)` and confirm you can see the message specified in the static html that you created. 

Clean up:

- find the containerID with `docker ps` and run `docker stop <containerID>` (you can use the first 3 characters of ID) `docker rm <containerID>`

# Example 2

COPY works fine if you need a static content. But now to make any changes to the website or the container config we would need to stop the container, rebuild the image with the updated file and rerun it again. This can avoided in our case by attaching a volume to a container. 

### **Steps**

Part 1:

Navigate to example 2 directory (you can either use the files provided or write your own)

Build the image just like in the **Example 1** and then run it. 

- Note if there was any message printed in your terminal
- Check if the containers are running
- Try to explain what happened
- Run the container again this time without using `-d` flag
- Clean up

Part 2

As you noticed removing the detached flag when running the container resulted in a container printing the message specified in run.php message and then exiting. If you would like the container to not exit you can use `-ti` to get an interactive terminal to the container. 

Now let's change the message in the `run.php`  and run the container again. 

Notice that the message did not change.

- What is the reason for the message not changing even though we did change it in the `run.php`

---

**This is because docker copied run.php during the build and is keeping it as it was at the time of the build.**

---

Let's check the images:

`docker images`

You can either delete the php container that you build earlier or build the image again with the same name to get an updated image.

`docker rmi php` 

Since we want to minimise manual tasks as much as possible we can approach this task differently. 

In this case instead of deleting and rebuilding the images each time we want to test our changes we can just run the container with preferred base image and specify a directory on our host to be attached to the container. This way the `docker run` will run the code with all the changes made to `run.php`

Example command:

`docker run --rm --name JAP -v *full_path_to_run.php*:*workdir_inside_container* -w /usr/src/myapp php:7.2-cli php run.php`

- try to explain what used flags do
- make some changes to the `run.php` file and rerun the command to check if the changes apply

# Hello World for Java

Let's run our container with some sample Java Code inside.

# Deploy Spring Boot & MySQL in Docker

- Docker MySQL official image is avalivable (here)\[\[https://hub.docker.com/_/mysql/](https://hub.docker.com/_/mysql/)]

`docker run --name mysql-standalone -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=test -e MYSQL_USER=sa -e MYSQL_PASSWORD=password -d mysql`

generate a Spring project (I will use some generic one downloaded from [https://start.spring.io])