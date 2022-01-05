# Container manipulation basics

## Running containers

- Create and run a container `docker run <image name>` and better way of doing it is `docker <object-type> <command> <options>`<br>
  `<object-type>` could be container, image, network or volume.<br>
  `<command>` indicates task to be carried out i.e run.<br>
  `<options>` could be any valid parameter that can override the default behavior of the command i.e. `--publish`<br>
  `docker container run <image name>`
- Run a container using an image command example.<br>
  `docker container run --publish 8080:80 fhsinchy/hello-dock`

## Publishing Ports

- Containers are isolated environments, host doesn't know anything happening in the container. To allow access from outside, you must publish the appropriate port inside the container to a port on your local network.<br>
  `--publish <host port>:<container port>` i.e `--publish 8080:80`<br>
  - now to access the application on your browser, visit <a href="http://127.0.0.1:8080">http://127.0.0.1:8080</a>

## Detached Mode

- Use `--detach` or `-d` to run container in the background.
  i.e <br>
  `docker container run --detach --publish 8080:80 fhsinchy/hello-dock`

## Listing Containers

- List currently running containers
  `docker container ls`
- List all containers that have run in the past
  `docker container ls --all`

## Naming or Renaming Containers

- By default containers have two identifiers
  - CONTAINER ID - a random 64 characters long string.
  - NAME - combination of two random words, joined with an underscore.
- Naming a container can be achieved using `--name` option.
  - To run another container using an image with a new name can be executed like so.
    `docker container run --detach --publish 8888:80 --name hello-dock-container <image name>`
- You can rename containers using `container rename` command. Syntax is. `docker container rename <container identifier> <new name>`
- To rename conatiner A to B execute the following command.
  `docker container rename A B`

## Stopping or killing a running container

- Use Ctrl + C or close window.
- Use `docker container stop <container identifier>`
  `<container identifier>` can be ID or NAME
- You may use `docker container kill <container identifier>`

## Restarting containers

- Restarting could mean
  - Restarting a container that has been previously stopped or killed.
  - Rebooting a running container.
- Use `docker container start <container identifier>`.
- Use `docker container restart <container identifier>` to reboot.

## Creating container without running.

- To create use `docker container create --publish 8080:80 <image name>`
- To start use `docker container start <container identifier>`

## Removing dangling containers.

- Use `docker container rm <container identifier>`
- Use `docker container prune` to remove all.
- Use `--rm` to remove container as soon as it's stopped.
  - i.e `docker container run --rm --detach --publish 8888:80 --name hello-dock-volatile newname`

## Running containers in interactive mode.

- Some images do not just run some pre configured program. These instead are configured to run shell by default. Shells are interactive programs. Images configured to run such a program are interactive images. They require special -it option to be passed in the container run command.
  `docker container run --rm -it ubuntu`
- -t sets the stage for you to interact with any interactive program inside a container.<br>
  `docker container run -it node`

## Executing commands inside a container

i.e. `docker run alpine uname -a`. In this command `uname -a` was executed inside the alpine container.

- To perform an encoding using an image like busybox you could execute the following command.
  `docker container run --rm busybox sh -c 'echo -n my-secret | base64'`. What happens here is, in a container run command, whatever you pass after the image name gets passed to the default entry point of the image.
  Most of images except executable ones uses shell or sh as the default entry point. Any valid shell command can be passed to them as arguments.

## Working with executable images.

- Containers are isolated from your local system, but if you can map local directory to the directiry inside the container the files should be accessible to the container.
- One way to grant access to yout local file system is by using bind mounts. A bind mount lets you form two way data binding between the content of a local file system directory and another directory inside a container.
- A bind mount in action <br>
  `docker container run --rm -v $(pwd):/zone <image name> pdf` -v or --volume is used to create a bind mount for a container.
- This option can take three firlds separated by colons (:).
  The generic syntax for the option is as follows.
  `--volume <local file system directory absolute path>:<container file system drectory absolute path>:<read write access>`
- The third field is optional but you must pass the absolute path of your local directory and the absolute path of the directory inside.
- The source directory example could be `/home/fhsin/thezone`

