## COMMANDS

clean volumes not used:
docker volume rm $(docker volume ls -qf dangling=true)

clean images not used anymore and not reference by any container:
docker image prune --all


## DOCKER CONTAINERS

A Docker container is similar to a virtual machine. It basically allows you to run a pre-packaged "Linux box" inside a container. The main difference between a Docker container and a typical virtual machine is that Docker is not quite as isolated from the surrounding environment as a normal virtual machine would be. A Docker container shares the Linux kernel with the host operating system, which means it doesn't need to "boot" the way a virtual machine would.

Since so much is shared, firing up a Docker container is a quick and cheap operation — in most cases you can bring up a full Docker container (the equivalent of a normal virtual machine) in the same time as it would take to run a normal command line program. This is great because it makes deploying complex systems a much easier and more modular process


## Docker images

Docker images are the basis of containers.For simplicity, you can think of an image akin to a git repository - images can be committed with changes and have multiple versions. When you do not provide a specific version number, the client defaults to latest.

An important distinction with regard to images is between base images and child images.

  * Base images are images that have no parent images, usually images with an OS like ubuntu, alpine or debian.

  * Child images are images that build on base images and add additional functionality.

To get a new Docker image you can either get it from a registry (such as the Docker Hub) or create your own. There are hundreds of thousands of images available on Docker hub. You can also search for images directly from the command line using docker search.

To see the list of images that are available locally on your system, run the docker images command.The TAG refers to a particular snapshot of the image and the ID is the corresponding unique identifier for that image.

For example you could pull a specific version of ubuntu image as follows:

$ docker pull ubuntu:12.04


## DOCKER VOLUMES

Quite simply, volumes are directories (or files) that are outside of the default Union File System and exist as normal directories and files on the host filesystem.

There are three main use cases for Docker data volumes:

To keep data around when a container is removed
To share data between the host filesystem and the Docker container
To share data with other Docker containers

A container's volumes are just directories on the host regardless of what method they are created by. If you don't specify a directory on the host, Docker will create a new directory for the volume, normally under /var/lib/docker/vfs.

However the volume was created, it's easy to find where it is on the host by using the docker inspect command e.g:

$ ID=$(docker run -d -v /data debian echo "Data container")
$ docker inspect -f {{.Mounts}} $ID
[{0d7adb21591798357ac1e140735150192903daf3de775105c18149552a26f951 /var/lib/docker/volumes/0d7adb21591798357ac1e140735150192903daf3de775105c18149552a26f951/_data /data local  true }]
We can see that Docker has created a directory for the volume at /var/lib/docker/volumes/0d7adb21591798357ac1e140735150192903daf3de775105c18149552a26f951/_data.

You are free to modify/add/delete files in this directory from the host, but note that you may need to use sudo for permissions.

Docker will only delete volume directories in two circumstances:

If the --rm option is given to docker run, any volumes will be deleted when the container exits
If a container is deleted with docker rm -v CONTAINER, any volumes will be removed.
In both cases, volumes will only be deleted if no other containers refer to them. Volumes mapped to specific host directories (the -v HOST_DIR:CON_DIR syntax) are never deleted by Docker. However, if you remove the container for a volume, the naming scheme means you will have a hard time figuring out which directory contains the volume.



docker run -v /home/adrian/data:/data debian ls /data

Will mount the directory /home/adrian/data on the host as /data inside the container. Any files already existing in the /home/adrian/data directory will be available inside the container. This is very useful for sharing files between the host and the container, for example mounting source code to be compiled.


Some Notes

Some behaviour not particularly well explained in the doc:

If you create a named volume by running a new container from image by docker run -v my-precious-data:/data imageName, the data within the image/container under /data will be copied into the named volume.
If you create another container binds to an existing named volume, no files from the new image/container will be copied/overwritten, it will use the existing data inside the named volume.
They don’t have a docker command to backup / export a named volume. However you can find out the actual location of the file by “docker volume inspect [volume-name]”.
A more fancy way to backup/restore a named volume is to run a container (could be from a simple debian:jessie image) that binds to the named volume and run a command to export to a host directory / export to the cloud. Then later import by the same method.Backup:
1
$ docker run --rm --volumes-from dbstore -v $(pwd):/backup ubuntu tar cvf /backup

#### example sharing data host and containers

docker run -d -v ~/nginxlogs:/var/log/nginx -p 5000:80 -i nginx

 -v ~/nginxlogs:/var/log/nginx — We set up a volume that links the /var/log/nginx directory from inside the Nginx container to the ~/nginxlogs directory on the host machine. Docker uses a : to split the host's path from the container path, and the host path always comes first.

 -d — Detach the process and run in the background. Otherwise, we would just be watching an empty Nginx prompt and wouldn't be able to use this terminal until we killed Nginx.

 -p 5000:80 — Setup a port forward. The Nginx container is listening on port 80 by default, and this maps the Nginx container's port 80 to port 5000 on the host system.



* [Understanding docker volumes](http://container-solutions.com/understanding-volumes-docker/)
* [https://madcoda.com/2016/03/docker-named-volume-explained/](https://madcoda.com/2016/03/docker-named-volume-explained/)
* [https://www.digitalocean.com/community/tutorials/how-to-work-with-docker-data-volumes-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-work-with-docker-data-volumes-on-ubuntu-14-04)

## EXAMPLES


docker run --name static-site -e AUTHOR="Your Name" -d -P dockersamples/static-site

In the above command:

-d will create a container with the process detached from our terminal
-P will publish all the exposed container ports to random ports on the Docker host
-e is how you pass environment variables to the container
--name allows you to specify a container name
AUTHOR is the environment variable name and Your Name is the value that you can pass



# Resources

* [Docker labs repo](https://github.com/docker/labs/)
