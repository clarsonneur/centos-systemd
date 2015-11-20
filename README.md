# centos-systemd
Docker build image for Centos with systemd running.

The intent is to build a Base image for other images that requires systemd capabilities.

From my point of view, systemd is not really a great option for docker, while thinking docker as micro service enabler.
So, if you can avoid it, do avoid it. Why? systemd is well designed for OS to boot/manage services, but from my point
of view is too heavy tool if you just need to start up a service, monitor and restart it on need.

Also, it requires docker privilege and access to cgroups fs to work.
As systemd manages services, in containers as well, you will be able to get status/logs/control to your service directly from systemd instead of docker.

Ex: docker run -it -v /sys/run/cgroups:/sys/run/cgroups --privileged clarsonneur/centos-systemd:7 systemctl status

If you want to keep in my the micro service proned by Docker, I would suggest you to limit your image to a single service.

# How to built you own image?

docker build -t YourTag .

# How to use it?

Usually, using it directly is not really useful, as no services are currently managed. So, the real interest is to create a new image from this one


Dockerfile example:

    FROM clarsonneur/centos-systemd:7
    
    # Install a new service to start
    RUN yum install httpd -y && yum clean all

    RUN systemctl enable http

# Note

This image is built from official centos image.
As I was mainly interested by centos 7, by default, this image will build the systemd image from official Centos 7 image.

The Dockerfile has been written thanks to write up found in internet:
 - http://developerblog.redhat.com/2014/05/05/running-systemd-within-docker-container/
 - http://jperrin.github.io/centos/2014/09/25/centos-docker-and-systemd/
