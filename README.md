# Docker container for Jenkins with docker inside - Tested on Debian

[Jenkins](https://www.jenkins.io/) is an open source automation server which enables developers around the world to reliably build, test, and deploy their software.

## How to use

### Build

Container defaults UID=1000. You can override it at build stage.

```bash
docker build -t jenkins-w-docker --build-arg UID=$(id -u) .
```

### Simple start

```bash
docker run -p 8080:8080 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    --group-add=$(stat -c %g /var/run/docker.sock) \
    -v $(pwd)/jenkins_home:/var/jenkins_home \
    --name jenkins \
    jenkins-w-docker
```

## Parameters explained

### Docker socket

To be able to connect to the docker daemon, the container needs access to the docker socket.

```bash
    -v /var/run/docker.sock:/var/run/docker.sock
    --group-add=$(stat -c %g /var/run/docker.sock)
```

### Jenkins Home

All Jenkins data lives in `/var/jenkins_home` including plugins and configuration. You will probably want to make that an explicit volume so you can manage it and attach to another container for upgrades that will survive the container stop/restart/deletion.

```bash
    -v $(pwd)/jenkins_home:/var/jenkins_home
```
