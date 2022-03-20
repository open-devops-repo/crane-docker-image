crane-docker-image
====================

Introduction
------------
This projects builds a Docker image with [crane](https://github.com/google/go-containerregistry/tree/main/cmd/crane). The projects mirrows the Docker image ```gcr.io/go-containerregistry/crane:debug```
and republishes it on Dockerhub under ```opendevopsrepo/crane-debug:latest```.


Usage
-----

Example on flatten of a Docker image:

    #docker run --rm -it --entrypoint "/busybox/sh" gcr.io/go-containerregistry/crane:debug
    docker run --rm -it --entrypoint "/busybox/sh"opendevopsrepo/crane-debug:latest

    #crane auth login -u USER -p PW -v index.docker.io  # for dockerhub
    crane auth login -u USER -p PW -v REPOSERVER
    crane cp MYREPO/foo:latest MYREPO/foo-flat:latest
    crane flatten MYREPO/foo-flat:latest

Why can a flatten of an image be useful, beside of a potential reduced Docker image size?
Flatten of an image can be useful if you use [Kaniko](https://github.com/GoogleContainerTools/kaniko) and [Cloud Foundry](https://www.cloudfoundry.org/),
because Docker images built with Kaniko can fail to run in Cloud Foundry because of a [Kaniko Bug with CF #1149](https://github.com/GoogleContainerTools/kaniko/issues/1149).
This can also not be simply fixed by a multi-stage Dockerfile, because of a [Kaniko Multistage COPY failure #1210](https://github.com/GoogleContainerTools/kaniko/issues/1210).

Docs:
* https://github.com/google/go-containerregistry/tree/main/cmd/crane
* https://github.com/google/go-containerregistry/tree/main/cmd/crane/doc/


How to fork this repo and its automatic CI builds
-------------------------------------------------
* have a GitHub.com and a hub.docker.com account ready
* fork this git repo into your own GitHub.com repo
* create an hub.docker.com access token (https://hub.docker.com/settings/security)
* set this hub.docker.com access token in your GitHub.com repo Settings/Security/Secrets/Actions (https://github.com/your-GitHub.com-username/your-GitHub.com-repo-name/settings/secrets/actions)
    * set DOCKERHUB_USERNAME={your hub.docker.com user name}
    * set DOCKERHUB_TOKEN={your hub.docker.com access token from above}
* in .github/workflows/github-actions.yml replace tag "opendevopsrepo/jenkins-plugins-collection" with "{your hub.docker.com username}/{your new hub.docker.com repo name}"
* set a Readme text on hub.docker.com (https://hub.docker.com/repository/docker/your-hub.docker.com-username/your-new-hub.docker.com-repo-name/general)

After this setup, every push to "main" branch in your GitHub.com repo should trigger a CI build that uploads the docker image to your hub.docker.com repo.

