# Docker-compose Project Name Param Reduced Test Case

## The Issue

Using the `-p` or `--project-name` prevents `docker-compose up` from running.

Per Docker-compose’s help documentation, emphasis mine:

<pre><code>$ docker-compose —help
Define and run multi-container applications with Docker.

Usage:
  docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE             Specify an alternate compose file
                              (default: docker-compose.yml)
  <b>-p, --project-name NAME     Specify an alternate project name
                              (default: directory name)</b>
  --verbose                   Show more output
  --log-level LEVEL           Set log level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
  --no-ansi                   Do not print ANSI control characters
  -v, --version               Print version and exit
  -H, --host HOST             Daemon socket to connect to

  --tls                       Use TLS; implied by --tlsverify
  --tlscacert CA_PATH         Trust certs signed only by this CA
  --tlscert CLIENT_CERT_PATH  Path to TLS certificate file
  --tlskey TLS_KEY_PATH       Path to TLS key file
  --tlsverify                 Use TLS and verify the remote
  --skip-hostname-check       Don't check the daemon's hostname against the
                              name specified in the client certificate
  --project-directory PATH    Specify an alternate working directory
                              (default: the path of the Compose file)
  --compatibility             If set, Compose will attempt to convert keys
                              in v3 files to their non-Swarm equivalent
  --env-file PATH             Specify an alternate environment file

Commands:
  build              Build or rebuild services
  config             Validate and view the Compose file
  create             Create services
  down               Stop and remove containers, networks, images, and volumes
  events             Receive real time events from containers
  exec               Execute a command in a running container
  help               Get help on a command
  images             List images
  kill               Kill containers
  logs               View output from containers
  pause              Pause services
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pull service images
  push               Push service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  top                Display the running processes
  unpause            Unpause services
  up                 Create and start containers
  version            Show the Docker-Compose version information
</pre></code>

## Duplicating

- Clone this repo
- At the root level, run `docker-compose up`. Works as expected:

```bash
$ docker-compose up
Starting docker-compose-reduced-test-case_hello_1 ... done
Attaching to docker-compose-reduced-test-case_hello_1
hello_1  |
hello_1  | Hello from Docker!
hello_1  | This message shows that your installation appears to be working correctly.
hello_1  |
hello_1  | To generate this message, Docker took the following steps:
hello_1  |  1. The Docker client contacted the Docker daemon.
hello_1  |  2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
hello_1  |     (amd64)
hello_1  |  3. The Docker daemon created a new container from that image which runs the
hello_1  |     executable that produces the output you are currently reading.
hello_1  |  4. The Docker daemon streamed that output to the Docker client, which sent it
hello_1  |     to your terminal.
hello_1  |
hello_1  | To try something more ambitious, you can run an Ubuntu container with:
hello_1  |  $ docker run -it ubuntu bash
hello_1  |
hello_1  | Share images, automate workflows, and more with a free Docker ID:
hello_1  |  https://hub.docker.com/
hello_1  |
hello_1  | For more examples and ideas, visit:
hello_1  |  https://docs.docker.com/get-started/
hello_1  |
docker-compose-reduced-test-case_hello_1 exited with code 0
```

- Try running `docker-compose up --project-name foo` or `docker-compose up -p foo`. The output appears to be help context:

```bash
$ docker-compose up --project-name=hello
Builds, (re)creates, starts, and attaches to containers for a service.

Unless they are already running, this command also starts any linked services.

The `docker-compose up` command aggregates the output of each container. When
the command exits, all containers are stopped. Running `docker-compose up -d`
starts the containers in the background and leaves them running.

If there are existing containers for a service, and the service's configuration
or image was changed after the container's creation, `docker-compose up` picks
up the changes by stopping and recreating the containers (preserving mounted
volumes). To prevent Compose from picking up changes, use the `--no-recreate`
flag.

If you want to force Compose to stop and recreate all containers, use the
`--force-recreate` flag.

Usage: up [options] [--scale SERVICE=NUM...] [SERVICE...]

Options:
    -d, --detach               Detached mode: Run containers in the background,
                               print new container names. Incompatible with
                               --abort-on-container-exit.
    --no-color                 Produce monochrome output.
    --quiet-pull               Pull without printing progress information
    --no-deps                  Don't start linked services.
    --force-recreate           Recreate containers even if their configuration
                               and image haven't changed.
    --always-recreate-deps     Recreate dependent containers.
                               Incompatible with --no-recreate.
    --no-recreate              If containers already exist, don't recreate
                               them. Incompatible with --force-recreate and -V.
    --no-build                 Don't build an image, even if it's missing.
    --no-start                 Don't start the services after creating them.
    --build                    Build images before starting containers.
    --abort-on-container-exit  Stops all containers if any container was
                               stopped. Incompatible with -d.
    --attach-dependencies      Attach to dependent containers
    -t, --timeout TIMEOUT      Use this timeout in seconds for container
                               shutdown when attached or when containers are
                               already running. (default: 10)
    -V, --renew-anon-volumes   Recreate anonymous volumes instead of retrieving
                               data from the previous containers.
    --remove-orphans           Remove containers for services not defined
                               in the Compose file.
    --exit-code-from SERVICE   Return the exit code of the selected service
                               container. Implies --abort-on-container-exit.
    --scale SERVICE=NUM        Scale SERVICE to NUM instances. Overrides the
                               `scale` setting in the Compose file if present.
```
