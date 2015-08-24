# Docker Proxy-gen

proxy-gen sets up a container running nginx and [docker-gen].
docker-gen generates a configs for nginx and reloads it when containers are
started and stopped.
The nginx config forward traffic to the dedicated container based on the
`hostname`.

### Usage

    $ docker run -d -v /var/run/docker.sock:/var/run/docker.sock jderusse/proxy-gen

That is. You can now start yours containers and requesting it

    $ docker run --name my_app -d nginx
    $ curl my_app.docker

To automaticaly get the dns we recommand to use `jderusse/dns-gen`

You can customize the HOST name suffix by providing an env var `HOST=subdomain.youdomain.com`

    $ docker run -d -e HOST=foo.docker -v /var/run/docker.sock:/var/run/docker.sock jderusse/proxy-gen
    $ docker run --name my_app -d nginx
    $ curl my_app.foo.docker

If you use docker_compose, you'd notice that containers are named with the
following format `project_service_number`. You can simplify the full host name
by telling proxy-gen to remove project or service from the host name

    $ docker run -d -v /var/run/docker.sock:/var/run/docker.sock jderusse/proxy-gen
    $ cd my_project
    $ docker-composer up -d
    $ curl my_project_app.docker

or

    $ docker run -d -e SKIP_COMPOSE_PROJECT=1 -v /var/run/docker.sock:/var/run/docker.sock jderusse/proxy-gen
    $ docker-composer up -d
    $ curl app.docker

or

    $ docker run -d -e SKIP_COMPOSE_SERVICE=1 -v /var/run/docker.sock:/var/run/docker.sock jderusse/proxy-gen
    $ cd my_project
    $ docker-composer up -d
    $ curl my_project.docker
