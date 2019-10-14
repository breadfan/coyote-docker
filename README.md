# Docker and docker compose based integration tests

This image combines 

* Docker
* Docker Compose and 
* Landoops Test tool coyote (https://github.com/lensesio/coyote) 

in one image to allow automated integration testing in CI with solutions like DIND (Docker in Docker)

## How to use it

The entrypoint of this image is the coyote test tool which needs the test description as a coyote.yml file.
Usually inside this yml you use docker-compose accompained by a docker-compose.yml which sets up your services which
should be tested.

So create a ```coyote.yml``` and a ```docker-compose.yml``` which constitute your tests and their environment and add
them to the volume path /integrationtests: 

```
FROM longliveenduro/coyote:master

COPY ./coyote.yml /integrationtests/
COPY ./docker-compose.yml /integrationtests/
```

Build a docker image from the above Dockerfile:
```
docker build -t <org>/<int-test-projectname>:<version> . 
```

To run the test e.g. locally you can do: 
```
docker run -v /var/run/docker.sock:/var/run/docker.sock <org>/<int-test-projectname>:<version>
```

You can build and run the above integration test image in your CI by using DIND or by handing over the docker socket like above.