# dind-images-lib

### Purpose
Daemonset which is intended to run on each node of dind cluster
Provides ready /var/lib/docker etalon folder with widely used public images for Codefresh Builds
So each codefresh/dind image can mount it to /var/lib/docker and avoid to pull all those images

### How it works
dind-images-lib docker container is built FROM docker:dind 
- `dockerd --data-root ${DOCKER_LIB_ETALON_DIR}`
- for each image listed in `./images` we do `docker pull `
- stoping dockerd

- loop to rsync ${DOCKER_LIB_ETALON_DIR} to DOCKER_LIB_{001..NNN} directories to have at least ${DESIRED_DOCKER_LIB_NUMBER} folders with pulled images
- to provided consistency it firstly pulls/copies to some temporary directory and then uses `mv` command

### How dind uses it
codefres/dind container with defined DOCKER_IMAGES_LIB env takes one of avalible DOCKER_LIB_{001..NNN} and `mv` it to be used as /var/lib/docker
On Sigterm dind deleted its DOCKER_LIB_NNN folder




 





