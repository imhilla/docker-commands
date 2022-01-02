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