# Image manipulation basics

## Image creation basics

- As explained images are multi-layered self contained files that act as templates for creating docker containers. They are like frozen, read only copy of a container.
- In order to create an image using one of your programs you must have a clear vision of what you want from the image. Take the official nginx image for example. You can start a container suing the command.
  `docker container run --rm --detach --name default-nginx --publish 8080:80 nginx`
- Now if you visit `http://127.0.0.1:8080` you will see a default response image.
- That's all nice and good but what if you want to create custom nginx image?
- In order to make custom nginx image, you must have clear picture of what the final state of the image will be.
- The image should have custom nginx pre-installed which can be done using package manager
- The image should start nginx automatically upon running.
- Create a Dockerfile, a Dockerfile is a collection of instructions that once processed by the daemon, results into an image.
- Images are multilayered files and in this file, each line (known as instructions) that you've written creates a layer for your image.
- Every valid `Dockerfile` starts with a `FROM`. The instruction sets the base image for your resultant image. By setting `ubuntu:latest` as the base image, you get all the goodness of ubuntu already available in your custom image, hence you can do things like `apt-get` for easy package installation.
- The `EXPOSE` instruction is used to indicate port that needs to be published.
- The `RUN` instruction in a docker file executes a command inside the container shell.
- The `RUN` instructions are written ins `shell` form. Can also be written in `exec` form.
- Finally the `CMD` instruction sets the default command for your image. This instruction is written in `exec` form comprising of three separate parts.
  i.e `CMD ["nginx", "-g", "daemon off;"]`
  - `gninx` refers to the NGINX executable.
  - The `-g` and `daemon off` are options for NGINX.
  - The `CMD` instructions can also be written in shell.
- Now you can build an image by running <br>
  `docker image <command> <options>`
- Example `docker image build .`
- To perform an image build , the daemon needs two very specific information. These are the name of the Dockerfile and build context.
- `docker image build` is the command for bulding the image. The daemon finds any file named Dockerfile within context.
- The . at the end sets the context for the build. The context means directory accessible by daemon during build process.
- Now to run a container using this image, you can use the container run command coupled with the image ID you recieved as the result of the build process.
- i.e `docker container run --rm --detach --name custom-nginx-packaged --publish 8080:80 3199372aa3fc`

## Tagging Images

- Just like containers, you can assign custom identifiers to your images instead of relying
  on the randomly generated id. In case of an mage it's called taging instaed of naming. The --tag or -t option is used in such cases.
- Generic syntax for the option is as follows.
  `--tag <image repository>:<image tag>`
  The repository is usually known as the image name and tag indicates a certain build or version. i.e take the official mysql for example, if you want to run a container using a specific version of MYSQL i.e 5.7, you can execute `docker container run mysql:5.7` where mysql is the image repository and 5.7 is the tag.
- In order to tag your custom NGINX image with `custom-ginx:packaged` you can execute the following commands.
  `docker image build --tag custom-nginx:packaged .`
- Incase you forget to add tag during build, you could use
  `docker image tag <image id> <image repository>:<image tag>`

## Listing and removing images

`docker images ls`

- Images listed can be removed using `docker image rm <image identifier>`
- Identifier can be image ID or image repository.
  If you use the repository you will have to identify tag as well.
- To delete the `custom-nginx:packaged` image you may execute.
  `docker image rm custom-nginx:packaged`
- You can also use `image prune` to clean up all untagged dangling images as follows:
  `docker image prune --force`
- The `--force` or `-f` option skips any confirmation questions. You can also use `--all` or `-a` option to remove all cached images in your local registry.

## Sharing images online

- Login `docker login`
- In order to share an image online, the image has to be tagged. The general syntax is `--tag <image repository>:<image tag>`
- To share an image online you will have to tag it following the `<docker hub username>/<image name>:<image tag>`
- Example `docker image build --tag imhilla/custom-nginx:latest --file Dockerfile.built .`
- Once the image has benn built you can then upload that by executing the following command.
  `docker image push <image repository>:<image tag>`
  i.e `docker image push imhilla/custom-nginx:latest`
