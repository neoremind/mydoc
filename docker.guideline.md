# Docker commands cheatsheet

 [Reference](https://docs.docker.com/engine/reference/commandline/)

## management

$sudo docker version

$sudo docker info

## Image

$sudo docker commit -a xu.zhang -m "xxx" {CONTAINER_ID} url:tagname

$sudo docker images

$sudo docker rmi {image_name}  //delete images

## Container

$sudo docker cp XXX.txt {CONTAINER_ID}:/home/product/path  //copy files to docker container

$sudo docker stop|start {CONTAINER_ID}

$sudo docker ps -a

$sudo docker run -t -i --name my-batch-job {IMAGE_ID} /bin/bash  //交互式新建container

$sudo docker run --name my-batch-job -d 77d3df7f5b2f /bin/sh -c "while true; do ping 8.8.8.8; sleep 10; done" //后台backgroud运行container

$docker exec -it my-batch-job /bin/bash  //进入刚刚后台运行的docker

## Hub and registry

$sudo docker push url/batch-job:xuzhang

$sudo docker pull url/batch-job:xuzhang



