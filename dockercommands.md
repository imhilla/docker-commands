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
