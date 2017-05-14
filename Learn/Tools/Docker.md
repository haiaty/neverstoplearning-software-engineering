

## Docker images

Docker images are the basis of containers.For simplicity, you can think of an image akin to a git repository - images can be committed with changes and have multiple versions. When you do not provide a specific version number, the client defaults to latest.

An important distinction with regard to images is between base images and child images.

  * Base images are images that have no parent images, usually images with an OS like ubuntu, alpine or debian.

  * Child images are images that build on base images and add additional functionality.

To get a new Docker image you can either get it from a registry (such as the Docker Hub) or create your own. There are hundreds of thousands of images available on Docker hub. You can also search for images directly from the command line using docker search.

To see the list of images that are available locally on your system, run the docker images command.The TAG refers to a particular snapshot of the image and the ID is the corresponding unique identifier for that image.

For example you could pull a specific version of ubuntu image as follows:

$ docker pull ubuntu:12.04


docker run --name static-site -e AUTHOR="Your Name" -d -P dockersamples/static-site

In the above command:

-d will create a container with the process detached from our terminal
-P will publish all the exposed container ports to random ports on the Docker host
-e is how you pass environment variables to the container
--name allows you to specify a container name
AUTHOR is the environment variable name and Your Name is the value that you can pass



# Resources

* [Docker labs repo](https://github.com/docker/labs/)
