## Container
`docker container diff <container id>`

### Run
`docker container run --help`

`docker container run <repository>/<image>:<tag>`


`docker container run --rm -dit -name alpine1 --network alpine-net alpine ash` (registry=Docker Store, repository=library, tag=latest)

`docker attach alpine1` (detach by Ctrl-p Ctrl-q)

`docker container stop alpine1`

### List
* Active: `docker container ls`
* All: `docker container ls -a`

### Start
`docker container start <container id>`

`docker container exec <ontainer id> ls -l`

## Image
`docker image pull alpine`

`docker image inspect alpine`

`docker image ls`

### Create from container
`docker container commit <container id>`

`docker image ls`

`docker image tag <image id> <deisred name>`

`docker image history <image id>`

## Dockerfile
`docker image build -t hello:v0.1`

`docker image inspect --format "{{ json .RootFS.Layers }}" <image id>`

## Network
`docker network ls`

`docker network inspect bridge`

`docker network create --driver bridge alpine-net

`docker run -dit --name alpine1 --network alpine-net alpine ash`

`docker network connect bridge alpine1`

`docker network disconnect alpine-net alpine1`

`docker network rm alpine net`

## Frequent
* Remove all stopped containers: `docker rm $(docker ps -a -q)`
* Remove all images:`docker rmi $(docker images -q) --force`
