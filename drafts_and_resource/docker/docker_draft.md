# lynda c++
#include <iostream>
using namespace std;
int main()
{
  string name;
  cout << "please enter your name: " <<endl;
  cin >> name;
  cout << "Hello " << name <<endl;
}


/**
 * datatype
 **/

<datatype> name;
int myvar; 4bytes
float mySalary;
double salaryPerMonth;
char MiddleInitial = "A";
string firstName = "Tom";
bool isValid = TRUE; 1 bytes;

// docker
/***
 * docker
 *
 * */
installing docker toolbox in mac

docker info

docker run -ti ubuntu bash

## docker for windows
make sure virtualization is enabled
if installation failed uninstall the virtual box and let docker install virtual box for you
after install docker there will be a shortcut in desktop. if I click on it will install some ** and start docker
If I want same environment like others enviornment I have to use net by following procedure

docker run --net=host -ti ubuntu:14:04 bash


## docker in linux

from documentations

## command
sudo docker run hello-world

create docker group in linux machine for calling `docker` instead of `sudo docker`


docker run -ti debian bash

# Using docker

## the docker flow images to container
ti stands for terminal interactive
~~~
docker images
docker run -ti ubuntu:latest bash
// latest tag is optional
~~~


 to exit
~~~
exit
or
ctrl + d
~~~

### to see the running images
~~~
docker ps
~~~
or personal preferences

~~~
docker ps --format $FORMAT // to see accordance your choice
~~~

## the docker flow: containers to images


### for showing all containers (including stopped containers)
~~~
docker ps -a
~~~

### for showing last closed containers details

~~~
docker ps -l --format=$FORMAT
~~~


~~~
[images] -> (docker run) -> [running container]
                                ↓
[New Image] <--(docker commit) <-[Stopped container]
~~~

### workflow
~~~
// starting a image
docker run -ti ubuntu bash
// create a file
touch my-important file
exit
// getting last closed containers info. where I will get container id
docker ps -l --format=$FORMAT

// docker commit give us a image id
docker commit <containerId>

// keep name of the newly created image
docker tag <iamgeId> <my-image>
// showing all images
docker images
// showing my image

docker run -ti my-images bash

// container commit and image tag in single comment
docker commit <container_name> <my-image-2>
~~~

## Running things in Docker


### delete container after run

~~~
docker run --rm ti ubuntu sleep 5
docker run -ti ubuntu bash -c "sleep 3; echo all done"
~~~


### run docker in background


~~~
// -d for detach
docker run -d -ti ubuntu bash
// find your container info
docker ps --format=$FORMAT
// now attch your container
docker attach <names of running containers>

~~~

special sequence for detach docker

~~~
(ctrl + p) + (ctrl+q) (sequence)
~~~

### running more things using `docker exec` command

~~~
docker --exec -ti <containerName> bash
~~~

## manage containers

`docker logs` keeps the output of containers. To see output `docker logs <containerName>`

~~~
// wrong command make it crash
docker run --name example -d ubuntu bash -c "lose /etc/password"
// to know the logs
docker logs example
// output: lost: command not found
~~~

### some rule / lesson from exeperience
* don't let the output get too large
* Don't let your container fetch dependencies when they start
* Don't leave important things in unnamed stopped containers - you may delete mistakenly

### kill and rm
kill will stop container rm will delete container

~~~
docker kill <containerName>
docker rm <containerName>
~~~

memory & cpu limits

~~~
docker run --memory maximum-allowed-memory image-name command
docker run --cpu-shares (relative to other containers)
docker run --cpu-quota (to limit it in general- its a hard value)
~~~



































