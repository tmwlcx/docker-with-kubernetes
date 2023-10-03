# Basic Docker Commands
`docker logs container_name` shows past logs from the running containers.

`docker logs -f container_name` tails the logs from running containers

`docker start container_name` runs an existing container in detached mode by default (you can attach it with `docker attach container_name`) or by passing `-a` flag to `docker start container_name`

`docker run container_name` runs an existing container in attached mode by default. you can run in detached by passing the `-d` flag: `docker run -d container_name`.

`docker rm container_name` removes containers. 

`docker container prune` removes all stopped containers

`docker rmi image_ID` removes docker images with the specified `image_ID`. only removes images not used in containers (even stopped containers)

`docker run -rm image_ID` removes a container once it is no longer running

`docker cp file_source container_name:file_destination` copies files from host machine to container. you can pass either a file or folder to docker in a similar way: for instance, `docker cp source_folder/. container_name:destination_folder` will copy all of the files from the `source_folder` into the `destination_folder` on the container.

# Naming 
By default, Docker names images and containers by itself. The image names are a very long hash. The container names are silly auto-generated names. But the good news is that containers can have custom names and images can have custom tags. 

## Containers
when running a container, the `--name container_name` flag will append the `container_name` so that you can reference this instead of the auto-generated silly name. For example, `docker run --name docker_container_name docker_image_ID` will append the `docker_contianer_name` to the container and not generate a silly name.

## Images
similar to container names, images can have names. Optionally, images can also have tags. Think of the name as the specialized image name (such as `node` for node.js) and then the tag as the version (such as `14`). For example, to build a new image with a custome `name:tag`, just do `docker build -t image_name:image_tag image_ID`.

## Volumes
the `-v` flag passed to docker run creates a named volume if name is specified before `:` as in `docker run -v named_volume:\path\to\volume\on\container some_docker_image`, or as an *anonymous volume* if no name is specified as in  `docker run -v \path\to\volume\on\container some_docker_image`

# Networking    
`host.docker.internal` for all internal (localhost) IP addresses

# Cheat Sheet!
Check out the [Docker Cheat Sheet](./Cheat-Sheet-Images-Containers.pdf)!

