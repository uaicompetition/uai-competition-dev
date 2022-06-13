---
title: "Competition Entry"
permalink: /competition-entry/requirements/
---

## Solver Requirements


### Execution Environment

* The evaluations will be run on Linux machines up to **8 GB memory** made available.

* We request that solvers be Docker images that containerize executables using only **a single core (i.e., no parallel processes/threads)**.

* Each submission will be ran using [Singularity](https://sylabs.io/guides/2.6/user-guide/index.html) 
with restrictied computing resource per image as described in Tasks description.


### Containerizing Solvers in Docker
We will accept solver submissions in Docker images. Participants have two options

1. create a Docker project and upload its image to [Dockerhub](https://hub.docker.com/). 

Then, submit Dockerhub repo `<ID>/<REPO>` so that we can
  * pull the image `$ docker pull <ID>/<REPO>:latest` or
  * build singularity image  
 
```
$ singularity build --bind <HOST_FOLDER>:<IMAGE_FOLDER> <SOLVERNAME>.sif --writable docker://<ID>/<REPO>
```

2. Directly send a Docker project (Not image) with all necessary local files to build image

Then, Docker image will be built locally using the project by
```
$ docker build -t <ID>:<NAME> . 
```


### How to Containerizing Solvers in Docker
Containerizing solver as a Docker image will allow participants to use any working environment 
independ to the CentOS-based cluster environment.

Each image should be equipped with an executable and a folder to mount host directory during evaluation.
While running the image, the name of the files will be passed as environment variables 
so the image can execute a bash command with the environment variables.


### Example Docker projects
Please check out a few example project that will be listed below
and don't hesitate to contact oragnizers if any help or clarification neeeded.
* https://github.com/uaicompetition/docker-merlin-cpp: build executable from source
* https://github.com/uaicompetition/docker-merlin-cpp/tree/merlinbin: copy statically compiled executable inside image and define environment variables


### Environment Variables 
We will use **environment variables** to pass the path to the input and output files.
* `MODEL`, `EVID`, `QUERY`, `RESULT` environment variables are the name of each file
* `QUERY` filename is only relevant to the marginal MAP task, so solvers working on the other three tasks should ignore this file.
* a solver is expected to run the command ``` $ ./solver $MODEL $EVID $QUERY $RESULT ```

The formats are described here:
* [Model Format](../file-formats/model-format.md)   
* [Evidence Format](../file-formats/evidence-format.md)
* [Query variables](../file-formats/query-format.md) (only applies to the marginal MAP task)
* [Result format](../file-formats/result-format.md)
   
