---
title: "Competition Entry"
permalink: /competition-entry/requirements/
---

## Solver Requirements


### Execution Environment

* The evaluations will be run on Linux machines with up to **8 GB memory** made available.

* We request that solvers be submitted as Docker images that containerize executables using only **a single core (i.e., no parallel processes/threads)**.

* Each submission will be evaluated using [Singularity](https://sylabs.io/guides/2.6/user-guide/index.html) 
with restrictied computing resource per image as described in the [Tasks](./tasks.md) descriptions.


### Containerizing Solvers in Docker
We will be accepting submissions as Docker images. Participants have two options:

1. _(PREFERRED)_ Create a Docker project and upload its image to [Dockerhub](https://hub.docker.com/).
    
    You will submit the Dockerhub repository `<ID>/<REPO>` tags so that we can:
      * pull the image `$ docker pull <ID>/<REPO>:latest` or
      * build singularity image  
 
    ```
    $ singularity build --bind <HOST_FOLDER>:<IMAGE_FOLDER> <SOLVERNAME>.sif --writable docker://<ID>/<REPO>
    ```

2. Directly submit a Docker project (as opposed to the already built image) with all necessary local files to build an image

    Then a Docker image will be built locally using the command:
    ```
    $ docker build -t <ID>:<NAME> . 
    ```


### How to Containerize Solvers in Docker
Solvers containerized as a Docker image will allow use of any working environment seamlessly,
indepent to the CentOS-based cluster environment.

Each image should be equipped with an executable and a folder to mount the host directory during evaluation.
While running the image, the names of the files will be passed as environment variables (see section below on
Environment Variables) so the image can execute bash commands with them.


### Example Docker projects
Please see the following example projects
and don't hesitate to contact oragnizers if any help or clarification neeeded.
* https://github.com/uaicompetition/docker-merlin-cpp: build executable from source
* https://github.com/uaicompetition/docker-merlin-cpp/tree/merlinbin: copy statically compiled executable inside image and define environment variables


### Environment Variables 
We will use **environment variables** to pass the path of the input and output files.
* `MODEL`, `EVID`, `QUERY`, `RESULT` are the environment variables that will be used to pass the path of their respective files.
* `QUERY` filename is only relevant to the marginal MAP task, so solvers working on the other three tasks should ignore this file.
* a solver is expected to run the command ``` $ ./solver $MODEL $EVID $QUERY $RESULT ```

The file formats are described here:
* [Model Format](../file-formats/model-format.md)   
* [Evidence Format](../file-formats/evidence-format.md)
* [Query variables](../file-formats/query-format.md) (only applies to the marginal MAP task)
* [Result format](../file-formats/result-format.md)
   
